# File/Folder structure

The images we will use in this assignment can be found in the `/public/images/` folder of this repository.

The importent folders in this assignments are:

`/index.html` the only html file we will have in our project. Note that this file imports a single JS script `/src/pages/home-page.js` as well as a single stylesheet file `/styles.css`. For each assignment you will append html code to the body part of the file.

`/view` this folder represents the `view` layer of our architecture. It holds two subfolders `/view/pages` and `/view/components`. The `/view/pages` folder holds the JS files that represent the different pages of our application. The `/view/components` folder holds the JS files that represent the different components of our application.

`/view/pages/home-page.js` This is the file that's been used by the `index.html`. In this assignment this will be the only file within our `/view/pages` directory, because we will only have one page in our application. In each assignments we add imports of components we define in the `/view/components` folder. Doing so the imported components will become available within the `index.html`.

`/view/components` this is the folder where we will specify the web components.

# Assignments

Unlike the previous assignments in the previous sections, these assignments have to be done in a specific order. This is because each assignment builds on the previous one. All assignments are related to the same project, the `bike-repair-shop` project.

The assignments are as follows:

* [Assignment 1: Boilerplate Setup](./assignment1.md)
* [Assignment 2: SCRUM and Github](./assignment2.md)
* [Assignment 3: Setup of the backend](./assignment3.md)
* [Assignment 4: Architectural Layers](./assignment4.md)
* [Assignment 5: Multiple Pages Application (MPA)](./assignment5.md)
* [Assignment 6: Single Page Application (SPA)](./assignment6.md)
* [Assignment 7: Advanced Routing](./assignment7.md)

---

:house: [Home](../../README.md)