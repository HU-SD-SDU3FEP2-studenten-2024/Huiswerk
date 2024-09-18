# Assignment 7.3: Demo Project - Operators

RxJS Operators are functions that build on the observables foundation to enable sophisticated manipulation of collections. They are the essential pieces that allow complex asynchronous code to be easily composed in a declarative manner.

`of` and `from` are creation operators. They are used to create observables from values, arrays, promises, etc.

```javascript
import { serviceLayer } from '../service/service-layer';
import { of } from 'rxjs';

class ControllerLayer {

  controllerData$ = new of('initial data');

...
```

The example above would create an observable that emits the value `'initial data'` when subscribed to and automatically emits the `'complete'` signal after that.
Our view components therefore would only show the `'initial data'` and not the data that is added by the other component.

Other operators can be used to manipulate the data that is emitted by the observable. There are operators to filter, map, reduce, etc. the data. To find out more about the operators you can visit the [RxJS documentation](https://rxjs-dev.firebaseapp.com/guide/operators) and see https://rxmarbles.com/ or https://rxjsmarbles.dev/ for visual explanations called `RxJS Marbels` of the operators.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 7.2](./assignment7.2.md) | :arrow_up: [Assignments](./README.md) | [Assignment 7.4](./assignment7.4.md) :arrow_forward:
