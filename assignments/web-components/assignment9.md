# Assignment 9: Hello Nested Components

A common pattern is a list of items that we want to display using webcomponents. This is normally done by creating at least two components: one for the list and one for the items in the list.
In this assignment, we will reuse the template-demo component of the last assignment as a listitem component.
So we only need to create a list component that uses the template-demo component to display a list of items.

```javascript
import './template-demo.js';

export class ListDemo extends HTMLElement {
  constructor() {
    super();

    this.shadowDom = this.attachShadow({ mode: 'closed' });
    this.students = [];
  }

  connectedCallback() {
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

    const css = `
      <style>
        :host {
          display: grid;
          gap: 1rem;
          padding: 1rem;
          background-color: lightblue;
          border-radius: 1rem;
          margin: 1rem;
        }

      </style>
    `;

    this.shadowDom.innerHTML = css;

    const h2Element = document.createElement('h2');
    h2Element.textContent = 'Student List';
    this.shadowDom.appendChild(h2Element);

    const articleElement = document.createElement('article');
    articleElement.classList.add('student-list');
    this.shadowDom.appendChild(articleElement);

    this.render();
  }

  render() {
    const articleElement = this.shadowDom.querySelector('.student-list');
    articleElement.innerHTML = '';

    this.students.forEach((student) => {
      const templateElement = document.createElement('template-demo');
      templateElement.setAttribute('name', student.name);
      templateElement.setAttribute('id', student.id);
      templateElement.setAttribute('birthdate', student.birthdate);
      templateElement.setAttribute('imagesrc', student.imagesrc);
      const pElement = document.createElement('p');
      pElement.setAttribute('slot', 'description');
      pElement.textContent = student.description;
      templateElement.appendChild(pElement);
      articleElement.appendChild(templateElement);
    });
  }
}

customElements.define('list-demo', ListDemo);
```

```html
<list-demo></list-demo>
```

Note that this component first imports the template-demo component. Doing so allows us to use the template-demo component in the list-demo component.

The list-demo component will display a list of students. It is common to initialize the list of students in the constructor with an empty list.
The list itself normally gets populated with data in the connectedCallback method. In this case, we have hardcoded the data in the connectedCallback method, but it is more common to get the data from the controller who receives the data from the service, which uses the fetch API to get the data from the server.

Once the data is available, the render method is called to display the list of students. The render method first clears the list of students and then iterates over the list of students to create a template-demo component for each student. The template-demo component is then appended to the list of students.

To complete this assignment, we have to import the list-demo component in the `/src/view/pages/home-page.js`.

```javascript
import '../components/list-demo.js';
```

Note that wouldn't we use the template-demo tag in the html file, we wouldn't have to import the template-demo component in the `home-page.js` file, since we already imported it in the `list-demo` file.

And add the list-demo to the `index.html` file.

```html
<list-demo></list-demo>
```

If you have done everything correctly, you should see a list of students on the home page.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 8](./assignment8.md) | :arrow_up: [Assignments](./README.md) | [Assignment 10](./assignment10.md) :arrow_forward:
