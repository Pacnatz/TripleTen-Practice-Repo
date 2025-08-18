## Basic Data Types and Variables
#### Adding JavaScript to a Webpage
###### 2 ways to use \<script> to connect your javascript code:
1. Put the code between \<script>\</script> tags, directly in HTML
2. Write the code in a separate file and connect it to the HTML document using src attribute of the \<script> tag. <span class="green-text">We'll use this one</span>

```html
<script src="script.js"></script>
<!-- Place at the bottom of <body> just before the </body>
```
#### Displaying information with JavaScript:
<span class="blue-text-bold">Methods</span> -tell the browser to execute a pre-defined set of actions
```javascript
document.write("<p>Hello world!</p>");
console.log("See you in the console!");
```

#### Comments:
```javascript
// Single line comment
/*
This is a
multiline comment
*/
```

#### Numbers and Arithmetic Operators:
| Javascript operator | Operation      |
| ------------------- | -------------- |
| +                   | addition       |
| -                   | subtraction    |
| *                   | multiplication |
| /                   | division       |
| **                  | exponent       |
| %                   | remainder      |

#### Strings:
<span class="blue-text-bold">String</span> - Any set of characters enclosed inside of single or double quotes or backticks
Strings must start and end with the same type of quotation mark
Avoiding same quotation marks inside the string:
```javascript
console.log("The Republic of Cote d'Ivoire");
console.log('Never Say "Never" Again');
```
#### Working with Strings:
<span class="blue-text-bold">Concatenation</span> - Merging 2 strings into one string with + operator
```javascript
console.log("Java" + "Script");  // "Javascript"
```
<span class="blue-text-bold">Escape characters</span> - TO include a quote inside a string with double quotes use backslash "\"
```javascript
console.log('The Republic of Cote d\'Ivoire');
console.log("Never Say \"Never\" Again");
```
<span class="red-text-bold">Backticks behave differently than quotation marks</span>
```javascript
console.log(`I am a template literal,
I am pretty cool.
Look at me
taking up all these lines!`); // Takes up more than one line
console.log(`A stitch in time saves ${10 - 1}.`); 
// Also allows string interpolation
```

#### Primitive Data Types:
| Type      | Description                                         |
| --------- | --------------------------------------------------- |
| Number    | Represents both integers and floating-point numbers |
| String    | Represents text data                                |
| Boolean   | Represents true or false                            |
| Null      | Represents absent values                            |
| Undefined | Represents variables that aren't assigned           |
| BigInt    | Represents large integer numbers                    |
| Symbol    | Represents anonymous and unique values              |
###### typeof() operator:
```javascript
typeof(10); // Number
typeof("I want to order exotic cheeses!"); // String
typeof(true) // Boolean
typeof(undefined) // Undefined
// Returns a string
```

###### Implicit conversion:
```javascript
console.log(100 + "500"); // Converts to "100500"
console.log("10" * 2); // Converts to 20
console.log("15" - 5); // Converts to 10
console.log("10" * "2"); // Converts to 20
// Try to avoid implicit conversion as much as possible
```

###### Explicit conversion:
```javascript
String(2); // "2"
String(true); // "true"
Number("2"); // 2
Number(null); // 0
Boolean(2); // True
Boolean(0); // False
Boolean(""); // False
Boolean("any string that isn't empty"); // True
```
#### Variables:
```javascript
let username = "JazzFan1999"; // Variable declaration
const birthYear = 1999; // Const means the value cannot be changed
username = "Elise"; // Do not need to use let to reassign variables
```
###### Naming variables:
- You should name your variables with semantic meaning
- Only letters numbers dollar sign and underscore are valid characters
- Numbers cannot start the variable names and should be generally avoided
- Spaces are not allowed in variable names
- JavaScript uses camelCase convention
- Cannot use keywords such as let and const

#### Null, undefined, and NaN:
<span class="blue-text-bold">undefined</span> - When a value isn't assigned
<span class="blue-text-bold">null</span> - Intentional absence of a value <span class="green-text">typeof() returns null as an object</span> 
<span class="blue-text-bold">NaN</span> - Error with number operations

## Conditionals and Loops:
<span class="blue-text-bold">algorithms</span> - a set of steps designed to accomplish a particular coding task
#### Comparison Operators: 
| Operator | Meaning               |
| -------- | --------------------- |
| >        | greater than          |
| <        | less than             |
| >=       | greater than or equal |
| <=       | less than or equal    |
| ==       | equality              |
| !=       | inequality            |
| ===      | strict equality       |
| !===     | strict inequality     |
###### The equality operators in Javascript
```javascript
console.log("42" == 42); // Returns true
console.log("42" === 42); // Returns false
```
###### Negation with !:
```javascript
console.log(7 != 6); // true - inequality
console.log("42" !== 42); // true - strict inequality
```
#### Conditional Statements:
###### The if statement:
```javascript
let merry = false;
if (merry) {
	console.log("good");
} else {
	console.log("bad");
}
// Output: bad
```
###### Chaining conditionals with else if:
```javascript
let stockPrice = 644;
if (stoackPrice > 800) {
	console.log("Time to sell");
} else if (stockPrice > 650) {
	console.log("Hold and wait for the price to go up");
} else if (stockPrice > 500) {
	console.log("So cheap, must buy more stocks");
} else {
	console.log("All in");
}
// Output: "So cheap, must buy more stocks"
```
#### Logical Operators: 
```javascript
let isSunny = true;
let isRaining = false;
console.log(!isSunny); // false
console.log(!isRaining); // true
console.log(isSunny && isRaining); // false
console.log(isSunny || isRaining); // true
```
#### Loops and Iteration: while, for:
Runs the same code multiple times depending on if a condition remains true.
<span class="blue-text-bold">condition</span> - the true or false expression that is evaluated
<span class="blue-text-bold">body</span> - The block of code that will be run
<span class="blue-text-bold">iteration</span> - each individual execution of the loop.
```javascript
let number = 10;
while (number <= 20){
	console.log(number); // Runs 5 times
	number += 2;
}

for (let i = 0; i <= 100; i++) {
	if (i === 5) {
		continue; // Skips next section
	}
	if (i === 6) {
		break; // Breaks out of the loop
	}
	console.log(i);
}
```
## Arrays:
#### Creating Arrays and Accessing Array Elements:
<span class="blue-text-bold">array literal</span> - put elements in a [ ] separated by a comma
``` javascript
const groceries = ["chips", "lemonadee", "grapefruit", "orange", "juice", "milk", "eggs", "cookies", "banana", "cheese"];

// Accessing array elements
console.log(groceries[4]); // Juice

// Assigning to array indices
groceries[0] = "carrot";
let empty = []
empty[0] = "first";
empty[1] = "second";

// Finding the length of an array
console.log(groceries.length); // 10

// Removing last index
groceries.pop();
```
#### Iterating Over Arrays: 
```javascript
const students = ["Yasmin", "Elise", "Terry"];
for (let i = 0; i < students.length; i++) {
	console.log("Welcome to TripleTen, " + students[i] + "!");
}
```
###### Iterating over arrays with for...of:
```javascript
for (let student of students) {
	console.log("Welcome to TripleTen, " + student + "!")
}
```
#### Adding and Removing Array Elements:
###### Adding elements:
<span class="blue-text-bold">push()</span> - Adds one or more elements to the end of the array
```javascript
let students = ["Yasmin", "Elise", "Terry"];
students.push("Andy"); 
// Output 4 (Returns new length of the array)
```
###### Removing elements:
<span class="blue-text-bold">pop()</span> - Removes the last element from an array
```javascript
students.pop;
// Output Andy (Returns the popped element)
```

## Objects:
#### Creating Objects:
```javascript
let user = {
	name: "Karen",
	age: 29,
	hometown: "Boulder"
};
```
<span class="blue-text-bold">Property</span> - Consist of a key / value pair
<span class="blue-text-bold">key</span> - name, age, hometown
<span class="blue-text-bold">value</span> - Karen, 29, Boulder

###### What can be stored in an object?
Values can consist of primitives, arrays, and other objects
<span class="red-text-bold">Keys are generally restricted to strings or valid identifiers</span> 

#### Accessing Object Properties:
###### Dot notation:
```javascript
let user = {
	name: "Karen",
	age: 29,
	hometown: "Boulder"
};

console.log(user.name); // Karen
console.log(user.age); // 29
console.log(user.hometown); // Boulder
```
###### Bracket notation:
```javascript
console.log(user["name"]); // Karen
console.log(user["age"]); // 29
console.log(user["hometown"]); // Boulder
// This method is needed if the object key is not a valid javascript identifier (AKA a normal string)
```
###### Missing properties:
If an object key does not exist the value returned will be undefined.
```javascript
let obj = {
	"phone-number": "555-555-5555"
};

console.log(obj["cell-phone"]); // Undefined
```

## Functions:
<span class="blue-text-bold">Function</span> - A reusable block of code designed to perform a specific task
<span class="blue-text-bold">Dynamic Functions</span> - Can take arguments and produce different results depending on those inputs
#### Declaring and Calling Functions:
1. A name for your functions
2. Parenthesis ( )
3. Curly braces { } to hold the "body"
```javascript
function greet() {
	console.log("Hello, world!");
}
greet(); // Outputs: Hello, world!
```
<span class="red-text-bold">Write function names in camelCase</span>

#### Function Parameters:
<span class="blue-text-bold">parameter</span> - Place holder that you define within a function. <span class="green-text">Acts as variables inside the function</span>
<span class="blue-text-bold">arguments</span> - The passed in value for each parameter <span class="green-text">From a function call</span>
```javascript
function greet(name) { 
	console.log("Hello, ${name}!");
}
console.log(name); // name is not defined
greet("Alice"); // Output: Hello, Alice!
greet(); // Output: Hello, undefined!
```

#### Returning Values:
<span class="blue-text-bold">return value</span> - The value that a function sends back to the part of your program that called it
```javascript
function calculateArea(length, width) {
	return length * width;
}
const area = calculateArea(5, 7);
console.log(area); // Outputs 35
```
<span class="red-text-bold">If there's no return statement the function returns undefined</span>
<span class="red-text-bold">Return values stop the execution of a function</span>
```javascript
function isEven(n) {
	if (typeof n !== "number") {
		console.log('Invalid input: ${n} is not a number');
		return; // Function ends here
	}
	return n % 2 === 0;
}
```