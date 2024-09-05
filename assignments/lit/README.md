# File/Folder structure

The images we will use in this assignment can be found in the `/public/images/` folder of this repository.

The importent folders in this assignments are:

`/index.html` the only html file we will have in our project. Note that this file imports a single JS script `/src/pages/home-page.js` as well as a single stylesheet file `/styles.css`. For each assignment you will append html code to the body part of the file.

`/view` this folder represents the `view` layer of our architecture. It holds two subfolders `/view/pages` and `/view/components`. The `/view/pages` folder holds the JS files that represent the different pages of our application. The `/view/components` folder holds the JS files that represent the different components of our application.

`/view/pages/home-page.js` This is the file that's been used by the `index.html`. In this assignment this will be the only file within our `/view/pages` directory, because we will only have one page in our application. In each assignments we add imports of components we define in the `/view/components` folder. Doing so the imported components will become available within the `index.html`.

`/view/components` this is the folder where we will specify the web components.

# Assignments

* [Assignment 1: Hello Lit](./assignment1.md)
* [Assignment 2: Lit Attributes](./assignment2.md)
* [Assignment 3: Reactive Properties](./assignment3.md)
* [Assignment 4: CustomEvent Counter](./assignment4.md)
* [Assignment 5: Rendering Lit Components](./assignment5.md)

---

:house: [Home](../../README.md)