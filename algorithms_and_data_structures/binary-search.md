## Binary search
O(Log(n))

The array must be ordered
```js
const arr = [1, 2, 7, 9, 16, 20, 29, 35];
```
### Iterative implementation
```js
function binarySearch (array, x) {
  let [left, right] = [0, array.length - 1];

  while(left <= right) {
    let mid = Math.trunc((left + right) / 2);
    if (arr[mid] === x) {
      return true;
    } else if (x < arr[mid]) {
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }
  return false;
}

binarySearch(arr, 2);
```
