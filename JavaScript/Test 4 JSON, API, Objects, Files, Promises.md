

## Question 02
[[Api Fetching]]

Implement a function fetchDataForUser, which fetches data from a remote JSON api and then returns a part of it.

Since this is a network call, it will need to be an asynchronous function and return the data via a callback.

The JSON-based data will be fetched from this URL, and others like it:

https://gist.githubusercontent.com/kvirani/f7d65576cc1331da1c98d5cad4f82a69/raw/4baad7566f0b6cd6f651c5d3558a015e226428b5/data.json


The callback should be called with two arguments:

1. error: if request comes back with an err, pass it through to this callback. otherwise set this to null

2. data: if there is no error, this should be the object representing the wins and losses for the given username. If there is an error, this should be set to null.

  
Use the request library (https://www.npmjs.com/package/request) to fetch data.

The request library is already installed in this project, and you can require and use it.

### Notes

Data
https://gist.githubusercontent.com/kvirani/f7d65576cc1331da1c98d5cad4f82a69/raw/4baad7566f0b6cd6f651c5d3558a015e226428b5/data.json

```json
{
  "users": {
    "mr_robot": {
      "wins": 5,
      "losses": 2
    },
    "teddy_b": {
      "wins": 0,
      "losses": 3
    }
  }
}
```

1. Use the request library to fetch the data from the given url

```js 
request(url, (error, response, body) => {
```


2. If there is error in fetching the data, call the callback with the error object and data set to null

```js
if (error) { callback(error, null); return; }
```

3. If we successfully retrieve the data, we should call the callback with the wins and losses for the given username i.e. object["teddy_b"]

```js
const bodyObject = JSON.parse(body); 
const userObject = bodyObject.users[username]; 
callback(null, userObject);
```


### The Whole Code

``` js
/* 
1. Use the request library to fetch the data from the given url
2. If there is error in fetching the data, call the callback with the error object and data set to null
3. If we successfully retrieve the data, we should call the callback with the wins and losses for the given username i.e. object["teddy_b"]
*/

const request = require('request');

const fetchDataForUser = function(url, username, callback) {
    // IMPLEMENT ME

    request(url, (error, response, body) => {
        if (error) {
            callback(error, null);
            return;
        }

        const bodyObject = JSON.parse(body);
        const userObject = bodyObject.users[username];

        callback(null, userObject);
    });
}

fetchDataForUser('https://gist.githubusercontent.com/kvirani/fd5d76657c0133d41c1f5a9cdb58e476/raw/7aaa466b5cfd66f5155f5d38e0622485/data.json', 'mr_robot', (err, data) => {
    console.log("here is your data");
});

```

My code

``` js
const request = require('request');

const fetchDataForUser = function(url, username, callback) {

  request(url, (error, response, body) =>{

if (error) {
      callback(error, null);
      return;
    }
    
try {
      const data = JSON.parse(body);
      if (data.username === 0) {
        callback(error, undefined);
        return;
      }
      if (data && typeof data.users === 'object' && data.users[username] !== undefined) {
        const userData = data.users[username];
        callback(null, userData);
      } else {
        callback(null, undefined);
      }
         } catch (error) {
      callback("error", null);
      return;
    }
  });
 };
```

## Question 03
[[filePath]]
  

Write a function which takes in two file paths and can sum the numbers found in each file.

For example,
  
Given:
- filePath1 points to a txt file which contains "42"

- filePath2 points to a txt file which contains "24"

Then:
- call the callback with the number 66

The callback should be called with two arguments (an error object, followed by data) as is typical in Node.

The data passed in should therefore be the second argument, not the first.

The callback should be called with two arguments:

1. error: if there is an fs error, pass it through to this callback. otherwise set this argument to null

2. data: if there is no error, this should be the sum (number). If there is an error, this should be set to null.

- The function should support negative and decimal point numbers.

- Don't worry about other edge cases. For example, you can assume that if the given files are there,

they WILL contain valid numbers.

### Notes 

- We have to declare a function that takes in 3 arguments, filePath1, filePath2 and a callback (err, data)

``` js

const sumFileData = function(filePath1, filePath2, callback) 

```

- We need to read the first file, if there is error we need to invoke the callback with error that we received and data to null

```js

fs.readFile(filePath1, "utf8", (error, data) => {
        if (error) {
            callback(error, null);
            return;
        }
```

- We got the first number (may be store it in variable)

```js
     const firstNumber = parseFloat(data);
```

- We need to read the second file, if there is error we need to invoke the callback with error that we received and data to null

```js
  fs.readFile(filePath2, "utf8", (error, data) => {
            if (error) {
                callback(error, null);
                return;
            }
```

- We need to add the first number + second number

```js
      const secondNumber = parseFloat(data);
	const result = firstNumber + secondNumber;
```

- And call the callback with result in this case we will set the error to null and data to the result

```js
callback(null, result);
```

### The Whole Code

```js
const fs = require('fs');

const sumFileData = function(filePath1, filePath2, callback) {
    fs.readFile(filePath1, "utf8", (error, data) => {
        if (error) {
            callback(error, null);
            return;
        }

        const firstNumber = parseFloat(data);

        fs.readFile(filePath2, "utf8", (error, data) => {
            if (error) {
                callback(error, null);
                return;
            }

            const secondNumber = parseFloat(data);
            const result = firstNumber + secondNumber;
            callback(null, result);
        });
    });
};

```




## Question 4
[[Promises]] [[Callbacks]]
Write a function which creates and returns a promise. Its job will be similar to that of Question 01:
> Run a given (callback) function after a delay.

However:
- if the given callback returns a falsy value, the promise should fail (reject) the string "Falsy value retrieved" should be sent through to the reject function

- if the given callback returns a truthy value, the promise should pass (resolve)

  the return value of the executed callback should be sent through to the resolve function
### Notes

1. Create and return a promise  

```js
  return new Promise((resolve, reject) => {
```

2.  We need to invoke the callback with the data (run a code) after a set delay 
3. And store result in a variable 
```js
const result = callback(data);
```

4. If the result of the callback is falsy, we should reject the promise with string "Falsy value retrieved
5. If the result of the callback is truthy, we should resolve the promise with the result of the callback i.e. we need to pass the result to the resolve function 

``` js
if (result) {
        resolve(result);
      } else {
        reject("Falsy value retrieved");
      }
```

### The Whole Code

``` js
const doShortlyExpectingTruthy = function (callback, delay = 3000, data) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const result = callback(data);
      if (result) {
        resolve(result);
      } else {
        reject("Falsy value retrieved");
      }
    }, delay);
  });
};

// something in the first second!

console.log(result);

// Don't change below:
module.exports = { doShortlyExpectingTruthy };

```


