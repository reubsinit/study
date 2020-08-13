#### Generator Functions With Dynamic Data
```js
function *createFlow() {
    const num = 10;
    const newNum = yield num;
    yield 5 + num;
    yield 6;
}

const returnNextElement = createFlow();
const element1 = returnNextElement.next(); // returns 10
const element2 = returnNextElement.next(2); // returns 7
```

The generator function above on the first `yield` will return the value `10`.  Note the line in which the generator yields the first value: `const newNum = yield num` - at this point, `newNum` is not assigned anything at all. You cannot assign `yield` to any variable. Instead what happens is that the value of `num` is return immediately. Then when `returnNextElement.next(2)` is called, what happens is that the argument passed in, `2`, is used as the value being assigned to `newNum`, which means that the next time the generator returns a value is `yield 5 + num`, where `num,` is `2`, hence the returned value of `7`.