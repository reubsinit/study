#### Type Check Exercise

```js
if (!Object.is || true) {
  Object.is = function objectIs(a, b) {
    let aIsNeg0 = isNeg0(a);
    let bIsNeg0 = isNeg0(b);

    // You've got to check for either or here
    // because in JS -0 === 0 =\
    // which is what what happen in strict equality check
    // For instance, if the && operator was being used here
    // a === would return true for 0 === -0 =\
    if (aIsNeg0 || bIsNeg0) return aIsNeg0 && bIsNeg0;
    else if (isItNaN(a) && isItNaN(b)) return true;
    else if (a === b) return true;
    return false;

    function isNeg0(val) {
      return val === 0 && 1 / val === -Infinity;
    }

    function isItNaN(val) {
      // NaN is the only value in JavaScript that is not equal to itself
      return val !== val;
    }
  };
}

// tests:
console.log(Object.is(42, 42) === true);
console.log(Object.is("foo", "foo") === true);
console.log(Object.is(false, false) === true);
console.log(Object.is(null, null) === true);
console.log(Object.is(undefined, undefined) === true);
console.log(Object.is(NaN, NaN) === true);
console.log(Object.is(-0, -0) === true);
console.log(Object.is(0, 0) === true);

console.log(Object.is(-0, 0) === false);
console.log(Object.is(0, -0) === false);
console.log(Object.is(0, NaN) === false);
console.log(Object.is(NaN, 0) === false);
console.log(Object.is(42, "42") === false);
console.log(Object.is("42", 42) === false);
console.log(Object.is("foo", "bar") === false);
console.log(Object.is(false, true) === false);
console.log(Object.is(null, undefined) === false);
console.log(Object.is(undefined, null) === false);
```

#### Intentional Coersion

- Adapt a coding style that makes value types plaing and obvious.
- A quality JS program embraces coercions, making sure the types involved in every operation are clear - thus, corner cases are safely managed.

#### Coercion Exercises

1. Define an `isValidName(..)` validator that takes one parameter, `name`. The validator returns `true` if all the following match the parameter (`false` otherwise):

   - must be a string
   - must be non-empty
   - must contain non-whitespace of at least 3 characters

2. Define an `hoursAttended(..)` validator that takes two parameters, `attended` and `length`. The validator returns `true` if all the following match the two parameters (`false` otherwise):

   - either parameter may only be a string or number
   - both parameters should be treated as numbers
   - both numbers must be 0 or higher
   - both numbers must be whole numbers
   - `attended` must be less than or equal to `length`

```js
function isValidName(name) {
  return typeof name === "string" && name.trim().length >= 3;
}

// Mine
function hoursAttended(attended, length) {
  function isStringOrNum(val) {
    return ["string", "number"].includes(typeof val);
  }
  function isEmpty(val) {
    return typeof val === "string" && Number(val) === 0;
  }
  return (
    isStringOrNum(attended) &&
    isStringOrNum(length) &&
    !isEmpty(attended) &&
    !isEmpty(length) &&
    !isNaN(attended) &&
    !isNaN(length) &&
    Number(attended) >= 0 &&
    Number(length) >= 0 &&
    Number(attended) % 1 === 0 &&
    Number(length) % 1 === 0 &&
    Number(length) > Number(attended)
  );
}

// Instructors
function hoursAttended(attended, length) {
  if (typeof attended == "string" && attended.trim() != "") {
    attended = Number(attended);
  }
  if (typeof length == "string" && length.trim() != "") {
    length = Number(length);
  }
  if (
    typeof attended == "number" &&
    typeof length == "number" &&
    attended <= length &&
    attended >= 0 &&
    length >= 0 &&
    Number.isInteger(attended) &&
    Number.isInteger(length)
  ) {
    return true;
  }

  return false;
}

// tests:
console.log(isValidName("Frank") === true);
console.log(hoursAttended(6, 10) === true);
console.log(hoursAttended(6, "10") === true);
console.log(hoursAttended("6", 10) === true);
console.log(hoursAttended("6", "10") === true);
console.log(isValidName(false) === false);
console.log(isValidName(null) === false);
console.log(isValidName(undefined) === false);
console.log(isValidName("") === false);
console.log(isValidName("  \t\n") === false);
console.log(isValidName("X") === false);
console.log(hoursAttended("", 6) === false);
console.log(hoursAttended(6, "") === false);
console.log(hoursAttended("", "") === false);
console.log(hoursAttended("foo", 6) === false);
console.log(hoursAttended(6, "foo") === false);
console.log(hoursAttended("foo", "bar") === false);
console.log(hoursAttended(null, null) === false);
console.log(hoursAttended(null, undefined) === false);
console.log(hoursAttended(undefined, null) === false);
console.log(hoursAttended(undefined, undefined) === false);
console.log(hoursAttended(false, false) === false);
console.log(hoursAttended(false, true) === false);
console.log(hoursAttended(true, false) === false);
console.log(hoursAttended(true, true) === false);
console.log(hoursAttended(10, 6) === false);
console.log(hoursAttended(10, "6") === false);
console.log(hoursAttended("10", 6) === false);
console.log(hoursAttended("10", "6") === false);
console.log(hoursAttended(6, 10.1) === false);
console.log(hoursAttended(6.1, 10) === false);
console.log(hoursAttended(6, "10.1") === false);
console.log(hoursAttended("6.1", 10) === false);
console.log(hoursAttended("6.1", "10.1") === false);
```

#### Double Equals Corner Cases
```js
if ([] == ![]) ...
if ([] == false) ...
if ('' == false) ...
if (0 == false) ...
if (0 == 0) ...
```
- `[]` is a truthy value, so `![]` is `false`
- `[]` is then going to have the abstract `toPrimitive` invoked on it, which ends up converting the array to a string
- And empty string is then coerced into a number, which results in `0`
- `false` is coerced into a number, so you end up with `0 === 0`

#### The Case for Double Equals
- `==` is not about comparisons with unknown types
- Knowing types is always better than not knowing them
- You should prefer `==` in all possible places
- If you know the types in a comparison, if both types are the same, then `==` is identical to `===`
- Since `===` is pointless to use when the types don't match, it's similarly pointless when the types do match
- Not knowing the types means that you don't fully understand that code
- If you can't or won't use known and obvious types, then `===` is the only reasonable choice

#### Equality Exercise
1. The `findAll(..)` function takes a value and an array. It returns an array.

2. The coercive matching that is allowed:

	- exact matches (`Object.is(..)`)
	- strings (except "" or whitespace-only) can match numbers
	- numbers (except `NaN` and `+/- Infinity`) can match strings (hint: watch out for `-0`!)
	- `null` can match `undefined`, and vice versa
	- booleans can only match booleans
	- objects only match the exact same object
```js
function setsMatch(arr1,arr2) {
	if (Array.isArray(arr1) && Array.isArray(arr2) && arr1.length == arr2.length) {
		for (let v of arr1) {
			if (!arr2.includes(v)) return false;
		}
		return true;
	}
	return false;
}

// TODO: write `findAll(..)`
function findAll(value, values) {
	let result = [];
	values.forEach(entry => {
		if (Object.is(value, entry)) {
			result.push(entry);
		}
		else if (typeof value == 'string' 
			&& value.trim() != '' 
			&& typeof entry == 'number' 
			&& !Object.is(-0, entry) 
			&& value == entry) {
				result.push(entry);
			}
		else if (typeof value == 'number' 
			&& !Object.is(-0, value) 
			&& !Object.is(NaN, value) 
			&& !Object.is(Infinity, value) 
			&& !Object.is(-Infinity, value) 
			&& typeof entry == 'string' 
			&& entry.trim() != '' 
			&& value == entry) {
				result.push(entry);
			}
		else if (typeof value == 'boolean' 
			&& value === entry) {
				result.push(entry);
			}
		else if (value == null 
				&& entry == null) {
					result.push(entry);
				}
	});
	return result;
}



// tests:
var myObj = { a: 2 };

var values = [
	null, undefined, -0, 0, 13, 42, NaN, -Infinity, Infinity,
	"", "0", "42", "42hello", "true", "NaN", true, false, myObj
];

console.log(setsMatch(findAll(null,values),[null,undefined]) === true);
console.log(setsMatch(findAll(undefined,values),[null,undefined]) === true);
console.log(setsMatch(findAll(0,values),[0,"0"]) === true);
console.log(setsMatch(findAll(-0,values),[-0]) === true);
console.log(setsMatch(findAll(13,values),[13]) === true);
console.log(setsMatch(findAll(42,values),[42,"42"]) === true);
console.log(setsMatch(findAll(NaN,values),[NaN]) === true);
console.log(setsMatch(findAll(-Infinity,values),[-Infinity]) === true);
console.log(setsMatch(findAll(Infinity,values),[Infinity]) === true);
console.log(setsMatch(findAll("",values),[""]) === true);
console.log(setsMatch(findAll("0",values),[0,"0"]) === true);
console.log(setsMatch(findAll("42",values),[42,"42"]) === true);
console.log(setsMatch(findAll("42hello",values),["42hello"]) === true);
console.log(setsMatch(findAll("true",values),["true"]) === true);
console.log(setsMatch(findAll(true,values),[true]) === true);
console.log(setsMatch(findAll(false,values),[false]) === true);
console.log(setsMatch(findAll(myObj,values),[myObj]) === true);

console.log(setsMatch(findAll(null,values),[null,0]) === false);
console.log(setsMatch(findAll(undefined,values),[NaN,0]) === false);
console.log(setsMatch(findAll(0,values),[0,-0]) === false);
console.log(setsMatch(findAll(42,values),[42,"42hello"]) === false);
console.log(setsMatch(findAll(25,values),[25]) === false);
console.log(setsMatch(findAll(Infinity,values),[Infinity,-Infinity]) === false);
console.log(setsMatch(findAll("",values),["",0]) === false);
console.log(setsMatch(findAll("false",values),[false]) === false);
console.log(setsMatch(findAll(true,values),[true,"true"]) === false);
console.log(setsMatch(findAll(true,values),[true,1]) === false);
console.log(setsMatch(findAll(false,values),[false,0]) === false);

// ***************************
```

#### Scope
- JavaScript is not an interpreted language; it is a compiled language.

#### Dyanamic Global Variables
- In the following, the variable age is never declared within the `test` function, though when it is assigned as such upon calling the function, a new dynamic global variable called `age` is created.
```js
  let name = 'Reuben';
  function test() {
    age = 29;
  }
  test();
  console.log(name, age); // outputs Reuben 29
```

#### Strict Mode
- By using strict mode, you can avoid the dynamic declaration of globl variables
```js
  'use strict';
  let name = 'Reuben';
  function test() {
    age = 29; // Uncaught ReferenceError: age is not defined
  }
  test();
  console.log(name, age);
```

#### Function Expressions
- A function expression is any instance in which you assign the reference to a function to a variable, such as `test 2`
- Function definitions exist within the scope that they are declared.
- Function expressions exist within their own new scope - note that the variable `test2` is assigned a reference to the `expression` function, which can be used to invoke that function. `expression` can not be invoked using that identifier in the scope of which `test2` is declared.

```js
  function test1() { console.log('test 1') }; // Function declaration
  let test2 = function expression() { // Function expression
    console.log('test 2');
    console.log(expression);
  }

  test1(); // Outputs test 1
  test2(); // Outputs test 2 \n [Function: expression]
  expression(); // Reference error
```

```js
  let a = function() {} // Anonymous function expression
  let b = function named() {} // Named function expression
```

#### Naming Function Expressions
Main benefits to naming function expressions:
1. Reliable function self reference; a named function expression has a read only reference to itself within its scope using its identifier - useful for recursion. Anonymous function expressions would rely on referencing the variable in the function's parent scope, which is not read only, meaning the reference to that function expression can be overwritten
1. More debuggable stack traces - this is huge and I can recognise this straight away - I'm so used to seeing a stack trace full of anonymous function, which is very hard to debug
1. More self documenting code as naming a function expression documents the function's intent

#### Function Types Heirarchy
- Presenter reckons named function declarations > named function expressions > arrow functions (anonymous)

#### Function Expression Exercise
In this exercise, you will be writing some functions and function expressions, to manage the student enrollment records for a workshop.

**Note:** The spirit of this exercise is to use functions wherever possible and appropriate, so consider usage of array utilities `map(..)`, `filter(..)`, `find(..)`, `sort(..)`, and `forEach(..)`.

## Instructions (Part 1)

**Note:** In Part 1, use only function declarations or named function expressions.

You are provided three functions stubs -- `printRecords(..)`, `paidStudentsToEnroll()`, and `remindUnpaid(..)` -- which you must define.

At the bottom of the file you will see these functions called, and a code comment indicating what the console output should be.

1. `printRecords(..)` should:
	- take a list of student Ids
	- retrieve each student record by its student Id (hint: array `find(..)`)
	- sort by student name, ascending (hint: array `sort(..)`)
	- print each record to the console, including `name`, `id`, and `"Paid"` or `"Not Paid"` based on their paid status

2. `paidStudentsToEnroll()` should:
	- look through all the student records, checking to see which ones are paid but **not yet enrolled**
	- collect these student Ids
	- return a new array including the previously enrolled student Ids as well as the to-be-enrolled student Ids (hint: spread `...`)

3. `remindUnpaid(..)` should:
	- take a list of student Ids
	- filter this list of student Ids to only those whose records are in unpaid status
	- pass the filtered list to `printRecords(..)` to print the unpaid reminders

## Instructions (Part 2)

Now that you've completed Part 1, refactor to use **only** `=>` arrow functions.

For `printRecords(..)`, `paidStudentsToEnroll()`, and `remindUnpaid(..)`, assign these arrow functions to variables of such names, so that the execution still works.

As the appeal of `=>` arrow functions is their conciseness, wherever possible try to use only expression bodies (`x => x.id`) instead of full function bodies (`x => { return x.id; }`).

```js
// Named function declarations

function mapIds(student) {
	return student.id;
}

function printRecords(recordIds) {
	let result = [];
	function eachStudentId(id) {
		function studentById(record) {
			return record.id == id;
		}
		result.push(studentRecords.find(studentById));
	}
	function nameSort(recordA, recordB) {
		return recordA.name < recordB.name ? -1 : 1; 
	}
	function printStudent(student) {
		console.log(student.name, student.id, student.paid ? 'PAID' : 'UNPAID');
	}
	recordIds.forEach(eachStudentId);
	result.sort(nameSort);
	result.forEach(printStudent)
}
function paidStudentsToEnroll() {
	function getPaidNotEnrolled(student) {
		return student.paid && !currentEnrollment.includes(student.id);
	};
	return [...currentEnrollment, ...studentRecords.filter(getPaidNotEnrolled).map(mapIds)];
}

function remindUnpaid(recordIds) {
	function getUnpaidEnrollments(student) {
		return !student.paid && recordIds.includes(student.id)
	}
	printRecords(studentRecords.filter(getUnpaidEnrollments).map(mapIds));
}

// Anonymous function expressions

function printRecords(recordIds) {
	let result = [];
	recordIds.forEach(id => result.push(studentRecords.find(record => record.id == id)));
	result.sort((recordA, recordB) => recordA.name < recordB.name ? -1 : 1 );
	result.forEach(student => console.log(student.name, student.id, student.paid ? 'PAID' : 'UNPAID'))
}
function paidStudentsToEnroll() {
	return [
		...currentEnrollment, 
		...studentRecords.filter(student => student.paid && !currentEnrollment.includes(student.id)).map(student => student.id)];
}
function remindUnpaid(recordIds) {
	printRecords(studentRecords.filter(student => !student.paid && recordIds.includes(student.id)).map(student => student.id));
}


// ********************************

var currentEnrollment = [ 410, 105, 664, 375 ];

var studentRecords = [
	{ id: 313, name: "Frank", paid: true, },
	{ id: 410, name: "Suzy", paid: true, },
	{ id: 709, name: "Brian", paid: false, },
	{ id: 105, name: "Henry", paid: false, },
	{ id: 502, name: "Mary", paid: true, },
	{ id: 664, name: "Bob", paid: false, },
	{ id: 250, name: "Peter", paid: true, },
	{ id: 375, name: "Sarah", paid: true, },
	{ id: 867, name: "Greg", paid: false, },
];

printRecords(currentEnrollment);
console.log("----");
currentEnrollment = paidStudentsToEnroll();
printRecords(currentEnrollment);
console.log("----");
remindUnpaid(currentEnrollment);

/*
	Bob (664): Not Paid
	Henry (105): Not Paid
	Sarah (375): Paid
	Suzy (410): Paid
	----
	Bob (664): Not Paid
	Frank (313): Paid
	Henry (105): Not Paid
	Mary (502): Paid
	Peter (250): Paid
	Sarah (375): Paid
	Suzy (410): Paid
	----
	Bob (664): Not Paid
	Henry (105): Not Paid
*/
```

#### Lexical & Dynamic Scoping
- Lexical scope is determined at compile time
- Dynamic scope is determine at run time
- JavaScript is a lexically scoped language
- The following pushes each product in the `products` array that has an `id` matching an `id` in the `productIds` array. Note that the `forEach` function's call back is a function called `eachId`, which within itself, a function called `productById` is declared. `productById` references a variable called `id`, which itself does not define, though it has access to a reference of a variable with that identifier through its parent scope (the scope of `eachId` as `id` is passed in as a parameter). So this works and is completely normal, as when `productById` doesn't find `id` within its own scope, it looks one level above. This is essentially lexical scoping.
```js
// Lexically scoped example
const products = [
  {id: 1, name: 'Product 1'},
  {id: 2, name: 'Product 2'},
  {id: 3, name: 'Product 3'},
  {id: 4, name: 'Product 4'},
  {id: 5, name: 'Product 5'}
];
const productIds = [3,4,1];

const matched = [];

function eachId(id) {
  function productById(product) {
    return product.id == id;
  }
  const product = products.find(productById);
  if (product) matched.push(product);
}

productIds.forEach(eachId);

console.log(matched);
```
- The following snippet shows an example of how, in theory, dynamic scoping works. Notice that `productById` is no longer implemented within the `eachId` function. Of course, this will fail in JavaScript, as when `productById` is called in `eachId`, it has no reference to any variable `id`. However, in terms of dynamic scoping, the value of `id` would be resolved from within the function that `productId` is called. What this means is that `eachId` recieves a parameter `id` and `productById` is called within `eachId`, so dynamic scoping would resolve the reference to `id` within `productId` to the parameter `id` that `eachId` recieves.
```js
// Dynamic scoped example (purely theoretical)
const products = [
  {id: 1, name: 'Product 1'},
  {id: 2, name: 'Product 2'},
  {id: 3, name: 'Product 3'},
  {id: 4, name: 'Product 4'},
  {id: 5, name: 'Product 5'}
];
const productIds = [3,4,1];

const matched = [];

function productById(product) {
  return product.id == id;
}

function eachId(id) {
  const product = products.find(productById);
  if (product) matched.push(product);
}

productIds.forEach(eachId);

console.log(matched);
```

#### What is closure?
- Closure is when a function remembers it's lexical scope even when that function is executed outside that lexical scope.

#### The `this` keyword
- A function's `this` references the execution context for that call, determined entirely by how the function was called.
- In the example below, `call` on `ask.call` binds the reference to `this` in the `ask` function to the `context` object decalred in `callAsk`. This is an example of an explicit binding because we're telling JavaScript what the `ask` function will use as its `this` reference.
```js
function ask(question) {
	console.log(this.techer question);
}

function callAsk() {
	let context = {
		teacher: 'reuben'
	};

	ask.call(context, 'Why?');
}

callAsk(); // outputs reuben Why?
```

- In the example below, `this` references the object in which it is accessed. Furthermore, the call site of `ask` indicates that `this` references `workshop`. This is implicit binding.
```js
let workshop = {
	teacher: 'reuben',
	ask: function(question) {
		console.log(this.teacher, question)
	}
}

workshop.ask('Why?'); // outputs reuben Why?
```
- The example below is referred to as hard binding. In the first `setTimeout`, you pass a reference to the function `ask`, but when it is invoked, it is not invoked with the context `workshop`, so you lose the binding to `this`. The second example solves that issue by using `bind` to bind the reference to `this` for when the callback within `setTimeout` is called.
```js
let workshop = {
	teacher: 'reuben',
	ask: function(question) {
		console.log(this.teacher, question)
	}
}

setTimeout(workshop.ask, 1000, 'Why?'); // outputs undefined Why?
setTimeout(workshop.ask.bind(workshop), 1000, 'Why?'); // outputs reuben Why?
```

#### The `new` keyword
1. Create a brand new empty object
1. Links that object to another object `prototype`
1. Call the function with `this` set to new empty object
1. If function does not return an object, assume return of `this`


#### Arrow functions and lexical `this`
- An arrow function does not define a `this` keyword at all. If you put a this within an arrow function, it will lexically resolve the value of `this`. In the case below, `this` refers to the `workshop` object.
```js
let workshop = {
	teacher: 'reuben',
	ask: function(question) {
    	setTimeout(() => console.log(this.teacher, question), 1000)
	}
}

workshop.ask('lmofl');
```