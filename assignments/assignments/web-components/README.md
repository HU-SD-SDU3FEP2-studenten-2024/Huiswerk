# File/Folder structure

The images we will use in this assignment can be found in the `/public/images/` folder of this repository.

The importent folders in this assignments are:

`/index.html` the only html file we will have in our project. Note that this file imports a single JS script `/src/pages/home-page.js` as well as a single stylesheet file `/styles.css`. For each assignment you will append html code to the body part of the file.

`/view` this folder represents the `view` layer of our architecture. It holds two subfolders `/view/pages` and `/view/components`. The `/view/pages` folder holds the JS files that represent the different pages of our application. The `/view/components` folder holds the JS files that represent the different components of our application.

`/view/pages/home-page.js` This is the file that's been used by the `index.html`. In this assignment this will be the only file within our `/view/pages` directory, because we will only have one page in our application. In each assignments we add imports of components we define in the `/view/components` folder. Doing so the imported components will become available within the `index.html`.

`/view/components` this is the folder where we will specify the web components.

# Assignments

**Custom Elements**
* [Assignment 1: Hello World](./assignment1.md)
* [Assignment 2: Hello Attribute](./assignment2.md)

**Shadow DOM**
* [Assignment 3: Hello Shadowdom](./assignment3.md)
* [Assignment 4: Hello Shadowdom CSS](./assignment4.md)
* [Assignment 5: No Shadowdom CustomEvent Counter](./assignment5.md)
* [Assignment 6: Shadowdom CustomEvent Counter](./assignment6.md)

**HTML Templates**
* [Assignment 7: HTML Templates](./assignment7.md)
* [Assignment 8: HTML Templates and Slots](./assignment8.md)

**Nesting Components**
* [Assignment 9: Hello Nested Components](./assignment9.md)

**Lifecycle Callbacks**
* [Assignment 10: Lifecycle Callbacks](./assignment10.md)

---

:house: [Home](../../README.md)