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
