# Assignment 5: Multiple Pages Application (MPA)

In this assignment we will create a multiple pages application (MPA) meaning that our project will have multiple html files.

## Intake Page

We will start with a single page in the root directory of our project that we will name `intake-page.html`.
The repsonsibility of this page is to show a form to do an intake of a bike. This form will be a webcomponent that we will create using Lit.

The content of the `intake-page.html` file will be:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <base href="%BASE_URL%" />

    <link rel="icon" type="image/svg+xml" href="/lit.svg" />

    <title>Bike Repair Shop</title>
    <script type="module" src="/src/view/pages/intake-page.js"></script>
  </head>
  <body>
    <intake-form></intake-form>
  </body>
</html>
```

To reach this page we will also add a link to it from the home page. The home page will be the `index.html` file in the root directory of our project.

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <base href="%BASE_URL%" />

    <link rel="icon" type="image/svg+xml" href="/lit.svg" />

    <title>Bike Repair Shop</title>
  </head>
  <body>
    <main>
      <h1>Bike Repair Shop</h1>
      <a href="./intake-page.html">Add a repair assignment</a>
    </main>
  </body>
</html>

```

Since we use Vite we also need to add this page to the `vite.config.js` file:

```javascript
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// got the above code from https://flaviocopes.com/fix-dirname-not-defined-es-module-scope/
// this will resolve eslint errors for __dirname

import { resolve } from 'path';
import { defineConfig } from 'vite';
export default defineConfig({
  base: '/github-pages-subdir/',
  build: {
    sourcemap: true,
    rollupOptions: {
      input: {
        main: resolve(__dirname, 'index.html'),
        intake: resolve(__dirname, 'intake-page.html'),
      },
    },
  },
});
```

Like we did in the previous assignments of vanilla webcomponents and lit webcomponents, we will create a `view` folder in the `src` folder that holds two subfolders: `pages` and `components`.

Each page will have exactly one javascript file within the `pages` folder that will hold the webcomponent for that page.

Lets create the file `intake-page.js` within the `/src/view/pages` folder and give it the content:

```javascript
import '../components/intake-form.js';
```

Now lets create the file `intake-form.js` within the `/src/view/components` folder and give it the content:

```javascript
import { LitElement, html, css } from 'lit';
import { intakeController } from '../../controller/intake-controller';

export class IntakeForm extends LitElement {
  static styles = css`
    form {
      display: grid;
      gap: 1rem;
      margin: 1rem;
    }
    fieldset {
      padding: 1rem;
      margin: 0;

      border: 1px solid #007bff;

      display: grid;
      gap: 0.5rem;
      grid-template-columns: auto 1fr;
    }
    legend {
      font-weight: bold;
      padding: 0.5rem;
      font-size: 1.25rem;
    }

    input,
    textarea {
      width: 100%;
    }
    button {
      background-color: #007bff;
      color: white;
      border: 0;
      padding: 0.5rem 1rem;
      border-radius: 0.25rem;
    }

    .repair-id {
      display: none;
    }
  `;

  static properties = {
    repairId: { type: String },
  };

  constructor() {
    super();
    this.repairId = '-1';
  }

  submitHandler(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    // eslint-disable-next-line one-var
    const data = Object.fromEntries(formData);
  }

  render() {
    return html`
      <form @submit=${this.submitHandler}>
        <p class="repair-id">Repair ID: ${this.repairId}</p>
        <fieldset>
          <legend>Customer information</legend>
          <label for="name">Name</label>
          <input type="text" name="name" required />

          <label for="email">Email</label>
          <input type="email" name="email" required />

          <label for="phone">Phone</label>
          <input type="tel" name="phone" required />
        </fieldset>

        <fieldset>
          <legend>Repair assignment information</legend>
          <label for="repairassignment">What should be done:</label>
          <textarea
            name="repairassignment"
            placeholder=""
            rows="10"
            cols="60"
            required
          ></textarea>
        </fieldset>
        <button type="submit">Submit Repair Assignment</button>
      </form>
    `;
  }
}

customElements.define('intake-form', IntakeForm);
```

With this code we get an intake form that fullfills the requirements regarding the input fields of the user story.
We also have a submit button that will trigger the `submitHandler` method when clicked. This method will prevent the default form submission and extract the form data into a JSON object.

The next step is to send this data to the controller to be saved. The controller will take care of saving the data and return a JSON object holding the unique ID for the repair assignment.

```javascript
  submitHandler(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    // eslint-disable-next-line one-var
    const data = Object.fromEntries(formData);

    intakeController
      .addRepair(data)
      .then((repairAssignment) => {
        this.repairId = repairAssignment.id;
      });
      // It is important to close the "then" block here, so lit can re-render the component, because this.repairId is a reactive property that has changed.
  }
```

Setting the reactive property `this.repairId` to the received repair assignment id, will trigger a re-render of the component. But because we have set the `repair-id` class to `display: none;` in the css, the repair id will not be shown to the user. We did hide the repair id because it wasn't known at the time before the form was submitted and showing the repair id now would be of no use, since we will redirect the user to the home page after the form is submitted.

So why do we show the repair id at all? We show the repair id, because we want to see it on the printout that we will trigger when the form is submitted.
The printout is actually this intake form page, but without the submit button and with the repair id shown.

So the print should be triggeren when we have received the repair id from the controller and set it to the `this.repairId` property.

```javascript
  submitHandler(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    // eslint-disable-next-line one-var
    const data = Object.fromEntries(formData);

    intakeController
      .addRepair(data)
      .then((repairAssignment) => {
        this.repairId = repairAssignment.id;
      })
      // It is important to close the "then" block here, so lit can re-render the component, because this.repairId is a reactive property that has changed.
      .then(() => {
        this.printRepairCard();
      });
  }
```

So we need to implement the `printRepairCard` method that will trigger the print of the form data.

```javascript
  // eslint-disable-next-line class-methods-use-this
  printRepairCard() {
    window.print();
  }
```

This is less work than we thought. And since it is a single statement we can also put it directly in the `then` block of the promise chain.

```javascript
  submitHandler(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    // eslint-disable-next-line one-var
    const data = Object.fromEntries(formData);

    intakeController
      .addRepair(data)
      .then((repairAssignment) => {
        this.repairId = repairAssignment.id;
      })
      // It is important to close the "then" block here, so lit can re-render the component, because this.repairId is a reactive property that has changed.
      .then(() => {
        window.print();
      });
  }
```

The problem we now have is that the printout will show the whole page, including the submit button and without the repairId. To fix this we need to change the stylesheet for the print media. So we add the following to the css of the `intake-form.js` file:

```css
@media print {
  button {
    display: none;
  }

  .repair-id {
    display: block;
  }
}
```

Finally we will clear the form (which is of no importance since we leave the pageway) and navigate to the home page after the form is submitted.

```javascript
  submitHandler(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    // eslint-disable-next-line one-var
    const data = Object.fromEntries(formData);

    // This is where you would send the data to the controller to be saved.
    // The controller will take care of saving the data and return a unique ID for the repair assignment.

    intakeController
      .addRepair(data)
      .then((repairAssignment) => {
        this.repairId = repairAssignment.id;
      })
      // It is important to close the "then" block here, so lit can re-render the component, because this.repairId is a reactive property that has changed.
      .then(() => {
        window.print();
      })
      .finally(() => {
        this.shadowRoot.querySelector('form').reset();
        this.repairId = '-1';
        window.location.href = '/';
      });
  }
```

Here the redirect location is hardcoded to the root of the project. This might result in problems since on Github Pages our project would be hosted within a subdir.
To fix this we need to add the base url to the redirect location. We can do this by adding a `base` variable we defined in the `vite.config.js`. From JavaScript we can extract this variable using `import.meta.env.BASE_URL`.

So lets change the redirect location to `import.meta.env.BASE_URL`:

```javascript
  submitHandler(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    // eslint-disable-next-line one-var
    const data = Object.fromEntries(formData);

    // This is where you would send the data to the controller to be saved.
    // The controller will take care of saving the data and return a unique ID for the repair assignment.

    intakeController
      .addRepair(data)
      .then((repairAssignment) => {
        this.repairId = repairAssignment.id;
      })
      // It is important to close the "then" block here, so lit can re-render the component, because this.repairId is a reactive property that has changed.
      .then(() => {
        window.print();
      })
      .finally(() => {
        this.shadowRoot.querySelector('form').reset();
        this.repairId = '-1';
        window.location.href = `${import.meta.env.BASE_URL}`;
      });
  }
```

We have now finished the userstory for the intake form. The next step is to work on the second user story that will give us a dashboard at the home page.

## Home Page

The home page will show a list of all the repair assignments that are stored in the database. We will update the `index.html` for this to import the `/src/view/pages/home-page.js` file and to show the `repairassignment-list` webcomponent.

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <base href="%BASE_URL%" />

    <link rel="icon" type="image/svg+xml" href="/lit.svg" />

    <title>Bike Repair Shop</title>
    <script type="module" src="/src/view/pages/home-page.js"></script>
  </head>
  <body>
    <main>
      <h1>Bike Repair Shop</h1>
      <repairassignment-list></repairassignment-list>
      <a href="./intake-page.html">Add a repair assignment</a>
    </main>
  </body>
</html>
```

The `home-page.js` file will import the `repairassignment-list` webcomponent.

```javascript
import '../components/repairassignment-list';
```

The `repairassignment-list` webcomponent will show a list of all the repair assignments that are stored in the database. We will create a `repairassignment-list.js` file within the `/src/view/components` folder and give it the content:

```javascript
import { LitElement, html, css } from 'lit';
import { dashboardController } from '../../controller/dashboard-controller';
import './repairassignment-list-item';

export class RepairassignmentList extends LitElement {
  static styles = css``;

  static properties = {
    repairs: { type: Array },
  };

  constructor() {
    super();
    this.repairs = [];
  }

  connectedCallback() {
    super.connectedCallback();
    dashboardController.getRepairsList().then((repairs) => {
      this.repairs = repairs;
    });
  }

  render() {  
    return html`
      <h2>Repair Assignments</h2>
      ${this.repairs.map(
        (repair) => html`
          <repairassignment-list-item id=${repair.id}></repairassignment-list-item>
        `
      )}
    `;
  }
}

customElements.define('repairassignment-list', RepairassignmentList);
```

This webcomponent will make use of the `dashboardController` to get the list of repair assignments. The `dashboardController` will return a list of repair assignments that will be stored in the `repairs` property of the webcomponent.
Note that we assign in the connectedCallback we assign a new array to the `repairs` property. This will trigger a re-render of the component. If we would only have pushed the new items to the array, the component would not have re-rendered.

The `dashboardController` will now look like this:

```javascript
import { repairService } from '../service/repair-service';

class DashboardController {
  constructor(repairServiceInstance) {
    this.repairService = repairServiceInstance;
  }

  /**
   * 
   * @returns {Promise<JSON Object>}
   */
  getRepairsList() {
    return this.repairService.getRepairsList();
  }
}

const dashboardController = new DashboardController(repairService);

export { dashboardController };
```

This shows us that we also need to add a method to the service to get the list of repair assignments. The `repair-service.js` file will now look like this:

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

  /**
   * Retrieves a list of all repair assignments from the database
   * 
   * @returns {Promise<JSON Object>}
   */
  getRepairsList() {
    return fetch(`${this.repairServer}repairs`)
      .then(response => response.json());
  }
}

const repairService = new RepairService();

export { repairService };
```

We further can see that the `repairassignment-list` webcomponent will make use of the `repairassignment-list-item` webcomponent. This webcomponent will show the details of a single repair assignment. We will create a `repairassignment-list-item.js` file within the `/src/view/components` folder and give it the content:

```javascript
import { LitElement, html, css } from 'lit';
import { dashboardController } from '../../controller/dashboard-controller';

export class RepairassignmentListItem extends LitElement {
  static styles = css`
    :host {
      display: grid;
      gap: 1rem;
      grid-template-columns: 1fr minmax(auto, 300px);

      padding: 0.2rem;
      margin: 0.2rem;
      border: 1px solid #007bff;
    }

    .repairassignment {
      font-size: 1.25rem;
      font-weight: bold;
      padding: 0.5rem;
    }

    .repairassignment:hover {
      cursor: pointer;
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

  showAssignment() {
    const dialog = this.shadowRoot.querySelector('dialog');
    dashboardController.getRepairAssignment(this.id).then((repair) => {
      this.name = repair.name;
      this.email = repair.email;
      this.phone = repair.phone;
      this.repairassignment = repair.repairassignment;

      dialog.showModal();
    });
  }

  removeAssignment() {
    dashboardController.removeRepairAssignment(this.id).then(() => {
      this.remove();
    });
  }

  render() {
    return html`
      <span class="repairassignment" @click=${this.showAssignment}
        >${this.id}</span
      >
      <button class="remove" @click=${this.removeAssignment}>Remove</button>

      <dialog >
        <h2>Repair Assignment - ${this.id}</h2>
        <p>Name: ${this.name}</p>
        <p>Email: ${this.email}</p>
        <p>Phone: ${this.phone}</p>
        <p>Assignment: ${this.repairassignment}</p>
        <form method="dialog">
          <button autofocus>OK</button>
          <button class="remove" @click=${this.removeAssignment}>Remove</button>
        </form>
        
      </dialog>
    `;
  }
}

customElements.define('repairassignment-list-item', RepairassignmentListItem);
```

This webcomponent will show the details of a single repair assignment. It will show the id of the repair assignment and a remove button. When the id is clicked the details of the repair assignment will be shown in a dialog. The dialog will have an OK button to close the dialog and a remove button to remove the repair assignment.
We could have extracted the dialog to a separate webcomponent, which would have been a good idea, which would have made it easier to split up tasks or the story itself. But for this assignment we will keep it simple and keep the dialog in the same webcomponent.

The last part we need to implement are the methods in the `dashboardController` to get a single repair assignment and to remove a repair assignment.

```javascript
import { repairService } from '../service/repair-service';

class DashboardController {
  constructor(repairServiceInstance) {
    this.repairService = repairServiceInstance;
  }

  /**
   * 
   * @returns {Promise<JSON Object>}
   */
  getRepairsList() {
    return this.repairService.getRepairsList();
  }

  /**
   * 
   * @param {number} repairId 
   * @returns {Promise<JSON Object>}
   */
  getRepairAssignment(repairId) {
    return this.repairService.getRepairAssignment(repairId);
  }

  /**
   * 
   * @param {number} repairId 
   * @returns {Promise<JSON Object>}
   */
  removeRepairAssignment(repairId) {
    return this.repairService.removeRepairAssignment(repairId);
  }

}

const dashboardController = new DashboardController(repairService);

export { dashboardController };
```

This results in the following `repair-service.js` file:

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

  /**
   * Retrieves a list of all repair assignments from the database
   * 
   * @returns {Promise<JSON Object>}
   */
  getRepairsList() {
    return fetch(`${this.repairServer}repairs`)
      .then(response => response.json());
  }

  /**
   * Retrieves a single repair assignment from the database
   * 
   * @param {String} repairId 
   * @returns {Promise<JSON Object>}
   */
  getRepairAssignment(repairId) {
    return fetch(`${this.repairServer}repairs/${repairId}`)
      .then(response => response.json());
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
    }
    return fetch(`${this.repairServer}repairs/${repairId}`, options);
  }
}

const repairService = new RepairService();

export { repairService };
```

We now hava a working MPA that fullfills the requirements of the two user stories. We can now add, view and remove repair assignments.

We can merge the user stories to a single `review` branch and present the work to the product owner, who will provide feedback resulting in new stories or refinements of the current stories, but for now we are done.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 4](./assignment4.md) | :arrow_up: [Assignments](./README.md) | [Assignment 6](./assignment6.md) :arrow_forward:
