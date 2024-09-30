# Assignment 7.4: Bike Repair Shop Project - RxJS

Now that we know more about RxJS we can apply this knowledge to our bike repair shop project.

First we need to install RxJS. We can do this by running the following command in the terminal:

```bash
npm install rxjs
```

Next we start the refactoring of our code at the `repair-service` file. We need to think about what we want to communicate between the `repair-service` and the `repair-controller`. If we look at the CRUD methods in the `repair-service` we see that they all (except for the `getRepairAssignment` method) return a `Promise`, that resolves in an array of repair assignments.

So adding an observable to the `repair-service` that emits the repair assignments when they are changed is a good idea. We can then subscribe to this observable in the `repair-controller` and update the view when the repair assignments are changed.

Next we need to think about what kind of observable we want to use. Do we need a unicasting or multicasting observable? Do we need a hot or cold observable? Do we need a synchronous or asynchronous observable?

Since the their is only a single controller instance that uses the repair-service we can use a unicasting observable. But we don't know if their will be more controllers in the future that use the repair-service. So we could use a multicasting observable to be on the safe side.

And we want to initialize the system with the repair assignments that are already in the system. So we need a `BehaviorSubject` that emits the current repair assignments to new subscribers. And that will also emit the repair assignments when they are changed.

This results in the following code for the `/src/service/repair-service.js` file:

```javascript
import { BehaviorSubject } from 'rxjs';

class RepairService {

  repairAssignments$ = new BehaviorSubject([]);

  constructor() {
    const PORT = 3001;
    this.repairServer = `http://localhost:${PORT}/`;

    // initialize the repair assignments list
    this.getRepairsList().then((repairAssignments) => {
      this.repairAssignments$.next(repairAssignments);
    });
  }

  /**
   * Retrieves a list of all repair assignments from the database
   *
   * @returns {Promise<JSON Object>}
   */
  getRepairsList() {
    return fetch(`${this.repairServer}repairs`).then((response) =>
      response.json()
    );
  }

  /**
   * Stores a repair assignment in the database
   *
   * @param {JSON Object} repairAssignment
   * @returns {Promise<JSON Object>}
   */
  addRepair(repairAssignment) {
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(repairAssignment),
    };
    return fetch(`${this.repairServer}repairs`, options)
      .then((response) => response.json())
      .then((repairAssignments) => {
        // data has been updated, notify subscribers
        this.repairAssignments$.next(repairAssignments);
        return repairAssignments;
      });
  }


  /**
   * Retrieves a single repair assignment from the database
   *
   * @param {String} repairId
   * @returns {Promise<JSON Object>}
   */
  getRepairAssignment(repairId) {
    return fetch(`${this.repairServer}repairs/${repairId}`).then((response) =>
      response.json()
    );
  }

  /**
   * 
   * @param {String} repairId 
   * @param {JSON Object} repairAssignment 
   * @returns {Promise<JSON Object>}
   */
  updateRepairAssignment(repairId, repairAssignment) {
    const options = {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(repairAssignment),
    };
    return fetch(`${this.repairServer}repairs/${repairId}`, options)
      .then((response) => response.json())
      .then((repairAssignments) => {
        // data has been updated, notify subscribers
        this.repairAssignments$.next(repairAssignments);
      });
  }

  /**
   * Removes a repair assignment from the database
   *
   * @param {*} repairId
   * @returns
   */
  removeRepairAssignment(repairId) {
    const options = {
      method: 'DELETE',
    };
    return fetch(`${this.repairServer}repairs/${repairId}`, options)
      .then(() => {
        // data has been updated, notify subscribers
        this.getRepairsList().then((repairAssignments) => {
          this.repairAssignments$.next(repairAssignments);
        });
      });
  }
}

const repairService = new RepairService();

export { repairService };
```

Next we need to change the `/src/controller/repair-controller.js` file to subscribe to the `repairAssignments$` observable and update the view when the repair assignments are changed.

But we also want the view components to be able to react to changes in the repair assignments. So our `repair-controller` also needs to have a `BehaviorSubject` that emits the current repair assignments to new subscribers. And that will also emit the repair assignments when they are changed, which results in only a small change to the code.

```javascript
import { repairService } from '../service/repair-service';
import { BehaviorSubject } from 'rxjs';


class RepairController {

  repairAssignments$ = new BehaviorSubject([]);
  
  /**
   *
   * @param {RepairService Instance} repairServiceInstance
   */
  constructor(repairServiceInstance) {
    this.repairService = repairServiceInstance;

    this.repairService.repairAssignments$.subscribe((repairAssignments) => {
      this.repairAssignments$.next(repairAssignments);
    });
  }
  
  ...
```

Now we can make changes to the `todo-time` component, which didn't update when a repair assignment was delete. All we have to do is subscribe to the `repairAssignments$` observable in the `repair-controller` and update the view when the repair assignments are changed. Which will be archived by a single line of code within the connectedCallback method of the `todo-time` component.

```javascript
  connectedCallback() {
    super.connectedCallback();

    repairController.repairAssignments$.subscribe(this.updateRepairTime.bind(this));
  }
```

When we now run the bike repair shop project we see that the `todo-time` component updates when a repair assignment is added, updated or deleted. So everything works as expected.

But if we take a look at the code, you might wonder why the `todo-time` component subscribed to the `repair-controller` and not directly to the `repair-service`.
This is because the `repair-controller` is the only layer that should be aware of the `repair-service`. The `todo-time` component should not be aware of the `repair-service`. This is because the `repair-controller` is the layer that should be responsible for the communication between the `repair-service` and the `todo-time` component.
But taking a look at the Observable at the `repair-controller` we don't see that it takes any responsibility. It just passes the data from the `repair-service` to the `todo-time` component. This is not a good practice. The `repair-controller` should take responsibility for the data that is passed every data change to the components that are subscribed to it.

So let's change that. If we ask ourself what the todo-time want needs to know from the controller it's the totalRepairTime, that it stores in the property `totalTodoTime`. So instead of letting the `todo-time` component invoke a call to the `repair-controller` to get the `totalRepairTime` we can let the `repair-controller` offer that value as an observable.

This results in the following code for the `/src/controller/repair-controller.js` file:

```javascript
import { repairService } from '../service/repair-service';
import { BehaviorSubject } from 'rxjs';

const INITIAL_TOTAL_REPAIR_TIME = 0;

class RepairController {

  repairAssignments$ = new BehaviorSubject([]);
  totalRepairTime$ = new BehaviorSubject(INITIAL_TOTAL_REPAIR_TIME);
  
  /**
   *
   * @param {RepairService Instance} repairServiceInstance
   */
  constructor(repairServiceInstance) {
    this.repairService = repairServiceInstance;

    this.repairService.repairAssignments$.subscribe((repairAssignments) => {
      this.repairAssignments$.next(repairAssignments);
      
      this.getTotalRepairTime().then((totalRepairTime) => {
        this.totalRepairTime$.next(totalRepairTime);
      });
    });
  }
...
```

This means that our `todo-time` component no longer needs the `updateRepairTime` method since it will get the `totalRepairTime` from the `repair-controller` by subscribing to the `totalRepairTime$` observable. So the whole `todo-time` component will look like this:

```javascript
import { LitElement, html, css } from 'lit';
import { repairController } from '../../controller/repair-controller';

export class TodoTime extends LitElement {
  static styles = css`
    :host {
      display: block;
      top: 1rem;
      right: 1rem;
    }

    p {
      font-size: 0.9rem;
      text-align: right;
    }
  `;
  static properties = {
    totalTodoTime: { type: Number },
  };

  constructor() {
    super();
    this.totalTodoTime = 0;
  }

  connectedCallback() {
    super.connectedCallback();

    repairController.totalRepairTime$.subscribe((totalRepairTime) => {
      this.totalTodoTime = totalRepairTime;
    });
  }

  render() {
    return html`
      <p>Expected repairtime for all bikes: ${this.totalTodoTime}</p>
    `;
  }
}

customElements.define('todo-time', TodoTime);
```

And that is all we need to do to make the `todo-time` component work with the `repair-controller` and the `repair-service`.

Now we can refactor the other components in the same way. We can let the `repair-controller` offer the data that the components need as observables. This way the components don't need to know about the `repair-service` and the `repair-controller` can take responsibility for the data that is passed every data change to the components that are subscribed to it.

The `repairassignment-list` component is another good candidate for this refactoring. It needs the repair assignments to show them in a list. Our `repair-controller` already offers the repair assignments as an observable. So we only need to subscribe to the `repairAssignments$` observable in the `repairassignment-list` component and update the view when the repair assignments are changed. Resulting in only a small change to the connectedCallback method of the `repairassignment-list` component.

```javascript
  connectedCallback() {
    super.connectedCallback();
    repairController.repairAssignments$.subscribe((repairAssignments) => {
      this.repairs = repairAssignments;
    });
  }
```

> [!WARNING]
It now looks like everything works fine, but when you go on with this site you are highly likely to run into problems. This is because we have forgotten to unsubscribe from the observables in our view components. This will cause errors when your data changes and the view components that are listening to those changes are not in the DOM anymore. So please apply the unsubscribe in the disconnectedCallback method of your view components, as we did in [assignment 7.1](./assignment7.1.md).

This concludes the refactoring of the bike repair shop project to use RxJS. We now have a more reactive system that is easier to maintain and extend.

But there is still a problem that we haven't addressed yet. To demonstrate this problem you have to open a second browser window or tab and navigate to the bike repair shop project. When you add, update or delete a repair assignment in one window or tab you will see that the other window or tab doesn't update. This is because the each window/tab has its own instance of the `repair-controller` and `repair-service`. So the changes in one window/tab are not propagated to the other window/tab. But their is no event emitting from the backend to the frontends when a change to the database is made.

We could active polling (sending a request to the server every x seconds) to solve this problem. But this is not a good solution because it is not efficient, since it might cause a high CPU load as well as a lot of network calls to the backend. A better solution is to use [WebSockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket). WebSockets are a communication protocol that provides full-duplex communication channels over a single TCP connection. This means that the server can push data to the client without the client having to request it.
We could also use [EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource), which is a simpler way to receive updates from a server.
But EventSource as well as WebSockets require that the backend supports it. And our backend doesn't support it.

Would we have used Firestore database of [Firebase](https://firebase.google.com/) as our backend, we could have subscribed to the Firestore database and received updates in real-time, like we did in this RxJS assignment.

But since this is a frontend subject we will not investigate this further. But it is good to know that there are solutions for this problem.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 7.3](./assignment7.3.md) | :arrow_up: [Assignments](./README.md)
