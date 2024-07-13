Immutable basically means something that cannot be changed. In programming, immutable is used to describe a value that cannot be changed after it's been set.



Not immutable

``` js
// Objects
const object = {
  key: "Value of Key",
};

const copyOfObject = object;

// Arrays
const arrayOfNumbers = [1, 2, 3, 4, 5];

const copyArrayOfNumbers = arrayOfNumbers;

// Modify arrayOfNumbers
arrayOfNumbers.push(6);

console.log(arrayOfNumbers); // Output: [1, 2, 3, 4, 5, 6]
console.log(copyArrayOfNumbers); // Output: [1, 2, 3, 4, 5, 6]
```


As you can see from the output, both `arrayOfNumbers` and `copyArrayOfNumbers` are modified because they both reference the same array in memory. Therefore, any change made to `arrayOfNumbers` will also be seen in `copyArrayOfNumbers`.



Immutable 


``` js

// Objects

const object = {

  key: "Value of Key",

};


const copyOfObject = { ...object };

//Arrays

const arrayOfNumbers = [1, 2, 3, 4, 5];


const copyArrayOfNumbers = [...arrayOfNumbers];



// Modify arrayOfNumbers

arrayOfNumbers.push(6);


console.log(arrayOfNumbers); // Output: [1, 2, 3, 4, 5, 6]

console.log(copyArrayOfNumbers); // Output: [1, 2, 3, 4, 5, 6]
```


Immutable object

```js

// Objects

const object = {

  key: "Value of Key",

};

  
const copyOfObject = { ...object }; 

// Modify object

object.newKey = "New Value";

console.log(object); // Output: { key: "Value of Key", newKey: "New Value" }

console.log(copyOfObject); // Output: { key: "Value of Key" }


// Arrays

const arrayOfNumbers = [1, 2, 3, 4, 5];

const copyArrayOfNumbers = [...arrayOfNumbers];

  

// Modify arrayOfNumbers

arrayOfNumbers.push(6);

console.log(arrayOfNumbers); // Output: [1, 2, 3, 4, 5, 6]

console.log(copyArrayOfNumbers); // Output: [1, 2, 3, 4, 5]

```


- When we add a new key to the `object`, it does not affect `copyOfObject` because `copyOfObject` is a shallow copy of `object`.
- When we modify `arrayOfNumbers` using `.push(6)`, it does not affect `copyArrayOfNumbers` because `copyArrayOfNumbers` is a shallow copy of `arrayOfNumbers`.

This demonstrates that using the spread operator creates a new, independent copy of the original data structures, and modifications to the original do not affect the copied version.



## Spread Operator

The JavaScript spread operator (`...`) allows us to quickly copy all or part of an existing array or object into another array or object.


``` js

const array1 = [1, 2, 3];
const array2 = [...array1]; // Copies array1 into array2

console.log(array2); // Output: [1, 2, 3]

const array3 = [4, 5, 6];
const combinedArray = [...array1, ...array3]; // Merges array1 and array3

console.log(combinedArray); // Output: [1, 2, 3, 4, 5, 6]

```


In this example:

- `array2` is a shallow copy of `array1`. Any changes to `array1` after the copy will not affect `array2`.
- `combinedArray` merges the elements of `array1` and `array3`.

### Shallow Copy

The spread operator creates a shallow copy, meaning it only copies the first level of the array or object. If the array or object contains other arrays or objects (nested structures), the nested structures are not deeply copied. Instead, references to the nested structures are copied.


``` js 
const nestedArray = [[1, 2], [3, 4]];
const copyOfNestedArray = [...nestedArray];

nestedArray[0][0] = 99;

console.log(nestedArray); // Output: [[99, 2], [3, 4]]
console.log(copyOfNestedArray); // Output: [[99, 2], [3, 4]]

const nestedObject = { a: { b: 1 } };
const copyOfNestedObject = { ...nestedObject };

nestedObject.a.b = 99;

console.log(nestedObject); // Output: { a: { b: 99 } }
console.log(copyOfNestedObject); // Output: { a: { b: 99 } }


```


In this example:

- Changing `nestedArray` or `nestedObject` affects their copies (`copyOfNestedArray` and `copyOfNestedObject`) because the nested structures are still references to the same objects in memory.