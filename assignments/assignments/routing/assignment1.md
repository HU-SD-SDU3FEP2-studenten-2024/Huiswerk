# Assignment 1: Boilerplate Setup

This assignment is about setting up the boilerplate for the project as you have done before in the last section about lit.

Make sure that you select lit as the template engine when you create the project using vite. Cleanup the generated project, add ESLint and prettier to the repository and setup a vite.config.js as well as a deploy script.

Please use the following content as the content for your `eslint.config.js` file:

```javascript
import globals from 'globals';
import pluginJs from '@eslint/js';
import prettier from 'eslint-config-prettier';

export default [
  pluginJs.configs.all,
  {
    languageOptions: {
      ecmaVersion: 'latest',
      globals: globals.browser,
      sourceType: 'module',
    },
    plugins: {
      prettier,
    },
    rules: {
      "no-console": "warn",
      "sort-keys": "off",
      "sort-imports": "off",
      "one-var": "off",
      "no-ternary": "off",
    },
  },
  pluginJs.configs.recommended,
];
```

---

:house: [Home](../../README.md) | :arrow_up: [Assignments](./README.md) | [Assignment 2](./assignment2.md) :arrow_forward:
