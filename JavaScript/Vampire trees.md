

The method will need to add an offspring to the vampire array of its creator and set its creator:

```javascript
class Vampire {

  constructor(name, yearConverted) {
    this.name = name;
    this.yearConverted = yearConverted;
    this.offspring = [];
    this.creator = null;
  }

  

  /** Simple tree methods **/

  // Adds the vampire as an offspring of this vampire

  addOffspring(vampire) {
    this.offspring.push(vampire);
    vampire.creator = this;

  }
```


Next, we'll write a function that returns the number of offspring an vampire has.

```js
// Returns the total number of vampires created by that vampire

  get numberOfOffspring() {

    return this.offspring.length;

  }
```


## return the number of vampires in between an offspring and the creator.

```js
  
  // Returns the number of vampires away from the original vampire this vampire is

  get numberOfVampiresFromOriginal() { // this is a get method makes it a property
    let numberOfOffsprings = 0;
    let currentVampire = this; // one we are looking at right now

      // climb "up" the tree (using iteration), counting nodes, until no creator is found
      while (currentVampire.creator) { // along as its truthy, if it has a creator (parent)
        currentVampire = currentVampire.creator; // checks if current vampire is a creator, this moves one step up
        numberOfOffsprings++;//  tis jsut records the steps
        // contiues iterating until creator is null

      }
      return numberOfOffsprings;

    }
    ```


## Who is closer to the original vampire

it compares the number of steps each vampire is from the original vampire, which correctly reflects their seniority. Here, `this.numberOfVampiresFromOriginal` and `vampire.numberOfVampiresFromOriginal` are numerical values representing the depth of each vampire in the hierarchy. The vampire with the smaller number is closer to the original vampire and therefore more senior.

```js

 // Returns true if this vampire is more senior than the other vampire. (Who is closer to the original vampire)

  isMoreSeniorThan(vampire) {

    return this.numberOfVampiresFromOriginal < vampire.numberOfVampiresFromOriginal; //compares  current vampires ith another vampire , least steps

  }
```



## Returns the closest common ancestor of two vampires

``` js

 /** Stretch **/

  // Returns the closest common ancestor of two vampires.
  // The closest common anscestor should be the more senior vampire if a direct ancestor is used.

  // For example:

  // * when comparing Ansel and Sarah, Ansel is the closest common anscestor.

  // * when comparing Ansel and Andrew, Ansel is the closest common anscestor.

    // Helper method to get all ancestors of a vampire

getAncestors() {
const ancestors = [];
let currentVampire = this;
while (currentVampire) { // checks if currentvampire is truthy (if it has a creator)

ancestors.push(currentVampire); // lists of anestors
currentVampire = currentVampire.creator; // moves up the treee, stops once hit root vampire
     }
return ancestors;
    }
  // find the same ancestors here
  // need to look at one array of ancesotrs

  closestCommonAncestor(vampire) {
    const thisAncestors = this.getAncestors();
    const otherAncestors = vampire.getAncestors();

    for (const ancestor of thisAncestors) { // choose one list of ancestors
      if (otherAncestors.includes(ancestor)) { // is this ancestor in the other ancestor's array, we dont care if it is a different step, worst case is creator = null
        return ancestor; // return the common ancestor
      }
    }
    return null; // If no common ancestor is found, if ancestors are on a different tree
  }

}
```