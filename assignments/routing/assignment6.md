# Assignment 6: Single Page Application (SPA)

In this assignment we will transform the application from assignment 5 into a Single Page Application (SPA). This means that the application will only load once from a single html page and the content will be updated dynamically without reloading the page.

## Page Components

We start this transition by changing the javascript files in the directory `/src/view/pages` into webcomponents that hold the content of the `html` of the pages to whom they belong. The `html` files in the directory `/src/view/pages` will be empty and only contain the webcomponent tag.

So `index.html` will look like this:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <base href="%BASE_URL%" />

    <link rel="icon" type="image/svg+xml" href="/lit.svg" />

    <title>Bike Repair Shop</title>
    <script type="module" src="/src/view/pages/home-page.js"></script>
  </head>
  <body>
    <main>
      <home-page></home-page>
    </main>
  </body>
</html>
```

The `/src/view/pages/home-page.js` file will look like this:

```javascript
import { LitElement, html, css } from 'lit-element';
import '../components/repairassignment-list';

export class HomePage extends LitElement {
  static styles = css`
    h1 {
      text-align: center;
    }
  `;

  render() {
    return html`
      <h1>Repair Assignments</h1>
      <repairassignment-list></repairassignment-list>
      <a href="./intake-page.html">Add a repair assignment</a>
    `;
  }
}

customElements.define('home-page', HomePage);
```

And for the intake page the content of `intake-page.html` will be:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <base href="%BASE_URL%" />

    <link rel="icon" type="image/svg+xml" href="/lit.svg" />

    <title>Bike Repair Shop</title>
    <script type="module" src="/src/view/pages/intake-page.js"></script>
  </head>
  <body>
    <main>
      <intake-page></intake-page>
    </main>
  </body>
</html>
```

And the `/src/view/pages/intake-page.js` file will look like this:

```javascript
import { LitElement, html, css } from 'lit-element';
import '../components/intake-form.js';

export class IntakePage extends LitElement {
  static styles = css`
    h1 {
      text-align: center;
    }
  `;

  render() {
    return html`
      <h1>Bike Repair - Intake</h1>
      <intake-form></intake-form>
    `;
  }
}

customElements.define('intake-page', IntakePage);
```

So if we compare the two html files we will see that only the `script` tag and the webcomponent used differentiates the two files. So when we could import the same `js` file in both `html` files and could based on the url dynamically assign which component to show we would have a single page application. The dynamically assigning of the component to show is done by the `router` that we will implement in this assignment.

## Router

Routing is a concept that is used in Single Page Applications to determine which content to show based on the url.
We could write our own router, but there are already libraries that do this for us.
Frameworks like Angular, React and Vue have their own router libraries. But we are using LitElement, so we will use a library that is specifically made for webcomponents, but the concept is the same.
We will use the `vaadin-router` library. This library is a router for webcomponents and is based on the `lit-element` library itself.

> [!NOTE]
> Unfortunatly work on the `vaadin-router` library seems to have stopped. But the library still works fine. The Lit project team also is working on a router, but it is not yet released. So for now we will use the `vaadin-router` library.

To install the `vaadin-router` library as a dependency (we need the library not only in dev mode, but also in production) we use the following command:

```bash
npm install @vaadin/router
```

Now lets create a `router.js` file in the `/src/view` directory with the following content:

```javascript
import { Router } from '@vaadin/router';

import './pages/home-page';
import './pages/intake-page';

const outlet = document.querySelector('main');
const router = new Router(outlet);

router.setRoutes([
  { path: `/`, component: 'home-page' },
  { path: `/intake`, component: 'intake-page' },
]);
```

What this file does is it imports the `Router` class from the `@vaadin/router` library and the webcomponents that we created in the previous step. It then selects the `main` element from the html file that loaded this script and creates a new `Router` object with this element as the `outlet`. The `router` object is then configured with the routes that we want to use. The `path` property is the url that we want to use and the `component` property is the webcomponent that we want to show when the url is matched.

So we now can change the `index.html` file to:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <base href="%BASE_URL%" />

    <link rel="icon" type="image/svg+xml" href="/lit.svg" />

    <title>Bike Repair Shop</title>
    <script type="module" src="/src/view/router.js"></script>
  </head>
  <body>
    <main></main>
  </body>
</html>
```

And we can remove the `intake-page.html` file. As well as the configuration of the `intake-page.js` file in the `vite.config.js` file.

```javascript
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// got the above code from https://flaviocopes.com/fix-dirname-not-defined-es-module-scope/
// this will resolve eslint errors for __dirname

import { resolve } from 'path';
import { defineConfig } from 'vite';
export default defineConfig({
  base: '/github-pages-subdir/',
  build: {
    sourcemap: true,
    rollupOptions: {
      input: {
        main: resolve(__dirname, 'index.html'),
      },
    },
  },
});
```

What remains is to change the links in the two pages we created.
So in the `render` method of the `home-page.js` file we change the link from `./intake-page.html` to `./intake` so that it matches the route we set in the `router.js`. So now the render method will look like:

```javascript
  render() {
    return html`
      <h1>Repair Assignments</h1>
      <repairassignment-list></repairassignment-list>
      <a href="./intake">Add a repair assignment</a>
    `;
  }
```

In our `intake-form` we also use a link, but that link is not within the html part, but within the javascript code of the `submitHandler` method.

```javascript
window.location.href = `${import.meta.env.BASE_URL}`;
```

This line of code will still work, but it will trigger a full reload of the page, which is inefficient and therefore might results in a poor user experience. In complexer applications that full reload might also cause the loss of state. And last but not least, it doesn't integrate well with the browser's history API.

Therefore we will replace this line of code with the functionallity of the Vaadin Router. We therefore first have to add the import od the `Router` class from the `@vaadin/router` library in the `intake-form.js` file.

```javascript
import { Router } from '@vaadin/router';
```

Next we exchange the `window.location.href` line with the following line of code:

```javascript
Router.go('/');
```

This should have worked, were we not using a subdirectory as a base for our application. Due to a bug in the `@vaadin/router` library we have to replace the `/` project root with the base root folder of our application, which we had specified in the `vite.config.js` file. We can do this by using the `import.meta.env.BASE_URL` variable. So we once again change the line of code to:

```javascript
Router.go(import.meta.env.BASE_URL);
```

We now have successfully transformed our application into a Single Page Application.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 5](./assignment5.md) | :arrow_up: [Assignments](./README.md) | [Assignment 7](./assignment7.md) :arrow_forward:
