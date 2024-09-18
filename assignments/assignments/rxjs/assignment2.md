# Assignment 2: Solutions

In this assignment we will solve the problem of the total repair time not being updated when a repair assignment is removed from the system.

## The problem

The reason why the total repair time is not updated when a repair assignment is removed from the system is that the `todo-time` web component does know that a repair assignment is removed. 

The time displayed by the `todo-time` webcomponent is calculated by the `getTotalRepairTime` method of the `RepairController`. This information gets updated within the `connectedCallback` method.
The `connectedCallback` is a lifecycle method that is called when the component is connected to the DOM. This method is called only once, when the component is first connected to the DOM.
But since the remove of a repair assignment takes place in another component within the same page, and doesn't trigger the whole page to be reloaded, the `connectedCallback` method of the `todo-time` webcomponent is not called again.

## The solution

Think about different ways on how you could solve this problem (before looking at the solutions below).

<details>
<summary>Click to see the solutions</summary>
To solve this problem we have several options:

- **Page Reload** : This is not a good solution, because it will reset the state of the page and often results in a bad user experience.
- **Tag Attributes** : We can add an attribute to the `todo-time` webcomponent that is updated when a repair assignment is removed. This attribute can be listened to by the `todo-time` webcomponent, which can then update the total repair time.
- **Custom Event** : We can create a custom event that is triggered when a repair assignment is removed. This event can be listened to by the `todo-time` webcomponent, which can then update the total repair time.
- **Signals** : We can use signals to notify the `todo-time` webcomponent that a repair assignment is removed. This is a more advanced solution, but it is a good way to learn about signals.
- **RxJS** : We can use RxJS to create an observable that is updated when a repair assignment is removed. This observable can be listened to by the `todo-time` webcomponent, which can then update the total repair time.

In each of the next assignments we will take a look at one of these solutions.
</details>

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 1](./assignment1.md) | :arrow_up: [Assignments](./README.md) | [Assignment 3](./assignment3.md) :arrow_forward: