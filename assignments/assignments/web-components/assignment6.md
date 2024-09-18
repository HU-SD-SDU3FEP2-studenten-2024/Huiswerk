# Assignment 6: Shadowdom CustomEvent Counter

In this assignment we will create a slightly modified version of the web component from assignment 5. This time we will use the shadow DOM. Except for the shadow DOM, the name of the event that will be dispatched is also different. The event will be dispatched from the button element within the webcomponent.

Create a new file `custom-shadow-event-counter.js` and add the following code:

```javascript
export class CustomShadowEventCounter extends HTMLElement {
  #INITIAL_VALUE = 0;
  #INCREMENT_VALUE = 1;

  constructor() {
    super();

    this.shadowDom = this.attachShadow({ mode: 'closed' });

    this.counterValue = this.#INITIAL_VALUE;
  }

  connectedCallback() {
    const css = `
      <style>
        button {
        background-color: darkseagreen;
        color: white;
        padding: 10px;
        margin: 10px;
        border-radius: 5px;
      }
      </style>
    `;
    const sectionElement = document.createElement('section');
    const h2Element = document.createElement('h2');
    const buttonElement = document.createElement('button');
    const spanElement = document.createElement('span');

    h2Element.textContent = 'Simple ShadowDom CustomEvent Counter';

    this.button = buttonElement; // because we need to access the button in the disconnectedCallback
    this.button.textContent = 'Click me!';
    this.button.addEventListener('click', this.updateCounter.bind(this));
    this.button.addEventListener(
      'customShadowEventCounterUpdate',
      this.render.bind(this)
    );

    sectionElement.appendChild(h2Element);
    sectionElement.appendChild(buttonElement);
    sectionElement.appendChild(spanElement);

    this.shadowDom.innerHTML = css;
    // we will append the sectionElement to the shadowDom instead of the custom element itself
    this.shadowDom.appendChild(sectionElement);

    this.render();
  }

  disconnectedCallback() {
    this.button.removeEventListener(
      'customShadowEventCounterUpdate',
      this.updateCounter.bind(this)
    );
    this.button.removeEventListener(
      'click',
      this.throwUpdateCounterEvent.bind(this)
    );
  }

  updateCounter() {
    this.counterValue += this.#INCREMENT_VALUE;

    const eventOptions = {
      bubbles: true,
      cancelable: true,
      composed: true,
      detail: {
        value: this.counterValue,
      },
    };

    const counterUpdateEvent = new CustomEvent(
      'customShadowEventCounterUpdate',
      eventOptions
    );

    // we will dispatch the event from the button element
    this.button.dispatchEvent(counterUpdateEvent);
  }

  render() {
    const counterField = this.shadowDom.querySelector('span');
    counterField.textContent = this.counterValue;
  }
}

customElements.define('custom-shadow-event-counter', CustomShadowEventCounter);
```

Import the webcomponent in the `/src/view/pages/home-page.js` file:

```javascript
import '../components/custom-shadow-event-counter.js';
```

And add the following code to the `index.html` file:

```html
<custom-shadow-event-counter></custom-shadow-event-counter>
<hr />
```

Just like with the previous assignment, the web component should display a counter that increments by one each time the button is clicked.

To express the difference between the two assignments, we will modify our custom-event-listener from the previous assignment to listen for the custom event dispatched from the button element within the web component.

```javascript
export class CustomEventListener extends HTMLElement {
  constructor() {
    super();
    this.customEventCounterValue = -1;
    this.customShadowEventCounterValue = -1;
  }

  connectedCallback() {
    const sectionElement = document.createElement('section');
    const h2Element = document.createElement('h2');
    const p1Element = document.createElement('p');
    p1Element.setAttribute('class', 'custom-event-counter-value');

    const p2Element = document.createElement('p');
    p2Element.setAttribute('class', 'custom-shadow-event-counter-value');

    h2Element.textContent = 'Custom Event Listener';

    sectionElement.appendChild(h2Element);
    sectionElement.appendChild(p1Element);
    sectionElement.appendChild(p2Element);

    this.appendChild(sectionElement);

    document.addEventListener(
      'customEventCounterUpdate',
      this.customEventCounterUpdateHandler.bind(this)
    );
    document.addEventListener(
      'customShadowEventCounterUpdate',
      this.customShadowEventCounterUpdateHandler.bind(this)
    );
  }

  disconnectedCallback() {
    document.removeEventListener(
      'customEventCounterUpdate',
      this.customEventCounterUpdateHandler.bind(this)
    );
    document.removeEventListener(
      'customShadowEventCounterUpdate',
      this.customShadowEventCounterUpdateHandler.bind(this)
    );
  }

  customEventCounterUpdateHandler(event) {
    this.customEventCounterValue = event.detail.value;
    this.render();
  }

  customShadowEventCounterUpdateHandler(event) {
    this.customShadowEventCounterValue = event.detail.value;
    this.render();
  }

  render() {
    const customEventCounterValueElement = this.querySelector(
      '.custom-event-counter-value'
    );
    customEventCounterValueElement.textContent = `Custom Event Counter Value: ${this.customEventCounterValue}`;

    const customShadowEventCounterValueElement = this.querySelector(
      '.custom-shadow-event-counter-value'
    );
    customShadowEventCounterValueElement.textContent = `Custom Shadow Event Counter Value: ${this.customShadowEventCounterValue}`;
  }
}

customElements.define('custom-event-listener', CustomEventListener);
```

When you now click one of the buttons, the counter value should also be displayed in the custom event listener. The listener should display a counter value of -1 for counters that have not been clicked yet.
So far the listener still don't express the difference between the two counters. But when we now set in both counters the `compose` attribute of the eventoptions to false, the listener will no longer update the counter value of the custom shadow event counter.
This is because the compose attribute is set to false, which means that the event will not cross the shadow boundary. This is the main difference between the two assignments.

---

:house: [Home](../../README.md) :arrow_backward: [Assignment 5](./assignment5.md) | :arrow_up: [Assignments](./README.md) | [Assignment 7](./assignment7.md) :arrow_forward: