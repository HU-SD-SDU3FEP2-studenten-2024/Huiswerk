# Assignment 10: Lifecycle Callbacks

In this final assignment about vanilla web components, we will learn about the lifecycle callbacks of a web component. The lifecycle callbacks are methods that are called at different stages of the lifecycle of a web component. To demonstrate it we create a new web component called `wc-lifecycle-demo` and log messages to the console in each method.

```javascript
export class WcLifecycleDemo extends HTMLElement {
  constructor() {
    super();
    console.log('WC Lifecycle Demo: constructor');
    this.name = '';
    this.shadowDom = this.attachShadow({ mode: 'closed' });
  }

  static get observedAttributes() {
    console.log('WC Lifecycle Demo: observedAttributes');
    return ['name'];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    console.log('WC Lifecycle Demo: attributeChangedCallback');
    if (name === 'name') {
      this.name = newValue;
    }
  }

  connectedCallback() {
    console.log('WC Lifecycle Demo: connectedCallback');
  }

  disconnectedCallback() {
    console.log('WC Lifecycle Demo: disconnectedCallback');
  }

  render() {
    console.log('WC Lifecycle Demo: render');
  }
}

customElements.define('wc-lifecycle-demo', WcLifecycleDemo);
```

Next we import the `wc-lifecycle-demo` component in the `home-page.js` file.

```javascript
import '../components/wc-lifecycle-demo.js';
```

So that we can use the `wc-lifecycle-demo` component in the `index.html` file.

```html
<wc-lifecycle-demo></wc-lifecycle-demo>
```

If you open the browser console you should see the following messages.

```
WC Lifecycle Demo: observedAttributes
WC Lifecycle Demo: constructor
WC Lifecycle Demo: connectedCallback
```

The `observedAttributes` method is called when the component is registered. The `constructor` method is called when the component is created. The `connectedCallback` method is called when the component is added to the DOM.

Now let's add an attribute to the `wc-lifecycle-demo` component in the `index.html` file.

```html
<wc-lifecycle-demo name="demo"></wc-lifecycle-demo>
```

If you open the browser console you should see the following messages.

```
WC Lifecycle Demo: observedAttributes
WC Lifecycle Demo: constructor
WC Lifecycle Demo: attributeChangedCallback
WC Lifecycle Demo: connectedCallback
```

The `attributeChangedCallback` method is called when the attribute is added to the component.
Remember that in [assignment 7](./assignment7.md) we got a lot of errors in the console and that we said that they are related to the lifecycle of the web component.
In assignment 7 our `attributeChangedCallback` method called the `render` method. And the `render` method tried to query elements of our webcomponent before the `connectedCallback` method was called. The elements the `render` method tried to query were created in the `connectedCallback` method. So we could have fixed the errors by moving the creation of the elements to the `render` method as well (which would be nicer, because it's not a good practice to create elements in the `connectedCallback` method).
But than we would have had the problem that the `render` method would have been called before the `connectedCallback` method. And the `render` method should only be called after the `connectedCallback` method was called. Because the `connectedCallback` method is called when the component is added to the DOM. And the `render` method should only be called when the component is added to the DOM. So we could have fixed the errors by adding a variable that checks if the `connectedCallback` method was called before the `render` method.

Wat you might notice in this wc-lifecycle-demo is that the `render` and the `disconnectedCallback` method are not called. The `render` method is not called because it's not part of the lifecycle of a vanilla web component.
The `disconnectedCallback` method is not called because we don't remove the `wc-lifecycle-demo` component from the DOM. Would we dynamically remove the `wc-lifecycle-demo` component from the DOM, the `disconnectedCallback` method would be called. That's why the `connectedCallback` method is the place to add event listeners and the `disconnectedCallback` method is the place to remove event listeners.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 9](./assignment9.md) | :arrow_up: [Assignments](./README.md)
