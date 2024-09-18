# Assignment 7: HTML Templates

Note that this assignment uses two images within the `/public/images` folder (`anomynous-male-front.jpg` and `ict-student.jpg`). In case you don't have these images you can use any other image you like.

Let's start by first adding a template to the `index.html` file.
The concept of HTML templates and how to clone and use them have been introduced in FEP1.

```html
<template id="student-template">
  <img src="" alt="studentimage" />
  <aside>
    <h2>Student Template</h2>
    <p id="student-id"></p>
    <p id="student-birthdate"></p>
  </aside>
</template>
```

Next we will create a new web component called `template-demo` in the `/src/view/components` folder. This component will use the template above to render a student object. The student object will have the following properties: `name`, `id`, `birthdate`, and `imagesrc`. The `imagesrc` property will be used to set the `src` attribute of the image element in the template.

```javascript
export class TemplateDemo extends HTMLElement {
  constructor() {
    super();

    this.shadowDom = this.attachShadow({ mode: 'closed' });
    this.student = {
      name: 'John Doe',
      id: '1234',
      birthdate: '01/01/2000',
      imagesrc: './images/anomynous-male-front.jpg',
    };
  }

  static get observedAttributes() {
    return ['name', 'id', 'birthdate', 'imagesrc'];
  }

  attributeChangedCallback(attributename, oldValue, newValue) {
    if (attributename === 'name') {
      this.student.name = newValue;
      this.render();
    }
    if (attributename === 'id') {
      this.student.id = newValue;
      this.render();
    }
    if (attributename === 'birthdate') {
      this.student.birthdate = newValue;
      this.render();
    }
    if (attributename === 'imagesrc') {
      this.student.imagesrc = newValue;
      this.render();
    }
  }

  connectedCallback() {
    const css = `
      <style>
        :host {
          display: grid;
          grid-template-columns: auto 1fr;
          gap: 1rem;
          background-color: peachpuff;
          padding: 1rem;
          border-radius: 1rem;
          margin: 1rem;
        }

        img {
          width: 300px;
          height: 300px;
        }

        h2 {
          background-color: orange;
        }
      </style>
    `;

    const template = document.querySelector('#student-template');
    const templateClone = template.content.cloneNode(true);

    this.shadowDom.innerHTML = css;
    this.shadowDom.appendChild(templateClone);

    this.render();
  }

  render() {
    this.shadowDom.querySelector('h2').textContent = `${this.student.name}`;
    this.shadowDom.querySelector('#student-id').textContent =
      `ID: ${this.student.id}`;
    this.shadowDom.querySelector('#student-birthdate').textContent =
      `Age: ${this.student.birthdate}`;
    this.shadowDom.querySelector('img').src = this.student.imagesrc;
    this.shadowDom.querySelector('img').alt = this.student.name;
  }
}

customElements.define('template-demo', TemplateDemo);
```

Don't forget to import the component in the `home-page.js` file.

```javascript
import '../components/template-demo.js';
```

Finally, add the `template-demo` element to the `index.html` file.

```html
<template-demo></template-demo>
<template-demo
  name="Debug Doodle"
  id="666"
  birthdate="1-1-1970"
  imagesrc="./images/ict-student.jpg"
></template-demo>
<hr/>
```

In the browser you should see two student cards, one with the default values and one with the values provided as attributes.
Unfortunately we will also see some errors in the console. These are related to the lifecycle of the web component. We will address these in a later assignment.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 6](./assignment6.md) | :arrow_up: [Assignments](./README.md) | [Assignment 8](./assignment8.md) :arrow_forward: