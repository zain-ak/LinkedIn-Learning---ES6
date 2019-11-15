# ES6 Course

### Transpiling
The process of converting ES6 into ES5 so that it can be read by browsers. Common tools for doing transpiling:
  -  <a href="http://babeljs.io" target="_blank">Babel.js</a>
  -  Traceur.js

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
The spread operator `...` is used to expand an _iterable_ (array or a string expression) in places where zero or more arguments or elements are required. A simple e.g:

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

### Destructuring Assignments
Destructuring allows you to break down an object or an array into their smaller elements and access those values directly. Below is a neat way to access an objects properties without explicity calling the object in the function vacationMarketing using the `({})` syntax.

```javascript
  var vacation = {
			destination: "Chile",
			travelers: 2,
			activity: "skiing", 
			cost: 4000
		};

  function vacationMarketing({destination, activity}) {
    return `Come to ${destination} and do some ${activity}`
  }

  console.log(vacationMarketing(vacation));
```

With arrays, you can assign specific variable names to indexes you know you'll use later and just call them like normal variables:

```javascript
  var [first,,,, last] = ['first', 'second', 'third', 'fourth', 'fifth']; //the commas will skip over the variables in between
```

### Asynchronity

Asynchronous JavaScript uses the _event-loop_. The delayed code will be added to a piece of code waiting to be run. The code waiting to be run will be executed first, then the delayed code will be executed. 

#### Promises

Promises are objects that represent the eventual outcome of an asynchronous operation. A Promise object can be in one of three states:

- *Pending*: The initial state — the operation has not completed yet.
- _Fulfilled_: The operation has completed successfully and the promise now has a resolved value. For example, a request’s promise might resolve with a JSON object as its value.
- _Rejected_: The operation has failed and the promise has a reason for the failure. This reason is usually an Error of some kind.

A promise has been _settled_ if it has been either resolved (fulfilled) or rejected.

##### Constructing Promises
```javascript
  const inventory = {
  sunglasses: 1900,
  pants: 1088,
  bags: 1344
};

// Write your code below:
const myExecutor = (resolve, reject) => {
  if (inventory.sunglasses > 0) {
    resolve(`Sunglasses order processed.`);
  }
  else
    reject(`That item is sold out.`);
};

/*A promise is created using a Promise constructor, which takes in a executor function
  as a parameter. The executor function contains the resolve and reject methods in it. 

  The executor, myExecutor, has been created above and is passed in the Promise 
  constructor below.
*/
const orderSunglasses = () => new Promise(myExecutor); 


const orderPromise = orderSunglasses(); //Create a promise

console.log(orderPromise); //Print the result of the promise

```

##### Consuming Promises

Knowing how to consume promises is key, mose of the time you'll be handling `Promise` objects as a result of asynchronous calls you've made. The initial state of all promises is `pending`, but we know it'll settle. What do we want to happen `then`? Well, you chain on the method `.then()`. 

`.then()` is a higher-order function; it takes two callback functions as arguments, called _handlers_:
  1. `onFulfilled` is the _sucesss_ handler and handles the logic when the promise resolves
  2. `onRejeccted` is the _failure_ handler and handles the logic when the promise is rejected.

`.then()` can be invoked with zero, one or both handlers. This allows for flexibility but can make debugging tricky because it'll ALWAYS return a promise.

```javascript
  //making two handlers then attaching them to a function that returns a promise
  const handleSuccess = (success) => {
    console.log(success);
  };

  const handleFailure = failure => console.log(failure);

  checkInventory(order).then(handleSuccess, handleFailure);
```

It's convention to chain on two `thens` one after the other to handle a success and a failure, for readability:

```javascript
  prom
  .then((resolvedValue) => {
    console.log(resolvedValue);
  })
  .then(null, (rejectionReason) => {
    console.log(rejectionReason);
  });
```

But this goes another step further by using `.catch()` to handle the failure:

```javascript
  prom
  .then((resolvedValue) => {
    console.log(resolvedValue);
  })
  .catch((rejectionReason) => {
    console.log(rejectionReason);
  });
```

##### Chaining Promises

It's common to chain the results of promises one after the other since they use them. The process of chaining promises together is called _composition_. 

```javascript
  firstPromiseFunction()
  .then((firstResolveVal) => {
    return secondPromiseFunction(firstResolveVal);
  })
  .then((secondResolveVal) => {
    console.log(secondResolveVal);
  });
```

*NOTE* Be careful, make sure you chain promises and don't nest them. Make sure to `return` promises in chains.

##### Promises.all()
