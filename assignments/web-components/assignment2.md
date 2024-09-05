# Assignment 2: Hello Attribute

Attribute values can be set on custom elements in the same way as on standard HTML elements.

In this assignment we will create a web component that displays a greeting with a name that can be set as an attribute.

Create a file named `/view/components/hello-attribute.js` and give it the following content:

```javascript
export class HelloAttribute extends HTMLElement {
  constructor() {
    super();
    this.name = 'World';
  }

  static get observedAttributes() {
    return ['name'];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    if (name === 'name') {
      this.name = newValue;
      this.render();
    }
  }

  connectedCallback() {
    this.render();
  }

  render() {
    this.textContent = `Hello ${this.name}!`;
  }
}

customElements.define('hello-attribute', HelloAttribute);
```

Add the following line to the bottom of the `/src/view/pages/home-page.js` file:

```javascript
import '../components/hello-attribute.js';
```

Now we can use the `hello-attribute` tag in our `index.html` file. Add the following code to the body of the file:

```html
<section>
  <h2>Hello Attribute</h2>
  <hello-attribute name="Student"></hello-attribute>
</section>

<hr />
```

Start the server and open the browser. You should see the text `Hello Student!` below the `Hello Attribute` heading. Change the value of the `name` attribute in the `index.html` file to see the text change. Try what happens if you remove the `name` attribute.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 1](./assignment1.md) | :arrow_up: [Assignments](./README.md) | [Assignment 3](./assignment3.md) :arrow_forward:
