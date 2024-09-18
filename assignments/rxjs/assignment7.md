# Assignment 7: RxJS

In the previous two assignments we stated that it is not the responsibility of the **view-layer** to keep track of the data. That is a responsibility that should be as close to the data as possible. In our case that would be responsibility of the **service-layer**.

So we should dispatch an event from the **service-layer** when data changes. But if we would use CustomEvents for this we would have to bind the CustomEvent to a DOM element. But the **service-layer** should not be aware of the DOM. Using CustomEvents therefore would violate our architecture.

But there is a solution that is more in line with our architecture and that is using RxJS. RxJS is a library for reactive programming using Observables, to make it easier to compose asynchronous or callback-based code.

Observables are as the name suggests, observable. They are like a stream of data that we can listen to and react on. While CustomEvents are a push mechanism, Observables are a pull mechanism. This means that we can pull data from the Observable when we need it.

RxJS Observables are therefore a good way to communicate between layers in our architecture. The **service-layer** can create an Observable that emits a value when the data changes. The upper layer can then listen to this Observable and react on the emitted value.

Let's learn more about RxJS with a small demo project (Assignments [7.1](./assignment7.1.md), [7.2](./assignment7.2.md) and [7.3](./assignment7.3.md)), before we apply the knowledge to our bike repair shop project ([Assignment 7.4](./assignment7.4.md)).

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 6](./assignment6.md) | :arrow_up: [Assignments](./README.md) | [Assignment 7.1](./assignment7.1.md) :arrow_forward:
