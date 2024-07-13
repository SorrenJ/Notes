

## Filtered Array

Arrow function callback

``` js

const anArrayMixedElements = [1, "hello", 80, "world", 24, "javascript", 6, 99, "LHL", 12, "bootcamp", 45, 3];

const filteredArray = anArrayMixedElements.filter(element => typeof element === 'number');

console.log(filteredArray); // Output: [1, 80, 24, 6, 99, 12, 45, 3]

```



Traditional function callback

``` js

const anArrayMixedElements = [1, "hello", 80, "world", 24, "javascript", 6, 99, "LHL", 12, "bootcamp", 45, 3];

function isNumber(element) {
  return typeof element === 'number';
}

const filteredArray = anArrayMixedElements.filter(isNumber);

console.log(filteredArray); // Output: [1, 80, 24, 6, 99, 12, 45, 3]


```


## Longest book


### Arrow Function Explanation:

```js

var longestBook = filteredBooks.reduce(function(acc, curr) { return curr.pages > acc.pages ? curr : acc; });
```

The arrow function provided is part of the `.reduce()` method applied to the `filteredBooks` array. The `.reduce()` method takes a callback function and an optional initial value, and it applies the callback function to each element in the array, accumulating a single result.

In this case:

- `acc` is the accumulator, which starts as the first element of the array if no initial value is provided.
- `curr` is the current element being processed in the array.
- The callback function compares the number of pages in `curr` and `acc`. If `curr.pages` is greater than `acc.pages`, `curr` becomes the new accumulator.
- The result is the book object with the most pages in the `filteredBooks` array.


- **Condition**: `curr.pages > acc.pages`
    
    - This checks if the current book (`curr`) has more pages than the accumulated book (`acc`).
- **True Expression**: `curr`
    
    - If `curr.pages` is greater than `acc.pages`, the ternary operator returns `curr`, making it the new accumulator.
- **False Expression**: `acc`
    
    - If `curr.pages` is not greater than `acc.pages`, the ternary operator returns `acc`, keeping the current accumulator unchanged.

The `?` syntax used in the arrow function is part of the ternary operator in JavaScript. The ternary operator is a shorthand way of writing an `if-else` statement and is used to assign a value based on a condition.

### Ternary Operator Syntax

The ternary operator is structured as follows:

``` js
condition ? expressionIfTrue : expressionIfFalse

```

- `condition`: This is the condition that is evaluated. If the condition is true, the operator returns `expressionIfTrue`; otherwise, it returns `expressionIfFalse`.
- `?`: The question mark separates the condition from the expression that will be executed if the condition is true.
- `:`: The colon separates the true expression from the false expression.



### Traditional Function Explanation:

``` js

var longestBook = filteredBooks.reduce(function(acc, curr) {
  if (curr.pages > acc.pages) {
    return curr;
  } else {
    return acc;
  }
});

```

- **Initialization**: If an initial value is not provided, `acc` starts as the first element of the array.
- **Iteration**: The function iterates over each element (`curr`) in the `filteredBooks` array.
- **Comparison**: For each element, it compares `curr.pages` with `acc.pages`.
- **Update Accumulator**: If `curr.pages` is greater than `acc.pages`, `curr` becomes the new accumulator (`acc`).
- **Return**: At the end of the iteration, the function returns the accumulator, which is the book object with the most pages.
