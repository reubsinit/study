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

#### Array Destructuring

```js
let records = [
  {
    name: "Reuben",
    last: "Wilson",
    email: "reubenmwilson@gmail.com",
  },
  {
    name: "Samuel",
  },
  {
    name: "John",
    email: "jk@gmail.com",
  },
  {
    name: "Boyd",
    email: "br@gmail.com",
  },
];

let [
  { name: firstName, email: firstEmail = "test@test.com" },
  { name: secondName, email: secondEmail = "test@test.com" },
] = records;

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
[first, second = 10, third, ...fourth] = temp = data();
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
  first, // skip the second using comma separation
  ,
  third,
] = data();
```

```js
// destructuring an array parameter
function data([first, , third, fourth]) {
  console.log(first, third, fourth);
}

data([1, 2, 3, 4, 5]); // outputs 1, 3, 4
```

```js
// nested array destructuring
let data = [1, 2, [3, 4]];
let [one, two, [three, four]] = data;

console.log(one, two, three, four); // outputs 1, 2, 3, 4
```

#### Object Destructuring

```js
function data() {
  return { a: 1, b: 2, c: 3, d: 4, e: 5 };
}

// without object destructuring
let temp = data();
let first = data.a != undefined ? tmp.a : 42;
let second = data.b;
let third = data.c;

// with object destructuring
let {
  a: first,
  b: second = 42, // default value of 42
  c: third,
  ...fourth // fourth will be an object with key/value pairs for properties d and e
} = data();
```

- If you want to declare your variables separately, then you need to wrap the entire destructuring assignment in parenthesis. Otherwise, JavaScript thinks you're declaring a block.

```js
let first, second, third;
({ a: first, b: second, c: third } = data());
```

- If you want a reference to the value being destructured

```js
let first, second, third, temp;
temp = { a: first, b: second, c: third } = data();
```

- If the source and the target have the same name

```js
let { a, b, c } = data();
```

- Nested object destructuring

```js
function data() {
  return { a: 1, b: { c: 3, d: 4, e: 5 } };
}

let {
  a,
  b: { c, d },
} = data();

console.log(a, c, d); // outputs 1 3 4
```

- Defaults!

```js
function data() {
  return { a: 1, b: { c: 3, d: 4, e: 5 } };
}

let {
  a,
  b: { c, d } = {}, // if b isn't a thing, default it to an empty object
} = data();

console.log(a, c, d); // outputs 1 3 4
```

- Object parameter destructuring

```js
function data({ a, b } = {}) {
  console.log(a, b);
}

data({ a: 101, b: 102 }); // outputs 101, 102
```

- Using the same source property to assign multiple targets

```js
let obj = {
  a: 1,
  b: {
    c: 2,
    d: 3,
  },
  e: 4,
};

let {
  a,
  b, // the object at key b on obj
  b: {
    c, // the value of property c on nested b on object
  },
  e,
} = obj;
```

#### Named Arguments

- You can use object destructuring to enforce the idea of named arguments

```js
  function lookupRecord(store = 'person-records', id = -1) {
    ...
  }; // without named arguments

  function lookupRecord({
    store = 'person-records',
    id = -1
  }) {
    ...
  }

  lookupRecord({id: 42}) // with name arguments
```

#### Destructuring Exercise

```js
// *******************************************************

function handleResponse({
  topic = "JavaScript",
  format = "Live",
  slides: { start = 0, end = 100 },
}) {
  TestCase({
    topic,
    format,
    slides: {
      start,
      end,
    },
  });
}

function TestCase(data) {
  console.log(data);
  console.log(
    data.topic == "JS Recent Parts" &&
      data.format == "Live" &&
      data.slides.start === 0 &&
      data.slides.end == 77
  );
}

// *******************************************************

function fakeAjax(url, cb) {
  // fake ajax response:
  cb({
    topic: "JS Recent Parts",
    slides: {
      end: 77,
    },
  });
}

fakeAjax("http://get-the-workshop.tld", handleResponse);
```

#### Async Await
```js
function getFile(file) {
	return new Promise(function(resolve){
		fakeAjax(file,resolve);
	});
}

async function loadFiles(files) {
  // make all requests concurrently
  let promises = files.map(getFile);
  for (let promise of promises) {
    console.log(await promise)
  }
}

loadFiles(["file1","file2","file3"]);


// **************************************


function fakeAjax(url,cb) {
	var fake_responses = {
		"file1": "The first text",
		"file2": "The middle text",
		"file3": "The last text"
	};
	var randomDelay = (Math.round(Math.random() * 1E4) % 8000) + 1000;

	console.log("Requesting: " + url);

	setTimeout(function(){
		cb(fake_responses[url]);
	},randomDelay);
}
```
