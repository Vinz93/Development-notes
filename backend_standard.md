# Express.js & Mongo API Standard
## JS Style Guide

First of all you must write your code with __ES6__ notation at least. you must have present that __ES7__ bring us new cool stuff like __Object spread operator__ and __Async + Await__.

In order to produce a good Javascript code, i highly recommended to follow the [Airbnb Style Guide](https://github.com/airbnb/javascript), and install __eslint__ in your editor with the `eslint-config-airbnb` plugin.

you can compile your code directly with babel in your `package.json` something like this:

```js
"scripts": {
  "dev": "nodemon --exec babel-node src/index.js",
  "build": "rm -rf dist && babel src --out-dir dist",
  "lint": "eslint src"
}

```

- Document your API with  Swagger jsdoc.
- Every time field should be expressed in hours.
- Avoid the callback hell, use Promises, Generators, (Async + Await) instead.


## Project Structure
This is an option to organize the structure of your API code,
the files and directories names must be in `snake_case`
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
      deployment/
      env/
        production.js
        development.js
        index.js
    tests/
      models/
      routes/
    index.js
```

## Libraries

These libraries will help you to make good API:

- Handle Promises with [Bluebird.js](http://bluebirdjs.com/docs/api-reference.html)

-  Tests with [Mocha](https://mochajs.org/), [Chai](http://chaijs.com/) and [Istanbul](https://www.npmjs.com/package/istanbul)

- Check the Request's with [Params validations](https://www.npmjs.com/package/express-validation) & [Joi](https://github.com/hapijs/joi)

- Parse, validate, manipulate, and display dates with  [momentjs](https://momentjs.com/)

## Examples

- [Example 1](https://github.com/Vinz93/node-redux/tree/master/server)
