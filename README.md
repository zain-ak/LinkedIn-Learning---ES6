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

### Asynchronicity

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

Promise composition is a great way to handle situations where asynchronous operations depend on each other and order matters. Sometimes, though, order does not matter but only that all operations complete, in fact it's ideal that they run in parallel. This running in parallel is called _concurrency_, and in JS it's achieved with the function `Promise.all()`.

`Promise.all()` accepts and *array* of Promises and returns a single promise. The single promise returned will either:
- If all promises in the array resolve, the function will return a resolve value or;
- If a single promise from the array fails, the function will return a reject value (Also known as _failing fast_).

```javascript
let myPromises = Promise.all([returnsPromOne(), returnsPromTwo(), returnsPromThree()]);

myPromises
  .then((arrayOfValues) => {
    console.log(arrayOfValues);
  })
  .catch((rejectionReason) => {
    console.log(rejectionReason);
  });
```

#### Async Await

JavaScript is _non-blocking_: instead of stopping the execution of code while it waits, JavaScript uses an *event-loop* which allows it to efficiently execute other tasks while it awaits the completion of these asynchronous actions.

Originally, JavaScript used callback functions to handle asynchronous actions. The problem with callbacks is that they encourage complexly nested code which quickly becomes difficult to read, debug, and scale.

ES6 introduced promises which allows for increased readability. ES8 introduced `async...await` which further improves readability by reading more like traditional, synchronous code. 

`async...await` introduces a new syntax for using promises and generators, it doesn't introduce any new functionality in JS that wasn't there before. 

##### async operator
The `async` keyword is used to write functions that'll handle asycnhronous operations. 

```javascript
//async declaration
async function myFunc() {
  // Function body here
};

myFunc();

//async expression
const myFunc = async () => {
  // Function body here
};

myFunc();
```

`async` functions always return a promise. This means that all the syntax for promises like `.then()` and `.catch()` applies. There are three possible return values:
- If there’s nothing returned from the function, it will return a promise with a resolved value of undefined;
- If there’s a non-promise value returned from the function, it will return a promise resolved to that value;
- If a promise is returned from the function, it will simply return that promise.

##### await operator
The `await` keyword can only be used _inside_ an async function. await is an *operator*: it returns the resolved value of a promise. Since promises resolve in an indeterminate amount of time, await *halts* the execution of our async function until a given promise is resolved.

Promises are usually returned from functions in libraries. We can await the resolution of a promise insde an async function. 

```javascript
async function asyncFuncExample(){
  let resolvedValue = await myPromise();
  console.log(resolvedValue);
}

asyncFuncExample(); // Prints: I am resolved now!
```

##### Dependent Promises
Similar to chaining promises, async-await becomes really useful when multiple operations that are dependent on each other are implemented with it. Async-await allows for less cluttered code that is easier to maintain. The bigger point is that the code looks similar to traditional synchronous code which allows for the former. 

Variables are also more easily assigned with async-await. Here's an example and comparison:
```javascript
//chained promises
function nativePromiseVersion() {
    returnsFirstPromise()
    .then((firstValue) => {
        console.log(firstValue);
        return returnsSecondPromise(firstValue);
    })
   .then((secondValue) => {
        console.log(secondValue);
    });
}

//async-await dependent
sync function asyncAwaitVersion() {
 let firstValue = await returnsFirstPromise();
 console.log(firstValue);
 let secondValue = await returnsSecondPromise(firstValue);
 console.log(secondValue);
}
```

##### Handling Errors
When `.catch()` is used with a long promise chain, there is no indication of where in the chain the error was thrown.

With `async...await`, we use `try...catch` statements for error handling. By using this syntax, not only are we able to handle errors in the same way we do with synchronous code, but we can also catch both synchronous and asynchronous errors.

Here's a simple example below:
```javascript
const hostDinnerParty = async () => {
  try {
    let result = await cookBeanSouffle();
    console.log(`${result} is served!`);
  }
  catch (error) {
    console.log(error);
    console.log(`Ordering a pizza!`);
  }
}
```

###### Independent Promises
```javascript
async function waiting() {
 const firstValue = await firstAsyncThing();
 const secondValue = await secondAsyncThing();
 console.log(firstValue, secondValue);
}

async function concurrent() {
 const firstPromise = firstAsyncThing();
 const secondPromise = secondAsyncThing();
console.log(await firstPromise, await secondPromise);
}
```
While `async...await` allows for synchronous style coding, it can slow down an async function if the promises within are independent of each other. This is evident in the `waiting()` function in the above code; `firstValue` will wait to be assigned its value and only then will the function move on to `secondValue`. 

However, in the function `concurrent()`, both promises within the function are assigned values asynchronously, meaning they are probably running simultaneously. The `concurrent()` function itself comes and pauses at the `console.log()` line, where it `awaits` the variables `firstPromise` and `secondPromise` to have values.

```javascript
//Another example with template literal
const serveDinner = async () => {
  let vegetablePromise = steamBroccoli();
  let starchPromise = cookRice();
  let proteinPromise = bakeChicken();
  let sidePromise = cookBeans();
  
  console.log(`Dinner is served. We're having ${await vegetablePromise}, ${await starchPromise}, ${await proteinPromise}, and ${await sidePromise}.`);
}

serveDinner();
```

*NOTE:* If we have multiple truly independent promises that we would like to execute fully in parallel, we must use individual .then() functions and avoid halting our execution with await.

##### Await Promise.all()
Another way to take advantage of concurrency when we have multiple promises which can be executed simultaneously is to `await` a `Promise.all()`.

`Promise.all()` allows us to take advantage of asynchronicity— each of the asynchronous task can process concurrently. There is also the benefit of _failing fast_, meaning it won’t wait for the rest of the asynchronous actions to complete once any one has *rejected*.
