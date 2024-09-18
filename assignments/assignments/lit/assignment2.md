# Assignment 2: Lit Attributes

As we saw in the web component assignment, attribute values can be set on custom elements in the same way as on standard HTML elements. In this assignment we will create a lit based web component that displays a greeting with a name that can be set as an attribute.

Create a file `/view/components/hello-lit-attribute.js` and give it the following content:

```javascript
import { LitElement, css, html } from 'lit';

export class HelloLitAttributes extends LitElement {
  static styles = css`
    p {
      color: blue;
    }
  `;

  static properties = {
    name: { 
      type: String,
      reflect: false 
    },
    id: { 
      type: Number, 
      reflect: true 
    },
  };

  constructor() {
    super();
    this.name = 'Lit';
  }

  changeValues() {
    this.name = 'Someone';
    this.id = -1;
  }

  render() {
    return html`
      <h2>Hello Lit Attributes</h2>
      <p>Hello ${this.name} (${this.id})</p>
      <p>Types of name: ${typeof this.name}</p>
      <p>Types of id: ${typeof this.id}</p>
      <button @click=${this.changeValues}>
        from now on you should be known as
      </button>
    `;
  }
}

customElements.define('hello-lit-attributes', HelloLitAttributes);

```

Note that unlike vanilla web component classes, lit based web component classes have a `properties` static property. This property is used to define the properties of the component.
In this case we define a single property `name` of type `String` and a second property 'id' of type 'Number'.
We also added the optional `reflect` properties. We will discover the effect at the end of this assignment.

Note also that with lit we don't need the attributeChangedCallback method to react to changes in attributes. Lit takes care of this for us.

How the button works is not important for this assignment. It is just there to demonstrate that the values of the properties can be changed.

Add the following line to the bottom of the `/src/view/pages/home-page.js` file:

```javascript
import '../components/hello-lit-attribute';
```

Now we can use the `hello-lit-attributes` tag in our `index.html` file. Add the following code to the body of the file:

```html
<hello-lit-attributes name="Student" id="123456"></hello-lit-attributes>
<hr />
```

Start the server and open the browser. You should see the text `Hello Student` with id `123456` below the `Hello Lit Attributes` heading. Change the value of the `name` attribute in the `index.html` file to see the text change.
Notice that the type of the attributes is converted automatically to the type defined in the `properties` static property.

Now inspect the element in the development tools of the browser and click on the button. You will see that the value of the `name` attribute changes, but the value of the `id` attribute does not change. This is because the `reflect` property of the `id` property is set to `true`. This means that the value of the `id` property is reflected in the attribute. The value of the `name` property is not reflected in the attribute.
One example in which this reflection is useful is when we want to use the value of the attribute in CSS. We could create a CSS rule that uses the value of the `id` attribute to style the element.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 1](./assignment1.md) | :arrow_up: [Assignments](./README.md) | [Assignment 3](./assignment3.md) :arrow_forward:
