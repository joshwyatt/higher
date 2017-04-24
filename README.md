# Higher Order Array Methods

This article focuses on improving your fluency in using 4 of the main higher order `Array` methods:

- `forEach`
- `map`
- `filter`
- `reduce`

## Summary of Array Method Uses

- **`forEach`** Use to perform operations on every element without creating a new array.
- **`map`** Use to return a new array of the same length where each element has been processed in the same way.
- **`filter`** Use to return a (potentially) smaller array based on a truth test, without modifying elements.
- **`reduce`** Use to return a single value based on a reduction of each of the elements in the array.

## `Array.prototype.forEach`

`forEach` expects a callback which will be called for every element in the array. The callback will be passed three arguments: the element, the index of the element, and the entire collection. `forEach` always returns `undefined`.

**Use `forEach` when you need to perform an action for each element in the array without creating a new array.**

### Example Implementation of `forEach`

```javascript
Array.prototype.forEach = function (callback) {
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

## `Array.prototype.map`

`map` expects a callback which will be called for every element in the array. The callback will be passed three arguments: the element, the index of the element, and the entire collection. `map` creates a new array, and pushes the result of the return value from calling `callback` on each element to this new array. `map` then returns this new array.

**Use `map` when you need a new array the same size as the original, but with each of the elements modified by the same operation.**

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

## `Array.prototype.filter`

`filter` expects a callback that returns `true` or `false`. `filter` returns a new array containing the elements from the original array that return `true` when passed into the callback.

**Use `filter` when you need to take an array and return a (potentially) smaller version of it, without modifying any of the elements.**

### Example implemention of `filter`

```javascript
Array.prototype.filter = function(callback) {
  let result = [];

  this.forEach((element, i, collection) => {
    if (callback(element, i, collection) === true) {
      result.push(element);
    }
  });

  return result;
}
```

### Example uses for `filter`

```javascript
let nums = [1, 2, 3, 4, 5, 6, 7];

let oddNums = nums.filter(num => num % 2);
```

Because `filter` and `map` both return arrays, they are easily chained:

```javascript
const double = (num) => num * 2
const greaterThanSix = (num) => num > 6

let nums = [1, 2, 3, 4, 5, 6, 7, 8]

let doubledNumsGreaterThanSix = nums.map(double).filter(greaterThanSix)
let numsGreaterThanSixDoubled = nums.filter(greaterThanSix).map(double)
```

## `Array.prototype.reduce`

`reduce` creates a single value out of an array. In order to do this, it uses accepts a callback function called an *accumulator*. The accumulator takes two values, and returns a single value. Before demonstrating `reduce`, here are a few example accumulators, notice that they each take two values and return a single value:

```javascript
const smaller = (a, b) => a < b ? a : b

const longer = (a, b) => a.length > b.length ? a : b

const sum = (a, b) => a + b

const product = (a, b) => a * b

const concatenated = (a, b) => a + b // for strings
const concatenated = (a, b) => [...a, ...b] // for arrays
```

`reduce` calls the passed in accumulator on every element in the array. The first argument of the accumulator is the result of the last call to the accumulator. If it is the first call to the accumulator, the first element of the array is passed in, or, if supplied, a default starting value. The second argument to the accumulator is the current element in the array.

**Use `reduce` when you need to reduce an array down to a single value.**

### Example implemention of `reduce`

```javascript
Array.prototype.reduce = function(accumulator, startingValue) {
  if (startingValue === undefined) {
    startingValue = this.shift();
  }
  let reduction = startingValue;

  this.forEach((element) => {
    reduction = accumulator(reduction, element)
  });

  return reduction;
}
```

### Example uses for `reduce`

```javascript
let nums = [1, 2, 3, 4, 5, 6, 7];

let total = nums.reduce((total, num) => total + num)
```

```javascript
let nums = [5, 2, 8, 1];

let largest = nums.reduce((total, num) => num > total ? num : total)
```

## Summary of Array Method Uses

- **`forEach`** Use to perform operations on every element without creating a new array.
- **`map`** Use to return a new array of the same length where each element has been processed in the same way.
- **`filter`** Use to return a (potentially) smaller array based on a truth test, without modifying elements.
- **`reduce`** Use to return a single value based on a reduction of each of the elements in the array.

## Example

Log the sum of all the numbers, and number like strings in the following array to the console:

```javascript
let values = ['letters', 3, '5', 4, ['a', 'b'], '2', false, 8, () => {}];
```

Being familiar with the uses of the higher order array methods, we can approach this problem as follows:

1) I'd like to try and convert every element in this array to be a number (**map**)
2) I can then filter for values that are not `NaN` (**filter**)
3) I can then reduce the remaining numbers down to a sum (**reduce**)

I'd like to start with a series of helper functions for each of these steps:

Step **1**: The native `parseInt` function will do this for me.

Step **2**:

```javascript
const isNotNaN = (num) => !isNaN(num)
```

Step **3**:

```javascript
const sum = (a, b) => a + b
```

Putting it all together:

```javascript
function makeSumOfNumberLikes(values) {
  return values
    .map((val) => parseInt(val))
    .filter(num => !isNaN(num))
    .reduce((total, num) => total + num);
}

let values = ['letters', 3, '5', 4, ['a', 'b'], '2', false, 8, () => {}];
let sumOfNumberLikes = makeSumOfNumberLikes(values);
console.log(sumOfNumberLikes);
```
