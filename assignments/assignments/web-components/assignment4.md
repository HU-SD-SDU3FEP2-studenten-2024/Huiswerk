# Assignment 4: Hello Shadowdom CSS

Since we cannot style our web component from the outside, we need to style it from the inside. In this assignment we will create a web component that contains a css style tag in the shadow DOM.

```javascript
export class HelloShadowDomCss extends HTMLElement {
  constructor() {
    super();

    this.shadowDom = this.attachShadow({ mode: 'closed' });
    this.name = 'ShadowWorld with CSS!';
  }

  connectedCallback() {
    const css = `
      <style>
        h2 {
          background-color: blue;
          color: white;
        }
      </style>
    `;
    const sectionElement = document.createElement('section');
    const h2Element = document.createElement('h2');

    sectionElement.appendChild(h2Element);
    this.shadowDom.innerHTML = css;
    this.shadowDom.appendChild(sectionElement);

    this.render();
  }

  render() {
    this.shadowDom.querySelector('h2').textContent = `Hello ${this.name}!`;
  }
}

customElements.define('hello-shadowdom-css', HelloShadowDomCss);
```

Next we need to import our component in the `/src/view/pages/home-page.js` file. Add the following line to the bottom of imports within the file:

```javascript
import '../components/hello-shadowdom-css.js';
```

Now we can use the `hello-shadowdom-css` tag in our `index.html` file. Add the following code to the body of the file:

```html
<hello-shadowdom-css></hello-shadowdom-css>

<hr />
```

If we inspect the result in the browser we should see the text `Hello ShadowWorld with CSS!` with a blue background and white text color. This is because the styles are now defined in the shadow DOM and are not affected by the styles of the rest of the page.

Unfortunately, the shadow DOM does not support the `link` tag, so we need to use the `style` tag to add CSS to the shadow DOM.
And since there is also no standard way to incluse HTML in JavaScript, we no longer have a nice separtion of concern between HTML, CSS and JavaScript. This is a limitation of web components that we have to live with. Some frameworks like Angular and Vue.js have found ways to overcome this limitation.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 3](./assignment3.md) | :arrow_up: [Assignments](./README.md) | [Assignment 5](./assignment5.md) :arrow_forward: