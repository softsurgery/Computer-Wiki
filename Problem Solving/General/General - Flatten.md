```js
function flatten(arr, index = 0, result = []) {
  if (index >= arr.length) {
    return result;
  }

  const item = arr[index];

  if (Array.isArray(item)) {
    flatten(item, 0, result);
  } else {
    result.push(item);
  }

  return flatten(arr, index + 1, result);
}

console.log(flatten(['a', ['b', 2], 3, null, [[4], ['c']]]));
// Output: ['a', 'b', 2, 3, null, 4, 'c']
```