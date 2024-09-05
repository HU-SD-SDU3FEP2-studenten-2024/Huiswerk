# Assignment 3: Reactive Properties

That we have seen in the previous example, lit takes care of the attributeChangedCallback method for us. So when the value of the property changes, lit will automatically update the DOM.
But what happens if we change the value of a property in the class? Let's find out.

Create a file `/view/components/lit-property-demo.js` and give it the following content:

```javascript
import { LitElement, css, html } from 'lit';

export class LitPropertyDemo extends LitElement {
  static styles = css`
    p {
      color: blue;
    }
  `;

  static properties = {
    property1: { 
      type: String,
      attribute: false,
    },
  };

  constructor() {
    super();
    this.property1 = 'initial value';
    this.property2 = 'initial value';
  }

  changePropertiesVariant1() {
    // wait for 1 second and then change the value of both properties.
    setTimeout(() => {
      this.property1 = 'Variant 1';
      this.property2 = 'Variant 1';
      console.log(`property1: ${this.property1}, property2: ${this.property2}`);
    }, 1000);
  }

  changePropertiesVariant2() {
    // two independent timeouts of each 1 second. Each timeout will change the value of one property.
    setTimeout(() => {
      this.property1 = 'Variant 2';
      console.log(`property1: ${this.property1}, property2: ${this.property2}`);
    }, 1000);
    setTimeout(() => {
      this.property2 = 'Variant 2';
      console.log(`property1: ${this.property1}, property2: ${this.property2}`);
    }, 1000);
  }

  changePropertiesVariant3() {
    // two independent timeouts of 1 and 1.5 seconds. Each timeout will change the value of one property, but property1 wil be changed before property2.
    setTimeout(() => {
      this.property1 = 'Variant 3';
      console.log(`property1: ${this.property1}, property2: ${this.property2}`);
    }, 1000);
    setTimeout(() => {
      this.property2 = 'Variant 3';
      console.log(`property1: ${this.property1}, property2: ${this.property2}`);
    }, 1500);
  }

  changePropertiesVariant4() {
    // two independent timeouts of 1.5 and 1 seconds. Each timeout will change the value of one property, but property2 wil be changed before property1.
    setTimeout(() => {
      this.property1 = 'Variant 4';
      console.log(`property1: ${this.property1}, property2: ${this.property2}`);
    }, 1500);
    setTimeout(() => {
      this.property2 = 'Variant 4';
      console.log(`property1: ${this.property1}, property2: ${this.property2}`);
    }, 1000);
  }

  render() {
    return html`
      <h2>Lit Property Demo</h2>
      <p>Property 1: ${this.property1}</p>
      <p>Property 2: ${this.property2}</p>

      <button @click=${this.changePropertiesVariant1}>Change Property Variant 1</button>
      <button @click=${this.changePropertiesVariant2}>Change Property Variant 2</button>
      <button @click=${this.changePropertiesVariant3}>Change Property Variant 3</button>
      <button @click=${this.changePropertiesVariant4}>Change Property Variant 4</button>
    `;
  }
}

customElements.define('lit-property-demo', LitPropertyDemo);

```

Notice that in this assignment we have defined the property of `property1` as a string that is not an attribute of the tag, because we set the `attribute` property to `false`.

Note also that we have added a second property `property2` of the class. But that property we have defined only in the constructor not in the `properties` part of the code.
Using the four buttons we can change the value of the properties. Each button will change the value of the properties in a different way. The console log will show the values of the properties after each change.

To use the `lit-property-demo` tag in our `index.html` file, add the following line to the bottom of the `/src/view/pages/home-page.js` file:

```javascript
import '../components/lit-property-demo';
```

and add the following code to the body of the `index.html` file:

```html
<lit-property-demo></lit-property-demo>
<hr />
```

Start the server and open the browser. You should see the text `Property 1: initial value` and `Property 2: initial value` below the `Lit Property Demo` heading. Open the console in the browser to see the values of the property after each change.
Try out the four different variants of changing the properties and see how the values change and observe the value of the properties in the console and compare them to the values shown in the browser.

This mechanism of updating the DOM when a property changes is called reactivity. Lit uses a reactive programming model to update the DOM when a property changes. This is a powerful feature of lit that makes it easy to create dynamic web applications.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 2](./assignment2.md) | :arrow_up: [Assignments](./README.md) | [Assignment 4](./assignment4.md) :arrow_forward:
