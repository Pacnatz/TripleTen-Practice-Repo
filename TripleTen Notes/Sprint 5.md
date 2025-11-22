## Strings, Numbers, and Program Logic
#### let and const vs var:
<span class="red-text-bold">var can let you use variables before it's declared</span>
```javascript
console.log(x);
var x = 10;
```
<span class="red-text-bold">If you declare an array with const you can still add or remove elements but cannot redeclare the whole array</span>
```javascript
const sisters = ["Faith", "Hope"];
sisters[2] = "Charity";
console.log(sisters); // Works
sisters = ["Good", "Bad", "Ugly"]; // TypeError
```
Use const whenever possible, use let if you know the value will change

#### Strings:
###### Calculating a String's Length with length Property:
```javascript
console.log("ABCDEFGHIJKLMNOPQRSTUVWXYZ".length); // 26
```
###### Accessing Characters by Index:
```javascript
console.log("espresso"[0]); // "e"
```

#### Methods of Searching Strings for Characters:
###### Searching a String with the indexOf() method:
```javascript
"Triple Peaks".indexOf("T"); // 0
const elements = "Earth, Air, Fire, Water";
elements.indexOf("Air"); // 8
// indexOf is case-sensitive
```
#### Searching a String with the includes() method:
```javascript
"salt and pepper".indexOf("salt") !== -1; // True
"salt and pepper".indexOf("sugar") !== -1; // false
// indexOf returns -1 if not found
"Teamwork".includes("I"); // false
```
#### Searching with startsWith() and endsWith():
```javascript
"Vendetta".startsWith("V"); // True
"This is not the end".endsWith("end"); // True
```
#### Methods of Converting Strings:
###### toLowerCase() and toUpperCase():
```javascript
"Turn Caps Lock on".toLowerCase(); // turn caps lock on
"Turn Caps Lock off".toUpperCase(); // TURN CAPS LOCK OFF
// You can use these methods to check for non-case sensitive equality checks
```
###### split() method:
```javascript
"I came. I saw".split(" "); // ['I', 'came.', 'I', 'saw']
"I came. I saw".split(". "); // ['I came', 'I saw']
```
###### slice() method:
```javascript
"Believe".slice(2, 5); // "lie"
// 2nd parameter is optional
```
#### Numbers and Special Numeric Values:
###### Infinity and the Number.isFinite() method:
```javascript
25 / 0; // Infinity
-25 / 0; // -Infinity
Infinity + -Infinity; // NaN
Infinity * 0; // NaN
Infinity * -1; // -Infinity
Infinity * -Infinity; // -Infinity

Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite(1703); // true
```
###### NaN & Number.isNaN() method:
```javascript
console.log(10 * "ten"); // NaN
console.log(typeof NaN); // "number"
console.log(NaN === NaN); // false
Number.isNaN(NaN); // true
console.log(Number.isNaN(0/0)); // true
```
#### Methods for Working With Numbers:
```javascript
// rounds a number down to the nearest int
Math.floor(9.99); // 9
// rounds a number up to the nearest int
Math.ceil(9.01); // 10
// rounds a number to the nearest int
Math.round(9.51); // 10
// returns the largest of the numbers passed through it
Math.max(1, 2, 3, 4, 5); // 5
// returns the smallest of the numbers passed through it
Math.min(1, 2, 3, 4, 5); // 1
// returns a random number between 0 inclusive and 1 exclusive
Math.random(); // 0.31765465456
```
###### Extracting an Int from A String with parseInt():
```javascript
let overtime = "17 hours, 59 minutes, and 59 seconds";
console.log(parseInt(overtime)); // 17
// Will keep parsing until found a non digit character
parseInt("99 Red Balloons"); // 99
parseInt("Catch 22"); // NaN
// Parse float same thing
parseFloat("98.6"); // 98.6

const eightAndAHalf = 8.5;
Number.isInteger(eightAndAHalf); // false
Number.isInteger(Math.floor(eightAndAHalf)); // true
```
#### Logical NOT Operator:
```javascript
!(3 > 2) === 3 <= 2; // true
// false === false

if (pass !== password){
	console.log("Wrong password");
}
// Equivilant to
if (!(pass === password)){
	console.log("Wrong password");
}

// Converts string to a boolean value
!"This is not an empty string"; // False value

// Double negation
!!true; // true
!!""; // false
!!"This is not an empty string"; // true
```
#### Logical OR Operator:
```javascript
true || false || false; // true

// Evaluated from left to right once it finds a truthy value the value will be returned

let condition = 0 || NaN || "string" || false || "str";
// value of condition is "string"
```
###### Default Value:
<span class="blue-text-bold">short-circuit</span> - Second part of an AND or OR operation is not needed to determine its truth value ( That code will not be executed)
```javascript
function howDoYouDo(answer) {
	const result = answer || "fine";
	return result;
}
howDoYouDo("better than ever"); // better than ever
howDoYouDo(); // fine
```
#### Logical AND Operator:
Only returns true if all of the expressions are truthy
<span class="red-text-bold">JS will return last expression if all expressions are true</span>
<span class="red-text-bold">JS will return first false expressions if at least one of them are false</span>
```javascript
2 * 2 === 4 && 5 < 6 && "Anyone"; // Anyone
2 * 2 === 4 && undefined && "Anyone"; // Undefined
```
###### Order of Priority
<span class="red-text-bold">Logical NOT is executed first, then AND, finally OR </span>

#### The Switch-Case Statement:
```javascript
let catName;
const cartoon = "Garfield and Friends";
switch (cartoon) {
	case "Shrek 2": 
		catName = "Puss in Boots"; 
		break; 
	default: 
		catName = "Garfield"; 
}
console.log(catName); // "Garfield"
```
#### The Ternary Operator:
```javascript
/* condition */ ? /* value if true */ : /* value if false */

```
## Array Methods:
#### Merging and Joining Arrays
<span class="blue-text-bold">concat()</span> - Merge 2 or more arrays and returns  a new one
<span class="blue-text-bold">join()</span> - Concatenates elements of an array and returns a new string original array stays the same

```javascript
const classA = ["John", "Emma", "Mia", "Lucas"];
const classB = ["Oliver", "Sophia", "Ava"];
const student = "Frank"; // You can pass in single values as well
const allStudents = classA.concat(classB, student); console.log(allStudents);
// ['John', 'Emma', 'Mia', 'Lucas', 'Oliver', 'Sophia', 'Ava', 'Frank']

const theBeatles = ["John Lennon", "Paul McCartney", "Ringo Starr", "George Harrison"]; 
console.log(theBeatles.join()); // Will seperate with a comma
// "John Lennon,Paul McCartney,Ringo Starr,George Harrison" 

console.log(`Introducing the Beatles: ${theBeatles.join(", ")}`); // "Introducing the Beatles: John Lennon, Paul McCartney, Ringo Starr, George Harrison" 

// notice that the original array still looks exactly the same: 
console.log(theBeatles); 
// ["John Lennon", "Paul McCartney", "Ringo Starr", "George Harrison"]
```
#### Removing the first element with the shift() method:
<span class="blue-text-bold">shift()</span> - Removes the first element of an array and returns the removed item
<span class="green-text">If array is initially empty the method returns undefined</span>
```javascript
const italianCities = ["Pompeii", "Rome", "Naples"];
const volcanicEruption = italianCities.shift();
// the shift() method returns the removed element 
console.log(volcanicEruption); // "Pompeii" 
// now the first element is gone from the array
console.log(italianCities); // ["Rome", "Naples"]
```
<span class="blue-text-bold">unshift()</span> - Add one or more elements to the beginning of an array. Returns the number of elements it contains after adding

```javascript
const governmentLand = ["National Park Service", "Fish and Wildlife Service", "Forest Service"]; 

governmentLand.unshift("Department of Defense", "Bureau of Land Management");

console.log(governmentLand);
// ["Department of Defense", "Bureau of Land Management", "National Park Service", "Fish and Wildlife Service", "Forest Service"]
```
###### Should I add elements at beginning or end of an array?
Adding to the beginning of an array the items need to "shift" it's position
So more computation is required

#### Modifying Elements at Specific Positions:
<span class="blue-text-bold">slice()</span> - Copies part of an array and creates a new one from it
 - Index of the element from which the new array begins <span class="red-text-bold">inclusive</span>
 - Index of the element from which the new array ends <span class="red-text-bold">exclusive</span> (optional)
 - Negative numbers can be used (starts at end of array)
```javascript
const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December" ]; 

// starting from the element with an index of 2 ("March") to the element with an index of 5, but not including it ("June") const 
spring = months.slice(2, 5); 

console.log(spring);
// ["March", "April", "May"]
console.log(months); 
/* ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"] */ 
// As you can see, the original array is the same as before
```
<span class="blue-text-bold">splice()</span> - Removes elements from an array and replace them with new ones
<span class="green-text"> Removing elements is optional and you can also add items</span>
- First argument is the index to start removing elements
- Second is the number of elements to be deleted
```javascript
const week = [ "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday" ]; 

// starting from index 0, delete five elements and replace them with these five elements 
const removedItems = week.splice(0, 5, "Sunday", "Saturday", "Sunday", "Saturday", "Sunday"); 

console.log(removedItems); 
// ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"] 

console.log(week); 
// ["Sunday", "Saturday", "Sunday", "Saturday", "Sunday", "Saturday", "Sunday"]
```
#### Looping Through Arrays:
<span class="blue-text-bold">forEach()</span> - Executes a function for each element in an array
<span class="green-text">Can't use continue and break statements</span>
```javascript
const how = ["harder", "better", "faster", "stronger"];
how.forEach(function (item) { 
	console.log(item + "."); 
}); 
// pay close attention to the use of parentheses here - notice that the whole function, including the body, is passed as an argument of the forEach() method 
/* harder. better. faster. stronger. */
function withForEach() {
  people.forEach((person) => {
    console.log(person.name);
  });
}

// With callback function
function forEachWithNamedCallback() {
  people.forEach(logDetailedInfo);
}
function logDetailedInfo(personData, i) { // i is used for index
  console.log(`${i + 1}. ${personData.name} is ${personData.age} years old.`);
}

```
<span class="blue-text-bold">map()</span> - Processes each element in the original array and returns the values in a new array
<span class="green-text"> You need to use the return keyword if not it will return undefined</span>
```javascript
const firstArr = [0, 1, 2, 3, 4]; 
const secondArr = firstArr.map(function (item) { 
	 return item * item; 
	 // in this case, we're going to square each element 
}); 
console.log(secondArr); // [0, 1, 4, 9, 16]
```
###### foreach() vs map():
- Foreach is best used when you want to iterate through each element and do something with them
- Map is best used when you want to modify each element and return a new array with those changes
#### Understanding Callback Functions in Array Methods:
<span class="blue-text-bold">callback</span> - Function passed as an argument to another function. The other function executes the callback function at a later time.
``` javascript
function () { 
	console.log("You've clicked on the element"); 
}
someArray.forEach(function(element, index, array) {
	console.log(index, element, array);
});
```
#### Filtering Arrays:
<span class="blue-text-bold">filter()</span> - Takes a callback as an argument and will return true or false for each element depending on whether or not it meets your criteria
```javascript
const a = [1, 9, 2, 2, 3, 4, 1, 7, 8, 0, 9, 0, 1, 5, 3];

// let's apply a filter that will leave us with only those elements that are greater than 5
const b = a.filter(function (item) {
  return item > 5
});

console.log(b); // [9, 7, 8, 9]

array.lastIndexOf(item); // Returns the index of the item that is furthest right in the array
```
#### Checking Array Elements:
<span class="blue-text-bold">some()</span> - Checks if at least one element in the array passes the test. Method returns false if not
```javascript
const oceanResidents = ["Flounder", "Nemo", "SpongeBob", "Aquaman"]; 
const nemo = oceanResidents.some(function (resident) { 
	return resident === "Nemo"; 
});
console.log(nemo); // true
```
<span class="blue-text-bold">find()</span> - Similar to some() but returns the value of the element for which the check returned true

```javascript
const flock = [ "Sheep", "Black-and-white sheep", "Black sheep", "Blue sheep" ];

const sheep = flock.find(function (sheep) { 
return sheep.includes("sheep"); 
}); 
 
console.log(sheep); // "Black-and-white sheep"
// Stops running at the first occurance
```
<span class="blue-text-bold">every()</span> - Returns true if all elements pass the test

```javascript
const numbers = [1, 5, 8, 3 ,7]; 
const positives = numbers.every(function(num) { 
return num > 0; 
}); 
console.log(positives); // true
```
#### Reducing an Array to a Single Value:
<span class="blue-text-bold">reduce()</span> - Iterates through the elements and reduces them to one value
```javascript
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]; 
const sum = arr.reduce(function (previousValue, item) { 
	return previousValue + item; 
}); 

console.log(`sum: ${sum}`);
```
###### Initial value:
Sets the previous value on the first iteration (starting value)
```javascript
const winsAndLosses = [190, 117, -381, -394, -36, 137, -473, 372, -383]; 
const total = winsAndLosses.reduce(function (previousValue, item) { return previousValue + item; }, 1000); 
// The initial value you want to set is passed through the reduce() method as the second argument. 
console.log(total); // 149
```
<span class="red-text-bold">You need to pass an empty object as the second parameter if you want to reduce it to an object</span>

```javascript
const order = ["apple", "banana", "orange", "banana", "apple", "banana"]; 
const result = order.reduce(function (prevVal, item) { 
	if (!prevVal[item]) { 
	// if an object doesn't have a key yet, it means it wasn't         repeated before 
	prevVal[item] = 1; } else { 
	// increase the number of repetitions by 1 
	prevVal[item] += 1; } // and return the changed object 
	return prevVal; }, {} ); // The initial value is an empty object. 
	console.log(result); // { apple: 2, banana: 3, orange: 1 }
```
#### Sorting Arrays:
<span class="blue-text-bold">sort()</span> - Has an optional callback function to sort arrays
```javascript
const myNumbers = [0, 3.14, 2.718, 13]; 
myNumbers.sort(); 
myNumbers; // [0, 13, 2.718, 3.14]

const fruits = ["banana", "apple", "tomato", "orange"]; fruits.sort(); // ["apple", "banana", "orange", "tomato"]
```
###### Sorting with callbacks:
Must return a number
1. Less than 0 : First element passed through will go before the second
2. Greater than 0 : Second will go before the first
3. Equal to 0: Sort order is unchanged
```javascript
const diseases = [ "Mysophobia", "Fear of missing out", "Erythrophobia" ]; 
diseases.sort(function(a, b) { 
	/* let's convert the strings to lowercase to ensure the comparison works */ 
	a = a.toLowerCase(); 
	b = b.toLowerCase(); 
	if (a < b) return -1; // a will come before b 
	if (b < a) return 1; // b will come before a 
	return 0; 
}); 

console.log(diseases); /* ["Erythrophobia","Fear of missing out", "Mysophobia"] */
```
## Functions. Part 2:
#### Scope:
<span class="blue-text-bold">Scope</span> - determines where in the code a given identifier can be accessed.

<span class="blue-text-bold">Global scope</span> - Accessed anywhere in your code
```javascript
// messageElement is a global variable 
const messageElement = document.querySelector("#message"); function showMessage() { 
	// messageElement is accessible here 
	console.log(messageElement.textContent); 
} 
showMessage(); // messageElement is accessible here, too 
console.log(messageElement.textContent);
```
<span class="blue-text-bold">Function scope</span> - Function parameters and any variables inside it can only be accessed in that function

<span class="blue-text-bold">Block scope</span> - Section of code enclosed by a pair of curly braces { }
```javascript
if (true) { 
	const fruit = "apple"; 
	console.log(fruit); // "apple" 
} 
// ReferenceError: fruit is not defined 
console.log(fruit);
```
###### Nested scopes:
Engine searches for a variable inside the current scope. If it can't find anything it goes to surrounding scope

#### Variable Shadowing:
- When declaring a variable inside a function with the same name as one that's outside the scope it will "shadow" the variable
- Function parameters also "shadow" the variable
- Shadowing is generally discouraged because it can be hard to understand and cause subtle bugs

#### Function Expressions:
<span class="blue-text-bold">function expression</span> - Another way to define a function in Javascript
<span class="blue-text-bold"> anonymous functions</span> - Functions that don't have a name
<span class="red-text-bold">Functions are values they can be assigned to variables, passed as arguments, or returned from other functions</span>
###### Comparing function expressions and declarations:
1. A function expression can be anonymous (call back functions)
2. Function expressions cannot be used before they are defined

```javascript
const multiply = function (a, b) {
	return a * b; 
}; 
multiply(2, 3); // 6
```
#### Arrow Functions:
<span class="blue-text-bold">Arrow functions</span> - Always anonymous and have a different syntax
```javascript
const funcName = function (params) { 
// Function expression
} 

const funcName = (params) => { 
// Arrow function
}

// Implicit return statements
// These are equivalent 
const square = (x) { 
	return x * x;
} 

const square = (x) => x * x;
// Omit parentheses for a single parameter
const square = x => x * x;
```
Arrow functions are often used as callbacks

#### Default Parameters:
If you omit an argument it will be undefined
If you pass undefined through the parameter with a default value, the default value will take place
```javascript
const createGreeting = (name, greeting = `Hello, ${name}!`) => { return greeting; }; 
console.log(createGreeting("Alice")); // "Hello, Alice!" 
console.log(createGreeting("Bob", "Welcome, Bob!")); 
// "Welcome, Bob!
}
```

#### Spread and Rest Parameter Syntax:
<span class="blue-text-bold">spread syntax</span> - Allows you to "unpack elements of an array or object"
```javascript
const nums = [4, 8, 15, 16, 23, 42]; 
Math.max(nums); // NaN

// Here we "spread out" the numbers in the array
// as separate function arguments. 
Math.max(...nums); // 42 
// The above is equivalent to this 
Math.max(4, 8, 15, 16, 23, 42); // 42
```
###### Copying and Combining Arrays:
```javascript
const original = [1, 2, 3]; 
const copy = [...original]; 
console.log(copy); // [1, 2, 3]

const a = [1, 2, 3]; 
const b = [4, 5, 6]; 
const combined = [...a, ...b]; 
console.log(combined); // [1, 2, 3, 4, 5, 6]
```
###### Copying and Combining Objects:
```javascript
// If we add the new property last, it will overwrite 
// an existing property. 
const alice = { name: "Alice", age: 35, job: "Surfer" }; 
const newAlice = { ...alice, job: "Software Engineer" };
console.log(newAlice.job); // "Software Engineer" 

// If we add the new property first, an existing property 
// will overwrite it. 
const alice = { name: "Alice", age: 35, job: "Surfer" }; 
const newAlice = { job: "Software Engineer", ...alice }; console.log(newAlice.job); // "Surfer"
```
<span class="blue-text-bold">rest syntax</span> - collects multiple values into a single array
```javascript
const showTeam = (leader, ...members) => { 
	console.log(`Leader: ${leader}`); 
	console.log(`Members: ${members.join(", ")}`); 
}; 

showTeam("Alice", "Bob", "Charlie", "Diana"); 
// Leader: Alice 
// Members: Bob, Charlie, Diana
```

## Creating Elements and Adding them to the DOM:
#### insertAdjacentHTML() and insertAdjacentText():
<span class="red-text-bold">innerHTML and textContentwill delete DOM tree and reconstruct it</span>
<span class="blue-text-bold">insertAdjacentHTML()</span> - Takes a string specifying insertion point and another string containing markup
- beforeend - insert before the closing tag
- beforebegin - insert before the opening tag
- afterbegin - insert after the opening tag
- afterend - insert after the closing tag
```html
<!-- "beforebegin" --> 
<div> 
	<!-- "afterbegin" --> 
		<!-- some HTML code that is already present --> 
	<!-- "beforeend" --> 
</div> 
<!-- "afterend" -->
```
<span class="blue-text-bold">insertAdjacentText()</span> - Works similar but it only adds text like the textContent property

```html
<div class="parent"> 
	<p>New</p> 
	<div class="child"></div> 
</div>
```
```javascript
document .querySelector(".parent") .insertAdjacentHTML("afterbegin", "<p>New</p>")
```

#### createElement() and createTextNode():
<span class="blue-text-bold">document.createElement()</span> - takes a tag name string as an argument and creates the element
<span class="blue-text-bold">document.createTextNode()</span> - takes any string as an argument and creates a text node
```javascript
// createElement() takes the tag name as input 
const listItem = document.createElement("li"); 
const divElement = document.createElement("div"); 
const imageElement = document.createElement("img"); 
// createTextNode() takes the text of the node as input 
const textItem = document.createTextNode("Hello, world");
```
You can use these variables to change class list and attributes and textContent
#### Adding Elements to the Page:
1. Select parent element
2. Create elements you want to insert
3. Append them to the parent element

```javascript
const list = document.querySelector(".todo-list"); 

// An array of tasks for today 
const tasks = [ 
	"Do a project", 
	"Walk the dog", 
	"Complete a React tutorial" 
]; 

// Convert the array of tasks for today to an array of elements 
const taskElements = tasks.map(task => { 
	const listItem = document.createElement("li"); 
	listItem.textContent = task; 
	return listItem; 
}); 

// Add all elements to the DOM by "unpacking" 
// the array elements into append() 
list.append(...taskElements);
```

###### The 5 methods of adding elements:
- <span class="blue-text-bold">node.append(...args)</span> - Inserts arguments after last child of node
- <span class="blue-text-bold">node.prepend(...args)</span> - Inserts arguments before the first child of the node
- <span class="blue-text-bold">node.before(...args)</span> - Inserts its arguments before the node
- <span class="blue-text-bold">node.after(...args)</span> - Inserts its arguments after the node
- <span class="blue-text-bold">node.replaceWith(...args)</span> - Replaces the node with the specified nodes or strings

#### Cloning Elements:
<span class="blue-text-bold">cloneNode()</span> - Allows you to get a clone of an element
```javascript
// Cloning the element with all its child elements 
const deepCopy = elem.cloneNode(true); 
// Cloning the element without its child elements 
const shallowCopy = elem.cloneNode(false);
```
<span class="red-text-bold">Any event handlers that were on the node that was cloned will not transfer to the cloned node</span>

#### Template Elements:
<span class="blue-text-bold">template</span> - Allows you to create reusable markup templates
```html
<!-- This will not appear on the webpage -->
<template id="user-template"> 
	<div class="user"> 
		<img class="user__avatar" alt="avatar"> 
		<p class="user__name"></p> 
	</div> 
</template>
```
1. Select the template and access its content
2. Make clone of the template's content
3. Add content to the clone
4. Add the clone to the DOM

```javascript
// Select the container where we add our users
const usersContainer = document.querySelector('.users-container');

const userTemplate = document
  .querySelector("#user-template")
  .content
  .querySelector(".user");
const userElement = userTemplate.cloneNode(true);
const userAvatar = userElement.querySelector(".user__avatar");

userAvatar.src = "https://placeholder.png";
const userName = userElement.querySelector(".user__name");
userName.textContent = "Laura Ipsum";

// Add the clone to the DOM
usersContainer.append(userElement);
```
#### Setting Listeners on Multiple Elements:
<span class="red-text-bold">Use a forEach loop to loop through all the elements and add them on with .addEventHandler("click", function() {});</span>

#### Removing or Moving Elements:
<span class="blue-text-bold">remove()</span> - Removes an element. You can make a delete button nested inside a list item to target that specific list with a forEach loop
``` javascript
function createTodoElement(data) {
    const todoElement = document.querySelector("#todo-template");
    
    const todoDeleteBtn = todoElement
	    .querySelector(".todo-item__button");
	    
    todoDeleteBtn.addEventListener(() => {
      todoElement.remove();
    });
}
```
###### Moving elements:
```javascript
const todoList = document.querySelector(".todo-list");

// listItems is a pseudo-array of all the
// children of the todo-list element.
const todoListItems = todoList.children;

// We can move the first child to the end of the
// list by appending it to the same container. 
todoList.append(todoListItems[0]);
```
This works with apppend(), prepend(), before(), after(), and replaceWith()

#### Family Relations in the DOM:
<span class="blue-text-bold">parentElement</span> - links it to the immediate parent element
```javascript
console.log(document.body.parentElement);

// Returns the <html> element because it's 
// the parent of the <body>
```
<span class="blue-text-bold">closest()</span> - returns the first parent element matching a given selector
```html
<li class="todo">
  Text
  <div class="buttons">
      <button class="todo__delete"></button>
      <button class="todo__mark-complete"></button>
  </div>
</li>
```
```javascript
const todoItem = document
  .querySelector(".todo__delete")
  .closest(".todo"); // li.todo
```
<span class="blue-text-bold">children</span> - contains an HTMLCollection containing all its child elements
<span class="blue-text-bold">HTMLCollection</span> - A pseudo-array so it doesn't have access to all array methods
```javascript
console.log(document.body.children);
// Output: HTMLCollection(3) [p, p, p]
// This is an array
const children = [...document.body.children];
```
<span class="blue-text-bold">previousElementSibling</span> - Gets the nearest neighbor upwards
<span class="blue-text-bold">nextElementSibling</span> - Gets the nearest neighbor downwards

<span class="red-text-bold">These are all read-only properties. You can read them but not write or change them</span>

#### DOM Collections:
HTMLCollections and querySelectorAll() return pseudo-arrays. 
- Object's elements have numeric indexes
- Object has the length property
- querySelectorAll() Has forEach but not map(), reduce, filter(), or sort() HTMLCollections don't

###### Array.from() method:
```javascript
// This is a NodeList.
const posts = content.querySelectorAll(".post");

// This is an array, so we can use array methods on it.
const postsArray = Array.from(posts);
```
###### Spread syntax:
```javascript
const arr = [1, 2, 3];
const copy = [...arr];
```
<span class="red-text-bold">HTMLCollections do not have forEach method. Also they will automatically update if the DOM is updated</span>

## Managing the Git Story:
#### Git Merge Conflicts:
```bash
git status -sb # Short hand status

# main
UU index.html

git diff # Shows you all changes between files
```
```html
<!-- unchanged code  -->

<<<<<<< HEAD <!-- The current branch, in our case it's main -->
        <a href="/catalog" class="navigation__link">Catalog</a>
======= <!-- The end of changes in the current branch, the beginning of the conflicting branch -->
        <a href="/contacts" class="navigation__link">Contacts</a>
>>>>>>> bugfix/header 
<!-- The end of the part from the bugfix/header branch -->

<!-- more unchanged code -->
```
#### Merge Tools:
Visual studio will identify a file with "C" for conflicting. It will then highlight the conflicting area allowing you to resolve it.
- Accept Current Change - This will keep what you have in the destination branch and discard incoming changes
- Accept Incoming Change - This will keep the incoming changes and discard what was already in the destination branch.
- Accept Both Changes - this keeps changes from both branches
- Compare Changes - This will open a new tab to give you a side-by-side view of the conflict.

#### Reverting a Git Merge
```bash
git revert HEAD # Revert to most recent commit
git revert commitNumber # Reverts to a commit number
git revert HEAD~2 # Reverts to 2nd most recent commit

git revert -m 1 3f0d8da 
# -m specifies that it's a merge commit
# 1 specifies the parent. Restore all contents from main and revert modifications on the bugfix/header branch
```
#### A Clear Commit Message
```bash
git log --pretty=oneline # log with each commit on one line
git show 4411c8d # Shows the git commit including files changed
```
<span class="blue-text-bold">Conventional Commits</span> - A commit standard
- feat - introduces a new feature in your procture
- fix - patches a bug in your code
- style - changes the appearance of your code
- refactor - restructures your code
```bash
git commit -m "feat: add responsive for smartphones and tablets"
```
#### Git Stash:
<span class="blue-text-bold">git stash</span> - Allows you to save all your pending changes to a temp location called the stash. (Used for if you don't want to make messy / unclear commits)
```bash
git stash # Stash the uncommitted changes
git switch main # Need to stash before changing branches
```
```bash
git stash list # Shows a list of all the stashed changes
git stash pop # Retrieve the last changes in any branch and remove from the list
git stash apply stash@{2} # Retrieve the changes saved in the stash but will keep it in the list
git stash drop stash@{0} # Deletes stashed changes
git stash save "stash message" # Similar to commit messages
git stash clear # Removes stash list entirely
```
