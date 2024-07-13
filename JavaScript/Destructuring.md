
Before being destructured


```javascript
// Arrays
const animals= ['labrador', 'tabby', 'parrot'];

const dog= animals[0];
const cat= animals[1];
const bird= animals[2];

console.log(dog); // ‘labrador’
```


The destructuring allows us to unpack values from arrays, or properties from objects, into distinct variables.

It allows us to unpack as many values of the array as we want. Using the same array as our previous example, let's see how we can declare `dog`, `cat`, and `bird`.


``` js

// Destructuring with Arrays
const animals= ['labrador', 'tabby', 'parrot'];
const [ dog, cat, bird] = animals;

console.log(dog); // ‘labrador’
```



## Destructuring Objects




```javascript
// Objects
const dog = {
    breed: “‘labrador’”,
    age: 1,
    furColor: “brown”
}

const breedOfDog= dog.breed;
const ageOfDog= dog.color;
const furColorOfDog= dog.furColor;

console.log(breedOfDog); // “labrador”

// Destructuring
const dog = {
    breed: “‘labrador’”,
    age: 1,
    furColor: “brown”
}

const { breed, age, furColor } = dog;

console.log(breed); // “labrador”
```


The order of the keys defined in the destructed Object does not matter.