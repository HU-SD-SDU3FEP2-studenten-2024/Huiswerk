# Assignment 5: Rendering Lit Components

In this assignment we will create a list component. The list component will be composed of list items. Each list item will display a student's name, id, birthdate, image, and description. The list component will be responsible for rendering the list items.

Start by creating a new file `lit-list-item.js` and add the following code:

```javascript
import { LitElement, html, css } from 'lit';

export class LitListItem extends LitElement {
  static styles = css`
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

    ::slotted(p) {
      color: blue;
    }
  `;

  static properties = {
    name: { type: String },
    id: { type: Number },
    birthdate: { type: String },
    imagesrc: { type: String },
  };

  constructor() {
    super();
    this.name = 'John Doe';
    this.id = -1;
    this.birthdate = '01/01/2000';
    this.imagesrc = './images/anomynous-male-front.jpg';
  }

  render() {
    return html`
      <img src="${this.imagesrc}" alt="${this.name}" />
      <aside>
        <h2>${this.name}</h2>
        <p id="student-id">ID: ${this.id}</p>
        <p id="student-birthdate">Age: ${this.birthdate}</p>
        <slot></slot>
      </aside>
    `;
  }
}

customElements.define('lit-list-item', LitListItem);
```

Note that in the `render` method we use the `slot` element to render the description of the student. This allows us to pass the description as a child element of the `lit-list-item` element.

Now we will create the list component. We therefore create a new file `lit-list.js` and add the following code:

```javascript
import { LitElement, html, css } from 'lit';
import { LitListItem } from './lit-list-item';

export class LitList extends LitElement {
  static styles = css`
    :host {
      display: grid;
      gap: 1rem;
      padding: 1rem;
      background-color: lightblue;
      border-radius: 1rem;
      margin: 1rem;
    }
  `;

  static properties = {
    students: { type: Array },
  };

  constructor() {
    super();
    this.students = [];
  }

  connectedCallback() {
    super.connectedCallback();
    this.students = [
      {
        name: 'John Doe',
        id: '1234',
        birthdate: '01/01/2000',
        imagesrc: './images/anomynous-male-front.jpg',
        description:
          'John is an ICT student, but there is not much information about him.',
      },
      {
        name: 'Jane Doe',
        id: '5678',
        birthdate: '02/02/2000',
        imagesrc: './images/anomynous-female.jpg',
        description:
          'Jane is an ICT student, but there is not much information about here.',
      },
      {
        name: 'Debug Doodle',
        id: '9012',
        birthdate: '1/1/1970',
        imagesrc: './images/ict-student.jpg',
        description:
          'Debug Doodle is a quirky and enthusiastic computer science student known for his wild, unkempt hair that seems to have a mind of its own. Always seen with a mischievous grin, Debug is the go-to person for solving the trickiest coding problems. His student card photo captures his playful spirit perfectly, with his eyes twinkling behind a pair of oversized glasses. Debug loves to crack jokes about algorithms and often doodles funny cartoons of his professors during lectures. Despite his lighthearted nature, he’s a whiz at debugging code and can turn a chaotic program into a masterpiece in no time. Debug Doodle is the heart and soul of the computer lab, always ready to lend a hand or share a laugh. 😄',
      },
    ];
  }

  render() {
    return html`
      <h2>Student List</h2>
      ${this.students.map(
        (student) => html`
          <lit-list-item
            name=${student.name}
            id=${student.id}
            birthdate=${student.birthdate}
            imagesrc=${student.imagesrc}
          >
            <p>${student.description}</p>
          </lit-list-item>
        `
      )}
    `;
  }
}

customElements.define('lit-list', LitList);
```

The `lit-list` component has a property `students` that is an array of objects. Each object represents a student. The `lit-list` component is responsible for rendering the list of students. It does this by mapping over the `students` array and rendering a `lit-list-item` component for each student.
Note the we import the `LitListItem` component at the top of the file and that we use the `LitListItem` component in the `render` method.

In the `render` methode we use the `map` method to iterate over the `students` array. For each student we render a an `html` template which holds the `lit-list-item` component. We pass the student's name, id, birthdate, and image source as properties to the `lit-list-item` component. We also pass the student's description as a child element of the `lit-list-item` component.

Add the following code to the `/src/view/pages/home-page.js` file:

```javascript
import '../components/lit-list';
```

Finally, add the following code to the `index.html` file:

```html
<lit-list></lit-list>
```

Start the development server and open the browser to see the list of students rendered on the page.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 4](./assignment4.md) | :arrow_up: [Assignments](./README.md) | [Assignment 6](./assignment6.md) :arrow_forward:
