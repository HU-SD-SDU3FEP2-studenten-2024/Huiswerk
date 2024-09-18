# Assignment 6: Lit Inputs

One of the most common tasks in web development is to handle user input. In this assignment, you will create a custom input component that will display the value of the input field as the user types.
Start by creating the file `lit-input.js` in the directory `/src/view/components` and add the following code:

```javascript
import { LitElement, html, css } from 'lit';

export class LitInput extends LitElement {
  static get properties() {
    return {
      inputValue: { type: String },
    };
  }

  constructor() {
    super();
    this.inputValue = '';
  }

  handleInput(event) {
    this.inputValue = event.target.value;
  }

  render() {
    return html`
      <h2>Lit Input</h2>
      <input
        id="lit-input-field"
        type="text"
        .value=${this.inputValue}
        @input=${this.handleInput}
      />

      <p>Value: ${this.inputValue}</p>
      <hr />
    `;
  }
}

customElements.define('lit-input', LitInput);
```

Note that the value attribute has a '.' before it. This is a special syntax that tells LitElement to bind the value of the input field to the `inputValue` property of the component. This will ensure that the value shown at the input field is always in sync with the `inputValue` property.
What it will not do is update the `inputValue` property when the user types in the input field. To achieve this, we need to add an event listener to the input field that calls the `handleInput` method whenever the user types in the input field.
The `@input` event listener is used to call the `handleInput` method whenever the user types in the input field. That the `handleInput` method is required to update the `inputValue` property might sound strange at first, but it is necessary because LitElement does not automatically update the property when the value of the input field changes. If it would do so we would call that a two-way binding, a concept that you might be familiar with from other frameworks like Angular or Vue. Lit however only supports one-way binding, which means that we have to update the property manually. The reason for this is that LitElement is designed to be as lightweight as possible, and two-way binding would add a lot of complexity to the framework.

Next, import the `lit-input` component in the `/src/view/pages/lit-page.js` file:

```javascript
import '../components/lit-input';
```

Finally, add the `lit-input` element to the `lit-page` template:

```html
<lit-input></lit-input>
```

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 5](./assignment5.md) | :arrow_up: [Assignments](./README.md)
