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
