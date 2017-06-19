## 3. Хвостовая рекурсия

Функция является "хвостовой рекурсией", если основные рекурсивные вызовы, которые она делает, находятся в хвостовых позициях.

Например, следующая функция не является хвостовой рекурсией, поскольку основной рекурсивный вызов в строке A не находится в хвостовой позиции:

```js
function factorial(x) {
    if (x <= 0) {
        return 1;
    } else {
        return x * factorial(x-1); // (A)
    }
}
```

Функция factorial() может быть реализована через вспомогательную функцию facRec(). В данном случае основной рекурсивный вызов в строке A находится в хвостовой позиции:

```js
function factorial(n) {
    return facRec(n,1);
}
function facRec(x, acc) {
    if (x <= 1) {
        return acc;
    } else {
        return facRec(x-1, x*acc); // (A)
    }
}
```

То есть некоторые функции, не являющиеся хвостовыми рекурсиями, могут быть в них преобразованы.

### 3.1 Циклы через хвостовую рекурсию

Оптимизация хвостовых вызовов позволяет реализовать циклы через рекурсию, не увеличивая стек. Ниже приведены два примера:

#### forEach()

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

#### findIndex()

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