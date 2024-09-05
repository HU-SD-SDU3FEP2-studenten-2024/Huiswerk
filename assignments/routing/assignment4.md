# Assignment 4: Architectural Layers

In order to have a clear separation of concerns in our application we will apply the architectural layers of our application. We will create a `service`, `controller` and `view` layer.

The responsibility of the `service` layer is to know how to store and retrieve data. Data can be stored in the browser (for instance within the localstorage) or at the backend.

The responsibility of the `controller` layer is to handle the business logic of the application. It will call the `service` layer to store and retrieve data.

The responsibility of the `view` layer is to display the data to the user. It will call the `controller` layer to retrieve the data.

Lets start with the `service` layer.

## Service Layer

First we need a new folder `service` in the `src` folder. In this folder we hold all our services.
The first service we will create is the `repair-service.js`.
Since we are working on the intake user story our service only needs a method to post a new repair to the backend.

```javascript
class RepairService {

  constructor() {
    const PORT = 3001;
    this.repairServer = `http://localhost:${PORT}/`;
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
    }
    return fetch(`${this.repairServer}repairs`, options)
      .then(response => response.json());
  }
}

const repairService = new RepairService();

export { repairService };

```

In the `repair-service.js` file we create a class `RepairService`.
In the constructor we set the `repairServer` property to the url of our `json-server` (check that the port is the same as the one used in the json-server start script).

We create a method `addRepair` that takes a `repairAssignment` as a parameter. This method will call the API of the backend to store the `repairAssignment` in the database. The method returns a `Promise` that will resolve to a `JSON Object`. The resolved `JSON Object` will be the `repairAssignment` that was stored in the database. And therefore it will have a unique `id` property (feature of the json-server).

To make sure that we only have one instance of the `RepairService` we create a `repairService` object and export it (singleton pattern).

## Controller Layer

Next we need a new folder `controller` in the `src` folder. In this folder we hold all our controllers.
We could approach this problem in two ways. We could create a `repair-controller.js` that holds all the methods that are related to the repairs. Or we could create a `intake-controller.js` that holds all the methods to handle the intake user story. Since we are working on the intake user story we will create the `intake-controller.js`.

```javascript
import { repairService } from '../service/repair-service';

class IntakeController {

  /**
   * 
   * @param {RepairService Instance} repairServiceInstance
   */
  constructor(repairServiceInstance) {
    this.repairService = repairServiceInstance;
  }

  /**
   * 
   * @param {JSON Object} repairAssignment 
   * @returns {Promise<JSON Object>}
   */
  addRepair(repairAssignment) {
    return this.repairService.addRepair(repairAssignment);
  }
}

const intakeController = new IntakeController(repairService);

export { intakeController };
```

Our IntakeController holds a `repairService` property that is set in the constructor. We create a method `addRepair` that takes a `repairAssignment` as a parameter. This method will call the `addRepair` method of the `repairService` and return the `Promise` that the `addRepair` method of the `repairService` returns.

Because the server already made sure that the `repairAssignment` has a unique `id` property we don't need to do anything regarding that business rule and just have to return resolved `JSON Object` in the `addRepair` method of the `IntakeController`.

In the next assignment we will create the `view` layer.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 3](./assignment3.md) | :arrow_up: [Assignments](./README.md) | [Assignment 5](./assignment5.md) :arrow_forward:
