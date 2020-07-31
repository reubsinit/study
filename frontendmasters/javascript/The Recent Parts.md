#### Tagged Templates
```js
const obj = {a: 'a', b: 'b', c: 'c'}l
function logger(strings, ...values) {
  let str = "";
  for (let i = 0; i < strings.length; i++) {
    if (i > 0) {
      let expression = values[i - 1];
      if (expression && typeof expression == 'object') {
        if (expression instanceof Error) {
          if (expression.stack) {
            str += expression.stack;
            continue;
          }
        } else {
          try {
            str += JSON.stringify(expression);
            continue;
          } catch (err) {}
        }
      }
      str += expression;
    }
    str += strings[i];
  }
  console.log(str);
  return str;
}

logger `${obj}`

function upper(strings, ...values) {
  let str = '';
  for (let i = 0; i < strings.length; i++) {
    if (i > 0) {
      str += values[i - 1].toUpperCase();
    };
    str += strings[i];
  }
  return str;
}

let name = 'reuben'
let teacher = 'the grand master'

upper `Hello ${name}, my name is ${teacher} hahahah!`
```

#### Destructuring
```js
let records = [
  {
    name: 'Reuben',
    last: 'Wilson',
    email: 'reubenmwilson@gmail.com'
  },
  {
    name: 'Samuel'
  },
  {
    name: 'John',
    email: 'jk@gmail.com'
  },
  {
    name: 'Boyd',
    email: 'br@gmail.com'
  }
]

let [
  {
    name: firstName,
    email: firstEmail = 'test@test.com'
  },
  {
    name: secondName,
    email: secondEmail = 'test@test.com'
  }
] = records

console.log(firstName, firstEmail); // outputs Reuben reubenmwilson@gmail.com
console.log(secondName, secondEmail); // outputs Samuel test@test.com
```

In the destructuring example above, on the left hand side of the assignment operator, we've got a pattern declared for how JavaScript should handle the destructuring assignment. The pattern specifies that we want four variables in total - `firstName`, `firstEmail`, `secondName` & `secondEmail`. Observe also that both the variables `firstEmail` & `secondEmail` have the default value assigned to them of `test@test.com`. This means that if the object we're trying to destructure doesn't have an `email` property, these variables will have a fallback default. Essentially what's being described in the pattern is that we want the values of the properties `name` and `email` of the first two objects in the array.

Here's some more examples:
```js
function data() {
  return [1, undefined, 3, 4, 5];
}

// Without array destructuring
let temp = data();

let first = temp[0];
let second = temp[1] !== undefined ? tmp[1] : 10; // default assignment
let third = temp[2];
let fourth = data().slice(3); // everything after the third element gets assigned to fourth as an array

// Using array destructuring
[
  first,
  second = 10, // default assignment
  third,
  ...fourth // everything after the third element gets assigned to fourth as an array using the rest (...) operator
] = data();
```

In the above example, if you wanted to keep a reference to `temp` in the destructuring example, you could
```js
[
  first,
  second = 10,
  third,
  ...fourth
] = temp = data();
```

```js
function data() {
  return [1, 2, 3, 4, 5];
}

// Without array destructuring
let temp = data();

let first = temp[0];
// let second = temp[1]; skip the second
let third = temp[2];

// Using array destructuring
[
  first,
  , // skip the second using comma separation 
  third,
] = data();
```

```js
// destructuring an array parameter
function data([
  first,
  ,
  third,
  fourth
]) {
  console.log(first, third, fourth)
};

data([1,2,3,4,5]); // outputs 1, 3, 4
```

```js
// nested array destructuring
let data = [1, 2, [3, 4]];
let [one, two, [three, four]] = data;

console.log(one, two, three, four); // outputs 1, 2, 3, 4
```