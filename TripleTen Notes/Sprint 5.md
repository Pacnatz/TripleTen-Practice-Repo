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
const firstArr = [0, 1, 2, 3, 4]; const secondArr = firstArr.map(function (item) { 
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
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]; const sum = arr.reduce(function (previousValue, item) { 
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