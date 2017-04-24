# Higher Order Array Methods

This article focuses on 4 main higher order `Array` methods:

- `forEach`
- `map`
- `filter`
- `reduce`

## `Array.forEach`

`forEach` expects a callback which will be called for every element in the array. The callback will be passed three arguments: the element, the index of the element, and the entire collection. `forEach` always returns `undefined`.

**Use `forEach` when you need to perform an action for each element in the array without creating a new array.**

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

### Example Implementation of `forEach`

```javascript
function Array.prototype.forEach(callback) {
  for (let i = 0; i < this.length; i++) {
    callback(this[i], i, this);
  }
}
```
