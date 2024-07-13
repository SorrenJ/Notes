
The difference between considering a structure as an array versus an object in programming, particularly in JavaScript, depends on the syntax and how data is indexed and accessed.

### Array

An array is a collection of elements accessed by their index (a numerical value).

**Old version: Array of objects**

```js

[ 
{ name: String, 
 studentId: String, 
 numberOfClasses: Number 
 },
  ... 
  ]

```



This is an array because the elements are accessed by their position in the array (e.g., `array[0]`, `array[1]`, etc.).


### Object

An object is a collection of key-value pairs where keys are unique strings.

**With keys: Object of objects**


```js
{
  studentId: {
    name: String,
    studentId: String,
    numberOfClasses: Number
  },
  ...
}

```

This is considered an object because each entry is accessed by a unique key (e.g., `object["studentId"]`). Here, the `studentId` serves as the key.


Another example: Object of objects


``` js

{
  "MURB090909": {
    name: "Buddy Murillo",
    studentId: "MURB090909",
    numberOfClasses: 2
  },
  "TAND060606": {
    name: "Dilan Tanner",
    studentId: "TAND060606",
    numberOfClasses: 5
  },
  "SKIS424242": {
    name: "Sophia Skinner",
    studentId: "SKIS424242",
    numberOfClasses: 1
  },
  "HALA101010": {
    name: "Antony Hale",
    studentId: "HALA101010",
    numberOfClasses: 0
  }
}

```


This structure is considered an object because each entry is accessed by a unique string key (e.g., `object["MURB090909"]`). In this case, the `studentId` values are used as keys.

### Summary

- **Array:** Indexed by numerical values, order matters. Example: `[{}, {}, ...]`.
- **Object:** Indexed by unique string keys, order does not matter. Example: `{ "key1": {}, "key2": {}, ... }`.

The first example is an array of objects, while the latter two examples are objects of objects where the keys are `studentId` values.


### Accessing Data from an Array of Objects

Consider the array of objects example:


``` js
let studentsArray = [
  {
    name: "Buddy Murillo",
    studentId: "MURB090909",
    numberOfClasses: 2
  },
  {
    name: "Dilan Tanner",
    studentId: "TAND060606",
    numberOfClasses: 5
  },
  {
    name: "Sophia Skinner",
    studentId: "SKIS424242",
    numberOfClasses: 1
  },
  {
    name: "Antony Hale",
    studentId: "HALA101010",
    numberOfClasses: 0
  }
];

// Accessing the first student's name
console.log(studentsArray[0].name); // Output: Buddy Murillo

// Accessing the second student's studentId
console.log(studentsArray[1].studentId); // Output: TAND060606

// Accessing the third student's number of classes
console.log(studentsArray[2].numberOfClasses); // Output: 1

```

### Accessing Data from an Object of Objects

Consider the object of objects example:

``` js
let studentsObject = {
  "MURB090909": {
    name: "Buddy Murillo",
    studentId: "MURB090909",
    numberOfClasses: 2
  },
  "TAND060606": {
    name: "Dilan Tanner",
    studentId: "TAND060606",
    numberOfClasses: 5
  },
  "SKIS424242": {
    name: "Sophia Skinner",
    studentId: "SKIS424242",
    numberOfClasses: 1
  },
  "HALA101010": {
    name: "Antony Hale",
    studentId: "HALA101010",
    numberOfClasses: 0
  }
};

// Accessing Buddy Murillo's name using his studentId
console.log(studentsObject["MURB090909"].name); // Output: Buddy Murillo

// Accessing Dilan Tanner's studentId
console.log(studentsObject["TAND060606"].studentId); // Output: TAND060606

// Accessing Sophia Skinner's number of classes
console.log(studentsObject["SKIS424242"].numberOfClasses); // Output: 1

```

### Summary

- For arrays, use the index to access elements: `array[index].property`.
- For objects, use the key to access elements: `object[key].property`.


### Looping Through an Array of Objects

Consider the array of objects example:

``` js
let studentsArray = [
  {
    name: "Buddy Murillo",
    studentId: "MURB090909",
    numberOfClasses: 2
  },
  {
    name: "Dilan Tanner",
    studentId: "TAND060606",
    numberOfClasses: 5
  },
  {
    name: "Sophia Skinner",
    studentId: "SKIS424242",
    numberOfClasses: 1
  },
  {
    name: "Antony Hale",
    studentId: "HALA101010",
    numberOfClasses: 0
  }
];

// Loop through the array and print each student's name
for (let i = 0; i < studentsArray.length; i++) {
  console.log(studentsArray[i].name);
}

// Alternatively, using forEach
studentsArray.forEach(student => {
  console.log(student.name);
});


```

### Looping Through an Object of Objects

Consider the object of objects example:

``` js
let studentsObject = {
  "MURB090909": {
    name: "Buddy Murillo",
    studentId: "MURB090909",
    numberOfClasses: 2
  },
  "TAND060606": {
    name: "Dilan Tanner",
    studentId: "TAND060606",
    numberOfClasses: 5
  },
  "SKIS424242": {
    name: "Sophia Skinner",
    studentId: "SKIS424242",
    numberOfClasses: 1
  },
  "HALA101010": {
    name: "Antony Hale",
    studentId: "HALA101010",
    numberOfClasses: 0
  }
};

// Loop through the object and print each student's name
for (let key in studentsObject) {
  if (studentsObject.hasOwnProperty(key)) {
    console.log(studentsObject[key].name);
  }
}

// Alternatively, using Object.keys and forEach
Object.keys(studentsObject).forEach(key => {
  console.log(studentsObject[key].name);
});


```

