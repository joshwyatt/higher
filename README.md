# Higher Order Array Methods

This article focuses on 4 main higher order `Array` methods:

- `forEach`
- `map`
- `filter`
- `reduce`

## `Array.forEach`

`forEach` expects a callback which will be called for every element in the array. The callback will be passed three arguments: the element, the index of the element, and the entire collection. `forEach` always returns `undefined`.

**Use `forEach` when you need to perform an action for each element in the array without creating a new array.**

### Example Implementation of `forEach`

```javascript
function Array.prototype.forEach(callback) {
  for (let i = 0; i < this.length; i++) {
    callback(this[i], i, this);
  }
}
```

### Example Use Cases for `forEach`

```javascript
function logEveryElement(array) {
  array.forEach(element => {
    console.log(element);
  });
}

let elements = [1, 2, 3 ,4];
logEveryElement(elements); // logs 1, 2, 3, 4
```

```javascript
function logEveryOtherElement(array) {
  array.forEach((element, index) => {
    if ( !(index % 2) ) {
      console.log(element);
    }
  });
}

let elements = [1, 2, 3 ,4];
logEveryOtherElement(elements); // logs 1, 3
```

```javascript
function logEveryElementGreaterThanArrayLength(array) {
  array.forEach((element, index, collection) => {
    if (element > collection.length) {
      console.log(element);
    }
  });
}

let elements = [10, 1, 3, 5];
logEveryElementGreaterThanArrayLength(elements); // logs 10, 5
```

Notice that in the examples above, we use our examples for their *side effects* and not to return or modify the passed in array.


## `Array.map`

`map` expects a callback which will be called for every element in the array. The callback will be passed three arguments: the element, the index of the element, and the entire collection. `map` creates a new array, and pushes the result of the return value from calling `callback` on each element to this new array. `map` then returns this new array.

**Use `map` when you need a new array the same size as the original, but with some operation performed on each of the elements.**

### Example implemention of `map`

```javascript
Array.prototype.map = function(callback) {
  let result = [];

  this.forEach((element, i, collection) => {
    result.push(callback(element, i, collection)); // The value returned from calling `callback`
                                                   // is pushed into the `result` array.
  });

  return result;
}
```

### Example uses for `map`

```javascript
let nums = [1, 2, 3, 4];

let doubledNums = nums.map(num => num * 2);
```

You can define functions to pass into `map` elsewhere, assuming their function signature follows the signature expected by `map`:

```javascript
function triple(num) {
  return num * 3;
}

let nums = [2, 3, 4];

let tripledNums = nums.map(triple);
```
