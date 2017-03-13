# Express.js & Mongo API Standard
## JS Style Guide

First of all you must write your code with __ES6__ notation at least. you must have present that __ES7__ bring us new cool stuff like __Object spread operator__ and __Async + Await__.

In order to produce a good Javascript code, i highly recommended to follow the [Airbnb Style Guide](https://github.com/airbnb/javascript), and install __eslint__ in your editor with the `eslint-config-airbnb` plugin.

you can compile your code directly with babel in your `package.json` something like this:

```js
"scripts": {
  "dev": "nodemon --exec babel-node src/index.js",
  "build": "rm -rf dist && babel src --out-dir dist",
}

```

- Document your API with  Swagger jsdoc.
- Every time field should be expressed in hours.
- Avoid the callback hell, use Promises, Generators, (Async + Await) instead.


## Project Structure

The file and directory names are in `snake_case`
```
server/
  README.md
  node_modules/
  package.json
  .eslintrc
  .babelrc
  src/
    controllers/
      some_controller.js
    models/
    routes/
    helpers/
      error_handler.js
    services/
      param_validators/
        some_controller.js
    config/
      env/
        production.js
        development.js
        index.js
    index.js
```

## Libraries

These libraries will help you to make good API:

- Handle Promises with [Bluebird.js](http://bluebirdjs.com/docs/api-reference.html)

-  Tests with [Mocha](https://mochajs.org/) & [Chai](http://chaijs.com/)

- Check the Request's with [Params validations](https://www.npmjs.com/package/express-validation) & [Joi](https://github.com/hapijs/joi)

- Parse, validate, manipulate, and display dates with  [momentjs](https://momentjs.com/)
