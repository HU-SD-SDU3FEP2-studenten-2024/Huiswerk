# Assignment 7: Advanced Routing

In this assignment we will refactor the application of assignment 6 to demonstrate some advanced routing features.

## Dynamic Routing

Dynamic Routing is a feature that allows us to define routes that are parameterized. This means that we can define a route that contains a placeholder for a value that will be passed to the route handler. For example, we can define a route `/repairassignment/:id` that will match `/repairassignment/1`, `/repairassignment/2`, etc. and pass the value of `id` to the route handler.

To demonstrate this we first create a new component `repair-assignment.js` in the directory `/src/view/components` That will replace the dialog of the `repairassignment-list-item`.:

```javascript
import { LitElement, html, css } from 'lit';
import { Router } from '@vaadin/router';
import { dashboardController } from '../../controller/dashboard-controller';

export class RepairAssignment extends LitElement {
  static styles = css`
    :host {
      background-color: beige;
      border-radius: 0.25rem;

      padding: 1rem;
    }

    article {
      display: grid;
      gap: 1rem;
      grid-template-columns: auto 1fr;
    }

    .actions {
      padding: 1.5rem;
      display: grid;
      gap: 1rem;
      grid-template-columns: 1fr 1fr;
    }

    button {
      background-color: #007bff;
      color: white;
      border: 0;
      padding: 0.5rem 1rem;
      border-radius: 0.25rem;
    }

    .remove {
      background-color: #dc3545;
    }
  `;

  static properties = {
    id: { type: String },
    name: { type: String, attribute: false },
    email: { type: String, attribute: false },
    phone: { type: String, attribute: false },
    repairassignment: { type: String, attribute: false },
  };

  constructor() {
    super();
    this.id = '-1';
    this.name = '';
    this.email = '';
    this.phone = '';
    this.repairassignment = '';
  }

  connectedCallback() {
    super.connectedCallback();
    dashboardController.getRepairAssignment(this.id).then((repair) => {
      this.name = repair.name;
      this.email = repair.email;
      this.phone = repair.phone;
      this.repairassignment = repair.repairassignment;
    });
  }

  removeAssignment() {
    dashboardController.removeRepairAssignment(this.id).then(() => {
      this.remove();
      Router.go(import.meta.env.BASE_URL);
    });
  }

  // eslint-disable-next-line class-methods-use-this
  backToDashboard() {
    Router.go(import.meta.env.BASE_URL);
  }

  render() {
    return html`
      <article>
        <label for="id">ID</label><span id="id">${this.id}</span>
        <label for="name">Name</label><span id="name">${this.name}</span>
        <label for="email">Email</label><span id="email">${this.email}</span>
        <label for="phone">Phone</label><span id="phone">${this.phone}</span>
        <label for="repairassignment">Assignment</label
        ><span id="repairassignment">${this.repairassignment}</span>
      </article>

      <div class="actions">
        <button @click=${this.backToDashboard}>OK</button>
        <button class="remove" @click=${this.removeAssignment}>Remove</button>
      </div>
    `;
  }
}

customElements.define('repair-assignment', RepairAssignment);
```

Next we need to define a page component for the `repair-assignment` component. Create a new file `repairassignment-page.js` in the directory `/src/view/pages`:

```javascript
import { LitElement, html, css } from 'lit';
import { router } from '../router';
import '../components/repair-assignment';

export class RepairassignmentPage extends LitElement {
  static styles = css`
    :host {
      display: grid;
      gap: 1rem;
    }

    repair-assignment {
      width: 600px;
    }
  `;

  static properties = {
    id: { type: String },
  };

  constructor() {
    super();
    this.id = '-1';
  }

  connectedCallback() {
    super.connectedCallback();
    const FIRST_ARRAY_ELEMENT = 0;
    const idArray = Array.isArray(router.location.params.id);
    this.id = idArray
      ? router.location.params.id[FIRST_ARRAY_ELEMENT]
      : router.location.params.id;
  }

  // eslint-disable-next-line class-methods-use-this
  render() {
    return html`
      <h1>Repair Assignment</h1>
      <repair-assignment id=${this.id}></repair-assignment>
    `;
  }
}

customElements.define('repairassignment-page', RepairassignmentPage);
```

In this `repairassignment-page` component we extract the `id` parameter from the router location (our url) in the `connectedCallback` method, make sure that we extract a single string param and pass it to the `repair-assignment` component.

Now we need to update the router configuration in the `router.js` file in the `/src/view` directory to include the new dynamic parameterized route:

```javascript
import { Router } from '@vaadin/router';

import './pages/home-page';
import './pages/intake-page';
import './pages/repairassignment-page';

const outlet = document.querySelector('main');
const router = new Router(outlet);

router.setRoutes([
  { path: `/`, component: 'home-page' },
  { path: `/intake`, component: 'intake-page' },
  { path: `/assignment/:id`, component: 'repairassignment-page' },
]);

export { router };
```

As you can see we added a new route `{ path: `/assignment/:id`, component: 'repairassignment-page' }` to the router configuration. This route will match any url that starts with `/assignment/` and will pass the value of the `id` parameter to the `repairassignment-page` component.

## Fallback Route

But what if for some reason we navigate to a route that does not exist? We can define a fallback route that will be shown when no other route matches. To demonstrate this we will create a new component `page-not-found.js` in the directory `/src/view/pages`:

```javascript
import { LitElement, html, css } from 'lit-element';

export class PageNotFound extends LitElement {
  render() {
    return html`
      <h1>Page not found</h1>
      <p>Oops! The page you are looking for does not exist.</p>
      <a href="/">Go back to the home page</a>
    `;
  }
}

customElements.define('page-not-found', PageNotFound);
```

The route to this component will be the fallback route. Update the `router.js` file in the `/src/view` directory to include the new fallback route:

```javascript
import { Router } from '@vaadin/router';

import './pages/home-page';
import './pages/intake-page';
import './pages/repairassignment-page';
import './pages/page-not-found';

const outlet = document.querySelector('main');
const router = new Router(outlet);

router.setRoutes([
  { path: `/`, component: 'home-page' },
  { path: `/intake`, component: 'intake-page' },
  { path: `/assignment/:id`, component: 'repairassignment-page' },
  { path: `(.*)`, component: 'page-not-found' },
]);

export { router };
```

The fallback route uses a wildcard `(.*)` that will match any url that does not match any other route. This route always has to be the last route in the router configuration.

So if we navigate to a route that does not exist (like the `/demo` page) the `page-not-found` component will be shown.
However if we navigate to a route that does exist (like the `/assignment/42` page) the `repairassignment-page` component will be shown. But there we might see an issue if there is no repair assignment with the id `42`.
To fix this we update the call of `getRepairAssignment` within the `connectedCallback` method of the `repair-assignment` component with a `catch` block that will navigate back to the home-page if the repair assignment does not exist:

```javascript
  connectedCallback() {
    super.connectedCallback();
    dashboardController.getRepairAssignment(this.id).then((repair) => {
      this.name = repair.name;
      this.email = repair.email;
      this.phone = repair.phone;
      this.repairassignment = repair.repairassignment;
    })
    .catch(() => {
      Router.go(`${import.meta.env.BASE_URL}`);
    });
  }
  ```

  ## Lifecycle Hooks

  In the lifecycle of an webcomponent there are several hooks that are called at different stages of the lifecycle. We have already seen the `connectedCallback` method that is called when the component is connected to the DOM. There are also other hooks like `disconnectedCallback`, `attributeChangedCallback`, `adoptedCallback` and `render`.

  The Vaadin Router also adds some lifecycle hooks to the components that are used as pages. These hooks are `onBeforeEnter`, `onAfterEnter`, `onBeforeLeave` and `onAfterLeave`. These hooks are called when the component is navigated to or from.

  We can use these hooks to perform some actions when we navigate to or from a page. For example we can use the `onBeforeLeave` hook to ask the user if he really wants to leave the page. But we could also use it for authentication purposes. Meaning that if a user is not authenticated we could redirect him to the login page.
  
  To demonstrate this we add a `user-service.js` file in the `/src/service` directory with the following content:

  ```javascript
  class UserService {
  constructor() {
    this.user = {
      authenticated: false,
      role: '',
    }
  }

  isLoggedIn() {
    return new Promise((resolve) => {
      resolve(this.user.authenticated);
    });
  }

  getUserRole() {
    return new Promise((resolve) => {
      resolve(this.user.role);
    });
  }

  setUserRole(role) {
    this.user.role = role;
    this.user.authenticated = true;
  }
}

const userService = new UserService();

export { userService };
```

Next we add a `user-controller.js` file in the `/src/controller` directory with the following content:

```javascript
import { userService } from '../service/user-service';

class UserController {
  constructor(userServiceInstance) {
    this.userService = userServiceInstance;
  }

  async isLoggedIn() {
    return await this.userService.isLoggedIn();
  }

  async getUserRole() {
    return await this.userService.getUserRole();
  }

  async setUserRole(role) {
    return await this.userService.setUserRole(role);
  }
}

const userController = new UserController(userService);

export { userController };
```

We further create a new component `login-page.js` in the directory `/src/view/pages`:

```javascript
import { LitElement, html, css } from 'lit';
import { Router } from '@vaadin/router';
import { userController } from '../../controller/user-controller';

export class LoginPage extends LitElement {
  static styles = css``;
  static properties = {};

  loginHandler(event) {
    event.preventDefault();
    const userrole = event.target.userrole.value;
    userController.setUserRole(userrole);
    Router.go(import.meta.env.BASE_URL);
  }

  render() {
    return html`
      <h1>Login</h1>
      <form @submit=${this.loginHandler}>
        <label for="userrole">Select a user role to log in</label>
        <select id="userrole" name="userrole">
          <option value="bike-doctor">Bike Doctor</option>
          <option value="anomynous">Anomynous</option>
        </select>
        <button type="submit">Login</button>
      </form>
    `;
  }
}

customElements.define('login-page', LoginPage);
```

We could have build a nive login page with a username and password field and could have let the service use the JSON-Server to store and retrieve users. But for this assignment we keep it simple and just let the user select a role to log in.

Next we update the `router.js` file in the `/src/view` directory to include the new login page:

```javascript
import { Router } from '@vaadin/router';

import './pages/home-page';
import './pages/login-page';
import './pages/intake-page';
import './pages/repairassignment-page';
import './pages/page-not-found';

const outlet = document.querySelector('main');
const router = new Router(outlet);

router.setRoutes([
  { path: `/`, component: 'home-page' },
  { path: `/login`, component: 'login-page' },
  { path: `/intake`, component: 'intake-page' },
  { path: `/assignment/:id`, component: 'repairassignment-page' },
  { path: `(.*)`, component: 'page-not-found' },
]);

export { router };
```

Finally we add the lifecycle hooks to the `home-page` component in the `/src/view/pages` directory:

```javascript
import { LitElement, html, css } from 'lit-element';
import { userController } from '../../controller/user-controller';
import '../components/repairassignment-list';

export class HomePage extends LitElement {
  static styles = css`
    h1 {
      text-align: center;
    }
  `;

  constructor() {
    super();
    console.log('constructor');
  }

  async onBeforeEnter(location, commands) {
    console.log('onBeforeEnter', location, commands);
    const loggedIn = await userController.isLoggedIn();
    if (!loggedIn) {
      return commands.redirect(`/login`);
    }
    const role = await userController.getUserRole();
    console.log(`logged in as ${role}`);
    return null;
  }

  async onAfterEnter(location) {
    console.log('onAfterEnter', location);
  }

  async onBeforeLeave(location, commands) {
    console.log('onBeforeLeave', location, commands);
  }

  async onAfterLeave(location) {
    console.log('onAfterLeave', location);
  }

  connectedCallback() {
    super.connectedCallback();
    console.log('connectedCallback');
  }

  disconnectedCallback() {
    console.log('disconnectedCallback');
    super.disconnectedCallback();
  }

  // eslint-disable-next-line class-methods-use-this
  render() {
    return html`
      <h1>Repair Assignments</h1>
      <repairassignment-list></repairassignment-list>
      <a href="./intake">Add a repair assignment</a>
    `;
  }
}

customElements.define('home-page', HomePage);
```

If you access the site now in the browser, you will see that the console logs the lifecycle hooks of the `home-page` component. And since we are not logged in we are getting redirected by the `onBeforeEnter` method to the login page. If you select a role and click the login button you will be redirected back to the home page.
The `onBeforeEnter` method gets called again, but this time we are logged in and we see in the console that we are logged in as the selected role.

We could use the selected role in the render method to set attributes on tags and use the css to hide or show elements based on the role. But for this assignment we keep it simple and just log the role to the console.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 6](./assignment6.md) | :arrow_up: [Assignments](./README.md) 
