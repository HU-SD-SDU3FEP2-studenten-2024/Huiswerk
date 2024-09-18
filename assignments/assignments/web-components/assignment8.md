# Assignment 8: HTML Templates and Slots

Imagine the we want to add a longer description to a student of the last assignment. We could add a new property to the student object, but the description might be quite long and contain quotes or other special characters. That would result in a ugly stringified attribute.

So lets try what happens if we add the description in between the `<template-demo>` and `</template-demo>` tags.

```html
<template-demo
  name="Debug Doodle"
  id="666"
  birthdate="1-1-1970"
  imagesrc="./images/ict-student.jpg"
>
  <p>
    Debug Doodle is a quirky and enthusiastic computer science student known for
    his wild, unkempt hair that seems to have a mind of its own. Always seen
    with a mischievous grin, Debug is the go-to person for solving the trickiest
    coding problems. His student card photo captures his playful spirit
    perfectly, with his eyes twinkling behind a pair of oversized glasses. Debug
    loves to crack jokes about algorithms and often doodles funny cartoons of
    his professors during lectures. Despite his lighthearted nature, he’s a whiz
    at debugging code and can turn a chaotic program into a masterpiece in no
    time. Debug Doodle is the heart and soul of the computer lab, always ready
    to lend a hand or share a laugh. 😄
  </p>
</template-demo>
```

If you add the above code to the `index.html` file, and you inspect the result within the browser, you can find the <p>-tag and it's content, within the template-demo tag, but after the shadow-root. However, you will not see the content on the page. This is because the content between the tags is not rendered by the web component.

To render the content between the tags we need to use slots. Slots are placeholders in the shadow DOM that allow you to pass content from the light DOM to the shadow DOM. Slots are defined in the shadow DOM using the `<slot>` element.

Adding a slot is quite simple. You just add a `<slot>` element to the shadow DOM where you want the content to be rendered. The slot element can have a name attribute that allows you to define multiple slots in the shadow DOM.
Since we are using a template, we don't have to change our JS code. We can just add the slot element to the template in our html.

```html
    <template id="student-template">
      <img src="" alt="studentimage" />
      <aside>
        <h2>Student Template</h2>
        <p id="student-id"></p>
        <p id="student-birthdate"></p>
        <slot name="description"></slot>
      </aside>
    </template>
```

To use the slot, all we have to do is add an attribute to the content we want to pass to the slot. The attribute should be named the same as the slot name. In our case, the slot name is "description".

```html
      <template-demo
        name="Debug Doodle"
        id="666"
        birthdate="1-1-1970"
        imagesrc="./images/ict-student.jpg"
      >
        <p slot="description">
          Debug Doodle is a quirky and enthusiastic computer science student
          known for his wild, unkempt hair that seems to have a mind of its own.
          Always seen with a mischievous grin, Debug is the go-to person for
          solving the trickiest coding problems. His student card photo captures
          his playful spirit perfectly, with his eyes twinkling behind a pair of
          oversized glasses. Debug loves to crack jokes about algorithms and
          often doodles funny cartoons of his professors during lectures.
          Despite his lighthearted nature, he’s a whiz at debugging code and can
          turn a chaotic program into a masterpiece in no time. Debug Doodle is
          the heart and soul of the computer lab, always ready to lend a hand or
          share a laugh. 😄
        </p>
      </template-demo>
```

Inspecting the result in the browser, you will see the content is now rendered on the page. The slotted content is not part of the shadow DOM, but a sub node of the web component. So if we want to style the slotted content from within our template-demo webcomponent, we have to use the `::slotted()` pseudo-element.

```css
    ::slotted(p) {
      color: blue;
    }
```

But we can also style the slotted content fromt the outside. Just add a style tag to the index.html file and add the following css.

```css
    template-demo p {
      color: blue;
    }
```

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 7](./assignment7.md) | :arrow_up: [Assignments](./README.md) | [Assignment 9](./assignment9.md) :arrow_forward:
