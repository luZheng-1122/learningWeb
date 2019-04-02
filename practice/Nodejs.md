# Nodejs

[what is nodejs](https://medium.freecodecamp.org/what-exactly-is-node-js-ae36e97449f5)
Node.js is a JavaScript runtime environment. It offers JavaScript engine, mainly V8 (same of Chrome).

Check ES implementation of different node version: https://node.green/

## Dev tools

### 1. use babel to convert node.js to es6
https://hackernoon.com/using-babel-7-with-node-7e401bc28b04
#### install:
```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/node
```
#### creating a .babelrc file in our project root
```.babelrc
// .babelrc
{
  "presets": ["@babel/preset-env"]
}
```
#### Adding scripts to package.json
```package.json
"scripts": {
    "start": "babel-node src/server.js",
    "build": "babel src --out-dir dist",
    "serve": "node dist/server.js"
  }
```


### 2. nodemon
[nodemon](https://github.com/remy/nodemon) Monitor for any changes in your node.js application and automatically restart the server.
#### install:
```bash
npm install --save-dev nodemon
```
#### config:
```package.json
"scripts": {
    "start": "nodemon --exec babel-node src/server.js"
  }
```
