# 3. Tail-recursive functions

A function is *tail-recursive*, if the main recursive calls it makes are in tail positions.

For example, the following function is not tail recursive, because the main recursive call in line A is not in a tail position:

```js
function factorial(x) {
    if (x <= 0) {
        return 1;
    } else {
        return x * factorial(x-1); // (A)
    }
}
```

factorial() can be implemented via a tail-recursive helper function facRec(). The main recursive call in line A is in a tail position.

```js
function factorial(n) {
    return facRec(n, 1);
}
function facRec(x, acc) {
    if (x <= 1) {
        return acc;
    } else {
        return facRec(x-1, x*acc); // (A)
    }
}
```

That is, some non-tail-recursive functions can be transformed into tail-recursive functions.

## 3.1 Tail-recursive loops

Tail call optimization makes it possible to implement loops via recursion without growing the stack. The following are two examples.

## forEach()

```js
function forEach(arr, callback, start = 0) {
    if (0 <= start && start < arr.length) {
        callback(arr[start], start, arr);
        return forEach(arr, callback, start+1); // tail call
    }
}
forEach(['a', 'b'], (elem, i) => console.log(`${i}. ${elem}`));

// Output:
// 0. a
// 1. b
```

## findIndex()

```js
function findIndex(arr, predicate, start = 0) {
    if (0 <= start && start < arr.length) {
        if (predicate(arr[start])) {
            return start;
        }
        return findIndex(arr, predicate, start+1); // tail call
    }
}
findIndex(['a', 'b'], x => x === 'b'); // 1
```