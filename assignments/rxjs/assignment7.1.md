# Assignment 7.1: Demo Project - Observable

Setup a boilerplate for the demo project and install RxJS into it:

```bash
npm install rxjs
```

Within the demo project create the folders for the service-, controller- and view-layer. 
In the service-layer create a file `service-layer.js` that contains a method to add data to the data arraylist that is stored in the service-layer:

```javascript
class ServiceLayer {

  constructor() {
    this.data = {};
  }

  addData(data) {
    this.data = { ...this.data, data };
    console.log('ServiceLayer.addData()', this.data);
  }
}

const serviceLayer = new ServiceLayer();

export { serviceLayer}
```

Create the file `controller-layer.js` and add the following code:

```javascript
import { serviceLayer } from '../service/service-layer';

class ControllerLayer {
  constructor(serviceLayerInstance) {
    this.serviceLayer = serviceLayerInstance;
  }

  addData(data) {
    this.serviceLayer.addData(data);
  }
}

const controllerLayer = new ControllerLayer(serviceLayer);

export { controllerLayer }
```

In the view layer we create two folders, one for the pages and one for the components. In the components folder create a file `rxjs-component.js` and add the following code:

```javascript
import { LitElement, html, css } from 'lit';
import { controllerLayer } from '../../controller/controller-layer';

export class RxjsComponent extends LitElement {
  
    constructor() {
      super();
    }

    connectedCallback() {
      super.connectedCallback();
      controllerLayer.addData('Some data');
    }
  
    render() {
      return html`
        <h2>RxJS Component</h2>
        <p>Coming soon...</p>
      `;
    }
  }

customElements.define('rxjs-component', RxjsComponent);
```

In the pages folder create a file `rxjs-page.js` and add the following code:

```javascript
import '../components/rxjs-component.js';
```

And of course we need to import the page in an html file:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <base href="%BASE_URL%" />

    <link rel="icon" type="image/svg+xml" href="./javascript.svg" />

    <title>Lit Components</title>
    <script type="module" src="./src/view/pages/rxjs-page.js"></script>
  </head>
  <body>
    <main>
      <h1>Lit WebComponents Demo</h1>

      <rxjs-component></rxjs-component>
    </main>
  </body>
</html>
```

Running the demo project you should see the message `ServiceLayer.addData() { data: 'Some data' }` in the console. But we are not using RxJS yet.
We start bottom up, by adding an Observable to the **service-layer** that emits a value when the data changes.

We therefore need to import the Observable from RxJS in the `service-layer.js` file:

```javascript
import { Observable } from 'rxjs';
```

Next we add a property to the `ServiceLayer` class that is an Observable. We choose a name that is similar to the property that stores the data we want to observe. In this case we choose `data$`, where the `$` is a convention to indicate that it is an Observable.

```javascript
import { Observable } from 'rxjs';

class ServiceLayer {

  data$ = new Observable((subscriber) => {
    this.subscriber = subscriber;
  });

  constructor() {
    this.data = {};
  }

...
```

As can be seen from the code, our Observable expects a subscriber. The subscriber is a function that is called when the Observable is subscribed to. The subscriber function has three methods that can be called:

- `next(value)`: Emits a value from the Observable.
- `error(error)`: Emits an error from the Observable.
- `complete()`: Completes the Observable.

We can now add a method to the `ServiceLayer` class that emits a value from the Observable when our data changes:

```javascript
import { Observable } from 'rxjs';

class ServiceLayer {

  data$ = new Observable((subscriber) => {
    this.subscriber = subscriber;
  });

  constructor() {
    this.data = {};
  }

  addData(data) {
    this.data = { ...this.data, data };
    console.log('ServiceLayer.addData()', this.data);
    this.subscriber.next(data);
  }

}

const serviceLayer = new ServiceLayer();

export { serviceLayer}
```

Our **service-layer** is now ready to emit a value when the data changes. We can now subscribe to the Observable in the **controller-layer** and react on the emitted value.
Let's first write a method in the `ControllerLayer` class that should be called when the data changes:

```javascript
  dataObserverHandler(data) {
    console.log('ControllerLayer.dataObserverHandler()', data);
  };
```

Next we subscribe to the Observable in the `ControllerLayer` class. Remember that that there are three methods that can be called on the subscriber. We only need the `next` method in this case, which is the first argument of the `subscribe` method:

```javascript
import { serviceLayer } from '../service/service-layer';

class ControllerLayer {
  constructor(serviceLayerInstance) {
    this.serviceLayer = serviceLayerInstance;
    this.serviceLayer.data$.subscribe(this.dataObserverHandler.bind(this));
  }

  dataObserverHandler(data) {
    console.log('ControllerLayer.dataObserverHandler()', data);
  };

  addData(data) {
    this.serviceLayer.addData(data);
  }
}

const controllerLayer = new ControllerLayer(serviceLayer);

export { controllerLayer }
```

But a nicer way to subscribe to the Observable is to provide an object with the `next` method, which can also provide an `error` and `complete` method:

```javascript
import { serviceLayer } from '../service/service-layer';

class ControllerLayer {
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
  };

  addData(data) {
    this.serviceLayer.addData(data);
  }
}

const controllerLayer = new ControllerLayer(serviceLayer);

export { controllerLayer }
```

Running the code you should see the message `ControllerLayer.dataObserverHandler() Some data` in the console. The **controller-layer** is now listening to the **service-layer** and reacts on the emitted value.

Now we can add the **view-layer** to the mix. Like with the `service-layer` we add a property to the class that is an Observable and that emits a value when the data changes:

```javascript
import { serviceLayer } from '../service/service-layer';
import { Observable } from 'rxjs';

class ControllerLayer {

  controllerData$ = new Observable((subscriber) => {
    this.subscriber = subscriber
  });

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
    this.subscriber.next(data);
  };

  addData(data) {
    this.serviceLayer.addData(data);
  }
}

const controllerLayer = new ControllerLayer(serviceLayer);

export { controllerLayer }
```

In the `rxjs-component.js` file we can now subscribe to the `controllerData$` Observable:

```javascript
import { LitElement, html, css } from 'lit';
import { controllerLayer } from '../../controller/controller-layer';

export class RxjsComponent extends LitElement {
  
    static styles = css`
      :host {
        display: block;
        border: 5px solid blue;
        width: 70%;
        padding: 1rem;
        margin: 1rem;
      }
    `;

    static properties = {
      data: { type: Array}
    };

    constructor() {
      super();
      this.data = [];
    }

    connectedCallback() {
      super.connectedCallback();

      const observer = {
        next: (data) => this.updateData(data),
        error: (error) => console.error('RxjsComponent.error()', error),
        complete: () => console.log('RxjsComponent.complete()')
      }
      controllerLayer.controllerData$.subscribe(observer);

      controllerLayer.addData('Some data from RxjsComponent');
    }

    updateData(data) {
      this.data = [...this.data, data];
    }
  
    render() {
      console.log('RxjsComponent.render()', this.data);
      return html`
        <h2>RxJS Component</h2>
        <p>Data received:
          <ul>
            ${this.data.map((item) => html`<li>${item}</li>`)}
          </ul> 
        </p>
      `;
    }
  }

customElements.define('rxjs-component', RxjsComponent);
```

Running the code you should see the message `RxjsComponent.render() ['Some data from RxjsComponent']` in the console and should also render this information on the page. The **view-layer** is now listening to the **controller-layer** and reacts on the emitted value, great.

So let's create a second component `rxjs2-component` that also listens to the same Observable:

```javascript
import { LitElement, html, css } from 'lit';
import { controllerLayer } from '../../controller/controller-layer';

export class Rxjs2Component extends LitElement {
  static styles = css`
    :host {
      display: block;
      border: 5px solid green;
      width: 70%;
      padding: 1rem;
      margin: 1rem;
    }
  `;

  static properties = {
    data: { type: Array },
  };

  constructor() {
    super();
    this.data = [];
  }

  connectedCallback() {
    super.connectedCallback();

    const observer = {
      next: (data) => this.updateData(data),
      error: (error) => console.error('RxjsComponent.error()', error),
      complete: () => console.log('RxjsComponent.complete()'),
    };
    controllerLayer.controllerData$.subscribe(observer);

    controllerLayer.addData('Some data from Rxjs2Component');
  }

  updateData(data) {
    this.data = [...this.data, data];
  }

  render() {
    console.log('RxjsComponent.render()', this.data);
    return html`
      <h2>RxJS Component</h2>
      <p>Data received:
        <ul>
          ${this.data.map((item) => html`<li>${item}</li>`)}
        </ul> 
      </p>
    `;
  }
}

customElements.define('rxjs2-component', Rxjs2Component);
```

And add it to the `rxjs-page.js` file:

```javascript
import '../components/rxjs-component.js';
import '../components/rxjs2-component.js';
```

and the html file:

```html
...
    <main>
      <h1>Lit WebComponents Demo</h1>

      <rxjs-component></rxjs-component>
      <rxjs2-component></rxjs2-component>
    </main>
...
```

The output we would expect is that since both view components add data to the `controllerData$` Observable, both components should receive the data and show the data as a list on the page. But this is not the case. Each component receives the data itself added to the Observable, but not the data added by the other component. This is because the Observable is a unicast Observable. This means that their is a 1:1 relationship between the Observable and the subscriber. If we want to have a multicast Observable, where multiple subscribers can listen to the same Observable, we need to use another kind of Observables from the RxJS library.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 6](./assignment6.md) | :arrow_up: [Assignments](./README.md) | [Assignment 7.2](./assignment7.2.md) :arrow_forward:
