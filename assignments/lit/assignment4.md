# Assignment 4: CustomEvent Counter

In this assignment we will create a component CustomEvent Counter component that will display a button and a counter. When the button is clicked, the counter will be incremented by 1. The counter value will be displayed next to the button. The counter value will be dispatched as a CustomEvent to a listener component.

Create a new file `lit-event-counter.js` and add the following code:

```javascript
import { LitElement, html, css } from 'lit';

export class LitEventCounter extends LitElement {
  #INITIAL_VALUE = 0;
  #INCREMENT_VALUE = 1;

  static styles = css`
    button {
      background-color: darkseagreen;
      color: white;
      padding: 10px;
      margin: 10px;
      border-radius: 5px;
    }
  `;

  static properties = {
    count: { type: Number },
  };

  constructor() {
    super();
    this.count = this.#INITIAL_VALUE;
  }

  updateCounter() {
    this.count += this.#INCREMENT_VALUE;

    const eventOptions = {
      bubbles: true,
      composed: true,
      detail: { count: this.count },
    };

    const counterUpdateEvent = new CustomEvent(
      'litEventCounterUpdate',
      eventOptions
    );

    // if we dispatch the event from 'this', it will be dispatched outside the shadow DOM. Therefore the compose attribute doesn't have any effect.
    // this.dispatchEvent(counterUpdateEvent);

    // if we however dispatch the event from within the shadowRoot, it will be dispatched inside the shadow DOM. Therefore the compose attribute will have an effect.
    this.shadowRoot.dispatchEvent(counterUpdateEvent);
  }

  render() {
    return html`
      <h2>Lit Event Counter</h2>
      <button @click=${this.updateCounter}>Click me!</button>
      <span>${this.count}</span>
    `;
  }
}

customElements.define('lit-event-counter', LitEventCounter);
```

Note that unlike in the vanilla web component assignment, we don't add a `click` event listener to the button element with the `addEventListener`. Instead, we use the `@click` directive in the button element to call the `updateCounter` method. This is a feature of LitElement.
You can any kind of directive to an element in LitElement. For example, you can use `@mouseover`, `@mouseout`, `@change`, `@submit`, etc.

Notice also that in the `updateCounter` method, we dispatch a custom event we call `litEventCounterUpdate`. We dispatch the custom event from the `shadowRoot` instead of `this`. This is because if we dispatch the event from `this`, it will be dispatched outside the shadow DOM. Therefore the `composed` attribute doesn't have any effect. If we dispatch the event from within the shadowRoot, it will be dispatched inside the shadow DOM. Therefore the `composed` attribute will have an effect.

Next, we will create a listener component that listens for the `litEventCounterUpdate` event.

Create a new file `lit-event-listener.js` and add the following code:

```javascript
import { LitElement, html, css } from 'lit';

export class LitEventListener extends LitElement {
  static styles = css``;

  static properties = {
    counter: { type: Number },
  };

  constructor() {
    super();
    this.counter = -1;
  }

  connectedCallback() {
    super.connectedCallback();
    document.addEventListener(
      'litEventCounterUpdate',
      this.litEventCounterUpdateHandler.bind(this)
    );
  }

  disconnectedCallback() {
    document.removeEventListener(
      'litEventCounterUpdate',
      this.litEventCounterUpdateHandler.bind(this)
    );
    super.disconnectedCallback();
  }

  litEventCounterUpdateHandler(event) {
    this.counter = event.detail.count;
  }

  render() {
    return html`
      <h2>Lit Event Listener</h2>
      <p>Received Lit Event Counter value: ${this.counter}</p>
    `;
  }
}

customElements.define('lit-event-listener', LitEventListener);
```

Note that now the `connectedCallback` method first has to call the `super.connectedCallback()` method. This is because we are overriding the `connectedCallback` method in the class. The `super.connectedCallback()` method will call the `connectedCallback` method of the parent class.

Notice also that at the `disconnectedCallback` method the final statement is to call the `super.disconnectedCallback()` method. This is because we are overriding the `disconnectedCallback` method in the class.The `super.disconnectedCallback()` method will call the `disconnectedCallback` method of the parent class.
And because this might close for instance a connection we still use within the `disconnectedCallback`, it is important to call the `super.disconnectedCallback()` method at the end of the `disconnectedCallback` method.

Note that now we add an event listener to the `document` object in the `connectedCallback` method. We also remove the event listener in the `disconnectedCallback` method.
That we are listening on the `document` object is because the event is dispatched from the `LitEventCounter` component. Therefore the event will bubble up to the `document` object. But as you will see from the `html` code below, the `LitEventListener` component is not a child of the `LitEventCounter` component. Therefore the event will not bubble up to the `LitEventListener` component. This is why we listen at the `document` object to make sure we receive the event.

We also have to use the `bind` method to bind the `this` object to the event handler method. This is because the `this` object in the event handler method will refer to the `document` object and not the custom element itself.

Next we need to import our components in the `/src/view/pages/home-page.js` file. Add the following lines to the bottom of imports within the file:

```javascript
import '../components/lit-event-counter';
import '../components/lit-event-listener';
```

Finally, add the following code to the `index.html` file:

```html
<lit-event-counter></lit-event-counter>
<hr />

<lit-event-listener></lit-event-listener>
<hr />
```

As with the vanilla webcompont assignment, you can test the functionality of the components by clicking the button in the `LitEventCounter` component. The counter value will be displayed next to the button. The counter value will be dispatched as a CustomEvent to the `LitEventListener` component and displayed there.
And if you change the `compose` property in the `CustomEvent` constructor in the `LitEventCounter` component from `true` to `false`, the event will not bubble up to the `LitEventListener` component. This is because the event will not be composed and will not bubble up to the `document` object.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 3](./assignment3.md) | :arrow_up: [Assignments](./README.md) | [Assignment 5](./assignment5.md) :arrow_forward: