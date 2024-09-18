# Assignment 5: No Shadowdom CustomEvent Counter

In this assignment we will create a web component that will show a button and a counter, which counts how often we have clicked on the button. This web component will not use the shadow DOM. This web component will also generate a custom event that will be dispatched from the button element.

```javascript
export class CustomEventCounter extends HTMLElement {

  #INITIAL_VALUE = 0;
  #INCREMENT_VALUE = 1;

  constructor() {
    super();
    this.counterValue = this.#INITIAL_VALUE;
  }

  connectedCallback() {
    const sectionElement = document.createElement('section');
    const h2Element = document.createElement('h2');
    const buttonElement = document.createElement('button');
    const spanElement = document.createElement('span');

    h2Element.textContent = 'Simple CustomEvent Counter';

    this.button = buttonElement; // because we need to access the button in the disconnectedCallback
    this.button.textContent = 'Click me!';
    this.button.addEventListener('click', this.updateCounter.bind(this));
    this.button.addEventListener('customEventCounterUpdate', this.render.bind(this));

    sectionElement.appendChild(h2Element);
    sectionElement.appendChild(buttonElement);
    sectionElement.appendChild(spanElement);

    this.appendChild(sectionElement);

    this.render();
  }

  disconnectedCallback() {
    this.button.removeEventListener('customEventCounterUpdate', this.render.bind(this));
    this.button.removeEventListener('click', this.updateCounter.bind(this));
  }

  updateCounter() {
    // this method will now be called by the counterUpdate event listener
    this.counterValue += this.#INCREMENT_VALUE;

    const eventOptions = {
      bubbles: true,
      cancelable: true,
      composed: true,
      detail: {
        value: this.counterValue
      }
    };

    const counterUpdateEvent = new CustomEvent('customEventCounterUpdate', eventOptions);

    // we will dispatch the event from the button element
    this.button.dispatchEvent(counterUpdateEvent);
  }

  render() {
    const counterField = this.querySelector('span');
    counterField.textContent = this.counterValue;
  }
}

customElements.define('custom-event-counter', CustomEventCounter);
```

Note that within the `connectedCallback` the button gets two eventListeners. The first one listens for a click event and calls the `updateCounter` method. The method updated the counter and creates a custom event 'customEventCounterUpdate' and dispatches it from the button element. But it will not trigger the rendering method.

The second eventListener of the button listens for this custom event and calls the `render` method.
This might seems a bit complicated, but now we can set options for the event and in doing so we can pass data with the event and set options for it. This is a very powerful feature of custom events.

Another aspect to note is that we need to remove the eventListeners in the `disconnectedCallback` method. This is important to avoid memory leaks.

Next we need to import this web component in the `home-page.js` file.

```javascript
import '../components/custom-event-counter.js';
```

And we need to append the custom event counter to the body of the `index.html` file.

```html
<custom-event-counter></custom-event-counter>

<hr />
```

Viewing the result within the browser should show a button with a counter. Every time the button is clicked the counter should increase by one.

Now let's create another webcomponent that will listen for the custom event and also display the counter value.
Create a new file `/src/view/components/custom-event-listener.js` and add the following code:

```javascript
export class CustomEventListener extends HTMLElement {
  constructor() {
    super();
    this.customEventCounterValue = -1;
  }

  connectedCallback() {
    const sectionElement = document.createElement('section');
    const h2Element = document.createElement('h2');
    const p1Element = document.createElement('p');
    p1Element.setAttribute('class', 'custom-event-counter-value');

    h2Element.textContent = 'Custom Event Listener';

    sectionElement.appendChild(h2Element);
    sectionElement.appendChild(p1Element);

    this.appendChild(sectionElement);

    document.addEventListener(
      'customEventCounterUpdate',
      this.customEventCounterUpdateHandler.bind(this)
    );
  }

    disconnectedCallback() {
    document.removeEventListener(
      'customEventCounterUpdate',
      this.customEventCounterUpdateHandler.bind(this)
    );
  }


  customEventCounterUpdateHandler(event) {
    this.customEventCounterValue = event.detail.value;
    this.render();
  }

  render() {
    const customEventCounterValueElement = this.querySelector(
      '.custom-event-counter-value'
    );
    customEventCounterValueElement.textContent = `Custom Event Counter Value: ${this.customEventCounterValue}`;
  }
}

customElements.define('custom-event-listener', CustomEventListener);
```

Add the import statement to the `home-page.js` file.

```javascript
import '../components/custom-event-listener.js';
```

And append the custom event listener to the body of the `index.html` file.

```html
<custom-event-listener></custom-event-listener>
<hr />
```

Viewing the result within the browser should show a button with a counter and a custom event listener that listens for the custom event and displays the counter value.

When you now change within the file `/src/view/components/custom-event-counter` the `bubbles` attribute of the `eventOptions` to false, the CustomEvent will no longer bubble up to the document. This means that the custom event listener will not receive the event and the counter value will not be updated.

Changing the other attibutes of the `eventOptions` will also have an effect on the behavior of the custom event, but they will not effect the code of this assignment.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 4](./assignment4.md) | :arrow_up: [Assignments](./README.md) | [Assignment 6](./assignment6.md) :arrow_forward: