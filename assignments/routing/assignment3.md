# Assignment 3: Setup of the backend

In our user stories we defined that we need a backend to store the repair assignments.
Since this subject is a frontend course, we will mock the backend using a simple tool called `json-server`.

Install `json-server` using npm:

```bash
npm install json-server
```

Create in the project root a directory `db` and within it a file called `db.json`.
For our project this database will hold a list of all repairs. That's why we will give it the following content:

```json
{
  "repairs": []
}
```

To run the `json-server` we add a script to our package.json:

````json
{
  "scripts": {
    "backend": "json-server db/db.json --port 3001"
  }
}
```

To start the backend server open a new terminal and run:

```bash
npm run backend
```

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 2](./assignment2.md) | :arrow_up: [Assignments](./README.md) | [Assignment 4](./assignment4.md) :arrow_forward:
````
