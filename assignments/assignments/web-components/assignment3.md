# Assignment 3: Hello Shadowdom

Create a file named `/view/components/hello-shadowdom.js` and give it the following content:

```javascript
export class HelloShadowDom extends HTMLElement {
  constructor() {
    super();

    this.shadowDom = this.attachShadow({ mode: 'open' });
    this.name = 'ShadowWorld!';
  }

  connectedCallback() {
    const sectionElement = document.createElement('section');
    const h2Element = document.createElement('h2');

    sectionElement.appendChild(h2Element);
    this.shadowDom.appendChild(sectionElement);

    this.render();
  }

  render() {
    this.shadowDom.querySelector('h2').textContent = `Hello ${this.name}!`;
  }
}

customElements.define('hello-shadowdom', HelloShadowDom);
```

Note that we added a `shadowDom` property to the class. This property is used to store the shadow DOM of the element. We also adapted the `connectedCallback` method to now append the content of the web component to the shadow DOM.

Add the following line to the bottom of the `/src/view/pages/home-page.js` file:

```javascript
import '../components/hello-shadowdom.js';
```

Now we can use the `hello-shadowdom` tag in our `index.html` file. Add the following code to the body of the file:

```html
<hello-shadowdom></hello-shadowdom>

<hr />
```

When you start the server and open the browser, you should see the text `Hello ShadowWorld!` on the page. What you might have noticed by now is unlike the web components from our first two assignments, that the text color of the h2 tag isn't blue.
This is because the shadow DOM is encapsulated from the rest of the page. This means that the styles of the page don't affect the shadow DOM and vice versa. This is a powerful feature of web components that allows us to create components that are isolated from the rest of the page.

:eyes: Inspect the element in the developer tools of the browser to see the shadow DOM.

In the constructor of our web component we call the `attachShadow` method on the `this` object. This method creates a shadow DOM for the element and returns a reference to it. The `mode` option can be set to `open` or `closed`. When you change it to `closed` you will not notice any difference in the output of the web component.

Lets add util functions investigate the shadow DOM. Create a file `/src/utils/hello-shadowdom-utils.js` and give it the following content:

```javascript
export function helloShadowDomViewer() {
  const helloShadowDomNode = document.querySelector('hello-shadowdom');
  console.log(helloShadowDomNode);
}

export function helloShadowDomH2Viewer() {
  const helloShadowDomH2Node = document
    .querySelector('hello-shadowdom')
    .shadowRoot.querySelector('h2');
  console.log(helloShadowDomH2Node);
}
```

The function `helloShadowDomViewer` logs the `hello-shadowdom` element to the console. The function `helloShadowDomH2Viewer` logs the `h2` element inside the shadow DOM of the `hello-shadowdom` element to the console.

To use this utils we need to import them in the `/src/view/pages/home-page.js` file.

```javascript
import { helloShadowDomH2Viewer, helloShadowDomViewer } from '../../utils/hello-shadowdom-viewer.js';
```

Further add the following lines below the imports:

```javascript
helloShadowDomViewer();
helloShadowDomH2Viewer();
```

When you start the server and open the browser, you should see the `hello-shadowdom` element and the `h2` element inside the shadow DOM logged to the console as long as you have set the shadow DOM mode within the constructor of the file `/src/view/components/hello-shadowdom` to `open`.

When you change the shadow DOM mode to `closed` you will see that the `hello-shadowdom` element is logged to the console, but the logging of the `h2` element inside the shadow DOM will result in an error.
This is because when you set the mode to `open`, the shadow DOM is accessible from JavaScript outside the element. When set it to `closed`, the shadow DOM is not accessible from JavaScript outside the element.

This is a important feature of the shadow DOM that allows us to create components that are truly encapsulated from the rest of the page.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 2](./assignment2.md) | :arrow_up: [Assignments](./README.md) | [Assignment 4](./assignment4.md) :arrow_forward:
