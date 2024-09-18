# Assignment 1: Hello Lit

Lit is a simple library for building fast, lightweight web components. It is based on the Web Components standard, which is supported by all modern browsers.

In this assignment we will create a simple lit based web component that provides us with a `<hello-lit>` tag that displays the text `Hello Lit` on the page.

Create a file `/view/components/hello-lit.js` and give it the following content:

```javascript
import { LitElement, css, html } from 'lit';

export class HelloLit extends LitElement {
  static styles = css`
    p {
      color: blue;
    }
  `;

  constructor() {
    super();
    this.name = 'Lit';
  }

  render() {
    return html`
      <h2>Hello Lit</h2>
      <p>Hello ${this.name}</p>
    `;
  }
}

customElements.define('hello-lit', HelloLit);
```

Note that in the class definition we extend the `LitElement` class, which we have to import from `lit`. This makes our class a lit based web component. For more information on lit see [lit.dev](https://lit.dev/).

Note also that we imported `html` en `css` from lit. These are tagged template literals that make it easy to create HTML and CSS in JavaScript.

The `css` tagged template literal is used to define the styles of the component. Which you can use in the `styles` static property of the class.

The `html` tagged template literal is used to define the template of the component. The `html` tagged template literal returns a `TemplateResult` object, which is a lightweight representation of a DOM tree.
As you can see we use the `html` tagged template literal in the `render` method to define the template of the component.

Note that we don't call the `render` method. The `LitElement` class takes care of calling the `render` method when needed. With Lit we never call the `render` method ourselves!!!

Note the last line of the file, where we define the custom element `hello-lit` and associate it with the `HelloLit` class. This makes it possible to use the `hello-lit` tag in our HTML when we import this file.

Next we need to import our component in the `/src/view/pages/home-page.js` file. Add the following line to the top of the file:

```javascript
import '../components/hello-lit';
```

Now we can use the `hello-lit` tag in our `index.html` file. Add the following line to the body of the file below the `h1` tag:

```html
<hello-lit></hello-lit>
<hr />
```

Start the server and open the browser. You should see the text `Hello Lit` below the `Hello Lit` heading.
Lookup the html in the developer tools of the browser to see the custom element.
Note that Lit uses the `shadow DOM` to encapsulate the styles and the template of the component. This means that the styles and the template of the component are not affected by the styles and the template of the rest of the page.

---

:house: [Home](../../README.md) | :arrow_up: [Assignments](./README.md) | [Assignment 2](./assignment2.md) :arrow_forward:
