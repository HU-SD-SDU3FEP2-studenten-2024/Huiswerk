# Assignment 3: Page Reload

One solution to solve our problem is to trigger the `connectedCallback` method of the `todo-time` webcomponent when a repair assignment is removed. This can be done by reloading the page.

To do so we need edit the file `/src/view/components/repairassignment-list-item.js`, since this is the place were a repair assignment is removed.

Here we have to make changes to the `removeAssignment` method.

```javascript
  removeAssignment() {
    repairController.removeRepairAssignment(this.id).then(() => {
      this.remove();
      Router.go(import.meta.env.BASE_URL);
    });
  }
```

Adding the Router.go method should reload the page after a repair assignment is removed.
However `this.remove()` will remove the element from the DOM, so the `Router.go` method will not be executed.
So we have to remove the `this.remove()` method. Which is no issue since we are reloading the page anyway.

```javascript
  removeAssignment() {
    repairController.removeRepairAssignment(this.id).then(() => {
      Router.go(import.meta.env.BASE_URL);
    });
  }
```

But now we have a problem. The `Router.go` method will not reload the page. This is because we are already on the page we are trying to navigate to. So for efficiency reasons, the `Router.go` method will not reload the page.

To solve this problem we can use the `window.location.reload()` method.

```javascript
  removeAssignment() {
    repairController.removeRepairAssignment(this.id).then(() => {
      window.location.reload()
    });
  }
```

This indeed triggers a reload of the page. But it will also reset the state of the page, which we experience by having to login again. Except for the lost of the state of the page, you can see in the network tab of the developer tools that all files that are needed gets reloaded again, which might take time.

So a page reload is not a good solution.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 2](./assignment2.md) | :arrow_up: [Assignments](./README.md) | [Assignment 4](./assignment4.md) :arrow_forward:
