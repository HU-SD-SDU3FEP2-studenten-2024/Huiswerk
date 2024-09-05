# Assignment 1: Hello World

Web components are custom HTML elements that can be used in the same way as standard HTML elements.
In this assignment we will create a simple web component that provides us with a `<hello-world>` tag that displays the text `Hello World!` on the page.

Create a file `/view/components/hello-world.js` and give it the following content:

```javascript
export class HelloWorld extends HTMLElement {
  constructor() {
    super();
    this.name = 'World';
  }

  connectedCallback() {
    this.render();
  }

  render() {
    this.textContent = `Hello ${this.name}!`;
  }
}

customElements.define('hello-world', HelloWorld);
```

Note that in the class definition we extend the `HTMLElement` class. This makes our class a web component. The `HTMLElement` class is a built-in class in the browser that represents an HTML element. By extending this class, our class inherits all the properties and methods of the `HTMLElement` class. This makes it possible to use our class as a custom HTML element (for more information see [HTMLElement on MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement)).

Note the last line of the file, where we define the custom element `hello-world` and associate it with the `HelloWorld` class. This makes it possible to use the `hello-world` tag in our HTML when we import this file.
You are free to name the class and the tag as you like, but it is a good practice to use the same name for both. And it is a good practice to use at least one dash in the tag name to avoid conflicts with future standard HTML elements.

Next we need to import our component in the `/src/view/pages/home-page.js` file. Add the following line to the top of the file:

```javascript
import '../components/hello-world.js';
```

Now we can use the `hello-world` tag in our `index.html` file. Add the following line to the body of the file below the `h1` tag:

```html
<section>
  <h2>Hello World</h2>
  <hello-world></hello-world>
</section>

<hr />
```

Start the server and open the browser. You should see the text `Hello World!` below the `Hello World` heading.
Lookup the html in the developer tools of the browser to see the custom element.

Note that in the HTML we only use the tag `<hello-world>`. Because we have defined the custom element `hello-world` in the last line of the `hello-world.js` file, the browser knows this tag. The browser will create an instance of the `HelloWorld` class (using the `constructor`) and automatically call the `connectedCallback` method. The `connectedCallback` method calls the `render` method, which sets the text content of the element to `Hello World!`.

---

:house: [Home](../../README.md) | :arrow_up: [Assignments](./README.md) | [Assignment 2](./assignment2.md) :arrow_forward:
