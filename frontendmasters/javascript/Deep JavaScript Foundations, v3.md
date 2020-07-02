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
		return false
	
	
		function isNeg0(val) {
			return val === 0 && (1 / val) === -Infinity;
		}
	
		function isItNaN(val) {
      // NaN is the only value in JavaScript that is not equal to itself
			return val !== val;
		}
	}
}


// tests:
console.log(Object.is(42,42) === true);
console.log(Object.is("foo","foo") === true);
console.log(Object.is(false,false) === true);
console.log(Object.is(null,null) === true);
console.log(Object.is(undefined,undefined) === true);
console.log(Object.is(NaN,NaN) === true);
console.log(Object.is(-0,-0) === true);
console.log(Object.is(0,0) === true);

console.log(Object.is(-0,0) === false);
console.log(Object.is(0,-0) === false);
console.log(Object.is(0,NaN) === false);
console.log(Object.is(NaN,0) === false);
console.log(Object.is(42,"42") === false);
console.log(Object.is("42",42) === false);
console.log(Object.is("foo","bar") === false);
console.log(Object.is(false,true) === false);
console.log(Object.is(null,undefined) === false);
console.log(Object.is(undefined,null) === false);

```