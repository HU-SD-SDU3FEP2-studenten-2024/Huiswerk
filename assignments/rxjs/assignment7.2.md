# Assignment 7.2: Demo Project - Subject

The Subject Observable is a special type of Observable that allows values to be multicasted to many Observers. While plain Observables are unicast (each subscribed Observer owns an independent execution of the Observable), Subjects are multicast.

In our demo project the `controller-layer` is the only user of the `service-layer`. Therefore a plain Observable is sufficient. But in the view-layer we have multiple components that need to listen to the same data. Therefore we need a Subject Observable.

So let's change the code of the `controller-layer.js` file in our demo project to use a Subject Observable.

```javascript
import { serviceLayer } from '../service/service-layer';
import { Subject } from 'rxjs';

class ControllerLayer {

  controllerData$ = new Subject();

  constructor(serviceLayerInstance) {
    this.serviceLayer = serviceLayerInstance;
    const observer = {
      next: (data) => this.dataObserverHandler(data),
      error: (error) => console.error('ControllerLayer.error()', error),
      complete: () => console.log('ControllerLayer.complete()')
    }
    this.serviceLayer.data$.subscribe(observer);
  }

  dataObserverHandler(data) {
    console.log('ControllerLayer.dataObserverHandler()', data);
    this.controllerData$.next(data);
  };

  addData(data) {
    this.serviceLayer.addData(data);
  }
}

const controllerLayer = new ControllerLayer(serviceLayer);

export { controllerLayer }
```

As you can see there are only a few changes in the code.
We now import the `Subject` instead of `Observable` from the `rxjs` library.
Next we changed the initialisation of our `controllerData$` property to a `Subject` instance. Unlike with `Observable` we don't have to initialize it by passing the subscribe function to the constructor.

The next change is in the `dataObserverHandler` method. Instead of calling the `next` method on the subsriber of the `Observable` instance we call the `next` method on the `Subject` instance itself.

And that is all we need to change to use a `Subject` instead of an `Observable`. A change of the view-layer is not necessary because the `Subject` is a drop-in replacement for the `Observable`.

When we now run the demo project we see that one component shows the data that is added by the other component, while the other component only shows the data that is added by itself.
This is because the component that only shows the data that is added by itself didn't receive the data that was already added by the other component before it was initialized.
But they both receive the data that is added after they are initialized.

We could fix the initial value by using a `BehaviorSubject` instead of a `Subject`. A `BehaviorSubject` is a `Subject` that requires an initial value and emits its current value to new subscribers.

```javascript
import { serviceLayer } from '../service/service-layer';
import { BehaviorSubject } from 'rxjs';

class ControllerLayer {

  controllerData$ = new BehaviorSubject('initial data');

...
```

The behavior of the `BehaviorSubject` is the same as the `Subject` except for the initial value. The `BehaviorSubject` will emit the initial value to all subscribers when they subscribe. This is why now one component shows the three items and the other component only shows two items.
But if we would store all items within a property of the `controller-layer` we could initialize the `BehaviorSubject` with that datastore and all components would show all items.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 7.1](./assignment7.1.md) | :arrow_up: [Assignments](./README.md) | [Assignment 7.3](./assignment7.3.md) :arrow_forward:
