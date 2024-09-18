# Assignment 5: Custom Event

In the last assignment we have seen that the mediator pattern uses a custom event to communicate between web components.
But why not use a custom event directly to exchange data between web components? They have a detail property that can be used to pass data. But what data should we pass?

We could pass the total repair time as a detail of the custom event. But that would mean that the `RepairAssignmentListItem` component would have to calculate the total repair time. This is not a good idea, because the `RepairAssignmentListItem` component should not be concerned with the total repair time.

So the alternative is to just let the `todo-time` component know that a repair assignment has been removed. The `todo-time` component can then recalculate the total repair time.

This means that to apply this solution we need to add a custom event to the `RepairAssignmentListItem` component that is triggered when a repair assignment is removed.

```javascript
  removeAssignment() {
    repairController.removeRepairAssignment(this.id).then(() => {
      this.dispatchEvent(new Event('assignmentRemoved', { bubbles: true, composed: true }));
      this.remove();
    });
  }
```

And we have to listen to this event in the `todo-time` component.

```javascript
  connectedCallback() {
    super.connectedCallback();
    this.updateRepairTime();
    document.addEventListener('assignmentRemoved', this.updateRepairTime.bind(this));
  }
```

And since we are adding an eventlistener we also should make sure to remove it when the component is disconnected.

```javascript
  disconnectedCallback() {
    document.removeEventListener('assignmentRemoved', this.updateRepairTime.bind(this));
    super.disconnectedCallback();
  }
```

Note that we have to listen to the document object, because the event gets dispatched from the `RepairAssignmentListItem` component, and the `todo-time` component is not a parent of the `RepairAssignmentListItem` component. Instead of the document, we could also have added the event listener to the `home-page` component, because that is their common parent. But the document object is a more general robust solution.

## Considerations

If you tryout this solution you will see that the total repair time is updated when a repair assignment is removed. But is it a good solution?

Events are a valid way to communicate between web components. But they can make the code harder to understand, because the event listeners are not directly connected to the event dispatchers. This makes it harder to follow the flow of the data and they are also harder to debug.
Using customEvents might cause memory leaks if the event listeners are not removed when the component is disconnected.

We also should look at the architecture aspect of this solution. Why should the `todo-time` component know that a repair assignment is removed? It's not the responsibility of the `todo-time` component to know about the removal of a repair assignment. This is a violation of the single responsibility principle. It's the data that changes, so it's not a responsibility of the **view-layer** to keep track of the data. That is a responsibility that should be as close to the data as possible. In our case that would be responsibility of the **service-layer**.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 4](./assignment4.md) | :arrow_up: [Assignments](./README.md) | [Assignment 5](./assignment5.md) :arrow_forward:
