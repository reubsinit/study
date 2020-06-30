## General Notes

- Content is pretty basic and I would say that it's definitely at a beginner level
- Presenter doesn't appear to be completely confident in her ability

#### Success Tips for Learning

- Figure out how to measure your learning

#### Property Access

- Can you get a reference to an object property? For properties that are references to objects, yes. Though, when would you want to do this rather than just accessing the desired property.

#### ES6 Destructuring

- ```js
  let [a, b] = [true, false];
  // a === true
  // b === true

  let [c, d, e] = [1, 2];
  // c === 1
  // d === 2
  // e === undefined

  let { firstName, lastName } = { firstName: "Reuben", lastName: "lastName" };
  // firstName === 'Reuben'
  // lastName === 'Wilson'

  const me = { firstName: "Reuben", lastName: "lastName" };
  let { firstName, lastName } = me;
  // firstName === 'Reuben'
  // lastName === 'Wilson'

  let a = 1,
    b = 2;
  [a, b] = [b, a];
  // a === 2 && b === 1

  let [a, [b, [c, d]]] = [1, [2, [[[3, 4], 5], 6]]];
  // a === 1 && b === 2 && c === [[3, 4], 5] && d === 6

  let people = [
    { firstName: "Reuben", lastName: "Wilson" },
    { firstName: "Samuel", lastName: "Wilson" },
  ];
  let [{ firstName: firstPerson }, { firstName: secondPerson }] = people;
  // firstPerson === 'Reuben' && secondPerson === 'Samuel'
  ```

- `const` on an object assignment will prevent the assigned variable from being overwritten. It does not stop you from changing the values of or adding properties to a `const` declared object.
  ```js
  const obj = { a: "a", b: "b", c: "c" };
  obj = {}; // Uncaught TypeError: Assignment to constant variable.
  obj.d = "d"; // { a: 'a', b: 'b', c: 'c', d: 'd' };
  obj.a = "not a"; // // { a: 'not a', b: 'b', c: 'c', d: 'd' };
  ```

#### _.map( ) Exercise

- You can go to a library website and use their functions within the browser console. For instance, head to [lodash.com](lodash.com), open up the developer console and try it out:
  ```js
  _.forEach([1, 2, 3], (val) => console.log(val));
  ```
#### Spread Operator
- ```js
  const fn = function(a, b, ...c) { 
    return [a, b, c]
  };
  fn(1, 2, 3, 4, 5); // returns [1, 2, [3, 4, 5]]
  ```
#### Arguments
- `aruguments` is an array of the values that are passed into a function as paremeters
  ```js
    const fn = function(a, b, ...c) {
      return arguments;
    };
    fn(1, 2, 3, 4, 5); // returns [1, 2, 3, 4, 5]
  ```
  This won't work using ES6 arrow functions
#### Default Parameters
- Available from ES6
  ```js
  const fn = function (a, b = 2) {
    console.log(a, b);
  };
  fn(1); // outputs 1, 2
  fn(1, 3); // outputs 1, 3
  ```
  To achieve the same thing in ES5 or lower:
  ```js
  const fn = function (a, b) {
    b = b || 2;
    console.log(a, b);
  };
  ```
  #### Scope Walkthrough, Parts 1 - 3
- ```js
  var a = 1;
  {
    a = 2;
  }
  console.log(a); // outputs 2
  ```
- With `let` (ES6), you can declare block scoped variables:
- ```js
  var a = 1;
  {
    let a = 2;
  }
  console.log(a); // outputs 1
  ```
- ```js
  const fn = () => {
    var a = 'a';
    console.log(a); // outputs a
  }
  fn();
  console.log(a === 'a'); // outputs false
  ```
- Create a variable `a` within an inner IIFE that masks the variable `a` in the parent scope
  ```js
  (function() {
    var a = 'a';
    (function() {
      var a = 'b';
      console.log(a); // outputs b
    })();
    console.log(a); // outputs a
  })();
  ```
- Inner function retains access to outer scope, even after the outer function has been invoked
  ```js
  (function() {
    const fn = () => {
      let a = 1;
      const inner = () => {
        a += 1;
        console.log(a);
      }
      window.innerFn = inner;
    }
    fn();
    window.innerFn(); // calls inner and outputs 2
  })();
  ```
#### Higher order functions - passing arguments
- ```js
  const numSquared = (n) => {
    return n * n;
  }

  const fn = (n, func) => {
    return func(n);
  }

  console.log(fn(5, numSquared)); // outputs 25
  ```
#### Closure Recipe
1. Create your parent function
1. Define any variables in the parent's local scope
1. Define a function inside the parent - referred to as the child
1. Return the child, or inner function, from the parent
- ```js
  const timer = () => {
    let elapsed = 0;
    let id;
    const increment = () elapsed++;
    const start = () => id = setInterval(increment, 1000);
    const ticks = () => { return elapsed; };
    const pause = () => clearInterval(id);
    return {start, stop, ticks};
  };
  ```