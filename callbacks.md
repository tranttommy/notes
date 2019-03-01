# Callbacks

## Array we will be working with
```javascript
const array1 = [12, 10, 13, 11];
```
## Quick definitions
* Higher-order function: 
    * A function that takes in a function as a parameter or returns another function
* Callback function: 
    * A function that is passed into a higher-order function as an argument and called in that function
___
## Examples of functions that use callbacks that we have seen so far
* ### `array.forEach`
```javascript
1 const blah = array1.forEach(item => { console.log(item) });
2 console.log(blah);
```
This example console logs all the items in the array and returns `undefined`.
```
// Console
12                          line 1
10                          line 1
13                          line 1
11                          line 1
undefined                   line 2
```
* ### `array.filter`
```javascript
1 const filtered = array1.filter(item => item % 2 === 0);
2 console.log(filtered);
```
This example returns a new array containing only the even numbers.
```
// Console
[12, 10]                line 2
```
___
## 1. Passing functions into functions
I think that one of the things that is causing confusion is the fact that we're learning callbacks in conjunction with arrow functions, and we've gone straight into using arrow functions as our callbacks. So in this section, I'm going to make equivalent functions that will be used as callbacks in the array methods above, and they will end up with the same result in the console.
* ### `array.forEach`
```javascript
1 function logConsole(feh) {
2    console.log(feh);
3 }
4 const blah = array1.forEach(logConsole);
5 console.log(blah);
```
```
// Console
12                      line 2
10                      line 2
13                      line 2
11                      line 2
undefined               line 5
```
* ### `array.filter`
```javascript
1 function filterEven(gah) {
2    return gah % 2 === 0;
3 }
4 const filtered = array1.filter(filterEven);
5 console.log(filtered);
```
```
// Console
[12, 10]                line 5
```
Here, I created a named function that does the exact same thing as the arrow function and passed in the function name into the array method without parentheses. This is because I'm only passing the function into the array method so that it can call the function when it runs. 
___
## 2. Looking inside a function that uses callbacks
I've tried to recreate the forEach and filter array methods as higher-order functions that use callbacks in the same way. While the functions probably aren't exactly the same, they're essentially the same for how we've used them. In this context, `array1` is a global variable, so I don't need to pass it into the function.
* ### `array.forEach`
```javascript
 1 function imitateForEach(callback) {
 2     for(let i = 0; i < array1.length; i++) {
 3         callback(array1[i]);
 4     }
 5 }
 6 function logConsole(feh) {
 7     console.log(feh);
 8 }
 9 const bleh = imitateForEach(logConsole);
10 console.log(bleh);
```
Here, `imitateForEach` will pass `array1[i]` as an argument into whatever callback function it receives (line 3).
```
// Console
12                      line 7
10                      line 7
13                      line 7
11                      line 7
undefined               line 10
```
* ### `array.filter`
```javascript
 1 function imitateFilter(callback) {
 2     const newArray = [];
 3     for(let i = 0; i < array1.length; i++) {
 4         if(callback(array1[i])) {
 5             newArray.push(array1[i])
 6         }
 7     }
 8     return newArray;
 9 }
10 function filterEven(gah) {
11    return gah % 2 === 0;
12 }
13 const filtered = imitateFilter(filterEven);
14 console.log(filtered);
```
Same situation here, where `array1[i]` is passed as an argument into the callback (line 4). In this situation however, it's important that the callback that's passed should return `true` or `false` because it becomes the condition for the `if` statement.
```
// Console
[12, 10]                line 14
```
___
## 3. Knowing the function
In order to successfully use callbacks, know what arguments the higher-order function is going to pass in and know if it needs the callback to return anything. In both of the array methods that we've gone over, both loop through and pass all of their items as arguments into the callback, so when we write the callback, it has to have the parameters to be able to take in those arguments. For `array.forEach`, the callback doesn't need to return anything but for `array.filter`, the callback needs to return `true` or `false`.
___
## 4. Incorporating arrow functions and 'calling out' the function's arguments
```javascript
1 array1.forEach(pants => { console.log(pants) });
2 array1.filter(shorts => shorts % 2 === 0);
```
When writing arrow functions as callbacks, since we know what arguments the function will pass, I like to think that I'm calling out the arguments that the function will pass in. For example, with the array methods above, I know that they will pass in their items into the callback, so I start the arrow function by calling out the method's arguments (its name doesn't matter, it just has to match like in named functions) and then proceed to direct the argument to what I want to do with it.
___
## 5. When the function passes multiple arguments but callback only takes one
The callback will just use the first argument that was passed and ignore the others, just like normal functions.
```javascript
 1 function something(callback) {
 2     const a = 1;
 3     const b = 2;
 4     const c = 3;
 5     callback(b, a, c);
 6 }
 7 function logConsole(boop) {
 8     console.log(boop);
 9 }
10 something(logConsole);
```
```
// Console
2                       line 10
```
___
## 6. When the callback takes multiple arguments but the function only passes one
This is the situation that we have seen/are seeing with the filter photo gallery app. The `filterImages` function (the one that returns the filtered array) requires the `filter` object and the `images` array. The function `loadFilter` is a higher-order function takes a callback function as an argument but only passes one argument (the `filter` object) into the callback. We still need to pull the other argument (the `images` array) somehow and pass it into the `filterImages` function. 
```javascript
 1 const images = [];
 2 function loadFilter(callback) {
 3     const filter = {};
 4     callback(filter);
 5 }
 6 function filterImages(obj, arr) {
 7     // Filter-y stuff
 8     return filteredArray;
 9 }
10 loadFilter(filterImages); // doesn't work
```
From line 10, `filterImages` is passed into `loadFilter`  where it will just pass `filter` into `filterImages` at line 4. This will end up not working because `filterImages` also needs an array , `images` in this case, which was defined on line 6.

So what we have to do here is to make a function that will just take one argument (`filter`) to satify the higher-order function (`loadFilter`) but pass both arguments (`filter` and `images`) to satify the callback (`filterImages`). We can do that by either a named function or anonymous arrow function.
### via named function
```javascript
10 // loadFilter(filterImages); // doesn't work 
11 function insert(phew) {
12     filterImages(phew, images); // phew is named arbitrarily but images is not because it's the array from line 1 in the above block
13 }
14 loadFilter(insert);
```
### via arrow function
```javascript
10 // loadFilter(filterImages); //doesn't work
11 loadFilter(phew => { filterImages(phew, images) });
```
Note: This doesn't have `loadImages` involved.