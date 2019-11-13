# ES6 Course

### Transpiling
The process of converting ES6 into ES5 so that it can be read by browsers. Common tools for doing transpiling:
  -  <a href="babeljs.io" target="_blank">Babel.js</a>
  -  <a href="babeljs.io" target="_blank">Traceur.js</a>

### Webpack
Webpack is a modular build tool for loaders and plugins; _Loaders_ transform the source code of a module e.g Babel. _Plugins_ usually have more complete functionality than loaders, and form the core of Webpack e.g is minifying files.

### var, let, const
`var` is *function-scoped*, `let` and `const` are *block-scoped*. Function scope means that the variable can be accessed from anywhere regardless of the scope it's been created in, this is due to hoisting.

With block-scope, variable declarations are akin to traditional programming languages and are usually limited to the scope in which they exist. The major difference between `let` and `const` are that the value of `const` cannot be changed once it has been assigned one. However, this is not the case for properties within a `const` variable:
  ```javascript
    const number = {first: 2, second: 1};
    number.first = 1; //this is allowed

    const letter = 'a';
    letter = 'b'; //this is not allowed
  ```

### Template Literals
``${}``, Need I more say? Use them, they're bomb.

### Spread Operator
The spread operator `...` is used to expand an _iterable_  (array or a string expression) in places where zero or more arguments or elements are required. A simple e.g:

```javascript
  const cars {'bmw', 'lexus'};
  const houses {'mansion', 'apartment'};

  let housesAndCars = {cars, houses}; //is going to be an array with two arrays within
  housesAndCars = {...cars, ...houses};  //is going to be a single array of size 4 with individual elements of cars and houses.
```

### Default function parameters

When defining a function, e.g `function calcFun(funWeight, time)`, if the function is called in a console log it will return a value of NaN. In ES6, however, you can assign function parameters _default_ values that'll be used if no paramaters are sent through. Think of it like having a placeholder in a textbox that a user hasn't filled in yet.

```javascript
  function calcFun(funWeight=1.1, timeMin=120) {
    return `${funWeight*timeMin} fun has been had!`;
  }
```

