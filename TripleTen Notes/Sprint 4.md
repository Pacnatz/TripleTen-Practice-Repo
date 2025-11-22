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

## Basics of the DOM:
<span class="blue-text-bold">DOM</span> - Document object model
<span class="blue-text-bold">DOM tree</span> - Browser translates all HTML content into a format Javascript can understand
<span class="red-text-bold">Elements on devtools actually show the DOM tree this includes elements added by Javascript</span>

#### Selecting HTML Elements :
```html
<!-- Adding scripts to the DOM -->
<!-- After all DOM elements -->
<body>
	<!-- elements -->
	<script src="path/to/script"></script>
</body>
<!-- Before DOM elements -->
<head>
	<script defer src="path/to/script"></script>
</head>
<body>
	<!-- elements -->
</body>
```
###### Accessing the first element that matches a CSS selector:
```html
<main id="container"> 
	<div class="content"> 
		<div class="content__item"></div>
		<div class="content__item"></div>
		<div class="content__item"></div>
	</div> 
</main>
```
```javascript
const containerElement = document.querySelector("#container");
const contentElement = containerElement.querySelector(".content");
// contentItem is the first matching element
const contentItem = contentElement.querySelector(".content__item");
// contentItems is a collection of all matching elements
const contentItems = contentElement.querySelectorAll(".content__item");
```
###### Using more complicated selectors:
```html
<section class="bag">
	<div class="item">
		<h3>Glasses case</h3>
		<p>Glasses</p>
	</div>
	<p class="item">Brush</p>
	<p class="item">Pocket mirror</p>
	<div class="item bag">
		<h3>Makeup Bag</h3>
		<p class="item">Lipstick</p>
		<p class="item">Mascara</p>
	</div> 
	<p class="item wallet">Wallet</p>
</section>
```
```javascript
const makeupBagContent = document.querySelectorAll("section.bag div.bag .item"); console.log(makeupBagContent); 
// Outputs a NodeList containing the elements with the text "Lipstick" and "Mascara"
```
<span class="green-text">querySelector() - reutrns null if nothing found</span>
<span class="green-text">querySelectorAll() - returns an empty NodeList if nothing found</span>

#### Browser Events
<span class="blue-text-bold">Event</span> - something that happens on a webpage

- "click" - triggered when you click on an element
- "mouseover" - triggered when the cursor hovers over an element
- "mouseout" - triggered when the cursor hover ends and it moves away from an element
- "scoll" - triggered when the user scrolls an element
- "submit" - triggered when the user clicks a submit \<button> element or presses Enter while editing an input field in a form

###### Listening for and reacting to events:
```javascript
element.addEventListener(eventName, handler);
// Handler is the function that runs when the event is triggered

const element = document.querySelector(".my-element");

function showClick() {
  console.log("You have clicked on the element");
}

element.addEventListener("click", showClick); 

element.addEventListener("click", function () {
  console.log("You have clicked on the element");
});
```
<span class="blue-text-bold">callback function</span> - A function that is passed into another function so that it can be invoked later
<span class="blue-text-bold">Anonymous function</span> - A function that is declared inside the parameter

###### Listener vs handler:
- Listener - Listens for an event to occur (i.e. the thing that the addEventListener() creates)
- Handler - A function that is called when the event occurs (i.e. the function passed to addEventListener())

#### Setting Attributes:
- Set value of an attribute with setAttribute()
- Remove an attribute with removeAttribute()

###### setAttribute() method:
```javascript
const button = document.querySelector(".form__button");
button.setAttribute("type", "submit");
button.setAttribute("style", "background-color: #000");
// Boolean attributes
button.setAttribute("disabled", true);
button.setAttribute("disabled", false);
// Remove attributes
button.removeAttribute("disabled");
```
###### Attributes as object properties:
```html
<a class="menu__link" href="https://tripleten.com/">TripleTen</a>
```
```javascript
const link = document.querySelector(".menu__link");
console.log(link.href);
link.href = "https://linkedin.com";
```
#### Manipulating Classes:
<span class="blue-text-bold">classList</span> - A property that conveniently modifies the class attributes
```html
<h1 class="article__title">Article Title</h1>
```
```javascript
const title = document.querySelector(".article__title");
console.log(title.classList); 
// Outputs ['article__title']
```
###### Adding a class name with the add() method:
```javascript
title.classList.add("article__title_theme_dark");
console.log(title.classList);
// ['article__title', 'article__title_theme_dark']
```
###### Removing a class with the remove() method:
```javascript
title.classList.remove("article__title_theme_dark");
console.log(title.classList);
// ['article__title']
```
###### Checking whether a class is present with contains() method: 
```javascript
console.log(title.classList.contains("article__title_theme_dark")); // False
```
###### Toggling a class with the toggle() method:
```javascript
title.classList.toggle("article__title_theme_dark");
// Adds if it's not there, removes if it's there
```
#### Form Submission Events:
<span class="blue-text-bold">Action attribute</span> - Rare but now we handle form submissions with JS and DOM events
###### Basic form submission without JavaScript:
1. The browser sends the form data to the URL specified in the \<form> tag's action attribute
2. The page is reloaded.
```html
<form id="myForm" action="/submit" method="POST">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" required>
  <button type="submit">Submit</button>
</form>
```
###### Handling form submission with JavaScript:
```html
<form id="myForm">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" required>
  <button type="submit">Submit</button>
</form>
```
```javascript
const myForm = document.querySelector("#myForm");

myForm.addEventListener("submit", function (evt) {
  evt.preventDefault(); // Prevent the page from reloading when the form is submitted
  // Handle the submission event
}) 
```
<span class="blue-text-bold">event object</span> - A special JavaScript object containing information about the event that just occurred.

#### Form Field Values:
###### The value property:
```html
<form class="form" id="myForm">
  <label for="name">Name:</label>
  <input class="form__input" type="text" id="name-input" name="name" required>
  <button type="submit">Submit</button>
</form>
```

```javascript
const myForm = document.querySelector("#myForm");
const nameInput = myForm.querySelector(".form__input");
// Selecting the input for the form
myForm.addEventListener("submit", function(evt) {
	evt.preventDefault();
	console.log(nameInput.value);
	// Resetting the form
	myForm.reset();
})
```
#### innerHTML and textContent:
```html
<p class="modal__text">
  Hi, {name}! Your reservation for {count} has been made for {date}. A
  confirmation email has been sent to {email}.
</p>
```
To access or change this text node we can use it's parent element's textContent property.
###### Acccessing text content:
<span class="blue-text-bold">textContent</span> - Allows us to access and change the text content of an element
```javascript
const title = document.querySelector(".article__title");
console.log(title.textContent); // Outputs the textContent
title.textContent = "Are NFTs the new way to collect art?";
console.log(title.textContent);
// Output: Are NFTs the new way to collect art?
```
<span class="red-text-bold">You should not modify the textContent if that element has children. It will override everything</span>
###### Accessing and updating HTML content
<span class="blue-text-bold">innerHTML</span> - A string containing all of an element's HTML content (including tags)
```javascript
document.body.innerHTML = "<h1 class='heading'>World of Web 3.0</h1>"
```
#### The Event Object's target Property:
###### The event target:
```javascript
myForm.addEventListener("submit", function (evt) {
	evt.preventDefault();
	evt.target.reset();
	// Stores a reference to the element the event fired on
	// This will always reset the form that was submitted
})
```
evt.target refers to the element that intiated the event

## Debugging
#### How to Read Errors:
![[8. Error Message | 300]]
1. Error Type - Reference errors indicates non-existent variable
2. Error message - Tells you exactly what happened
3. Call stack - List of functions that were called leading up to the error
4. Error location: File name and line location (2 numbers :42:12) indicates line number and characters in the line

#### Error Types:
<span class="blue-text-bold">Syntax Error</span> - Something wrong with the syntax
<span class="blue-text-bold">Reference error</span> - Undeclared variable or function
<span class="blue-text-bold">Type error</span> - When you try to use a variable name as a function

#### The Debugger:
###### The debugger statement:
```javascript
function logCharacters(str) {
	debuggger; // Will pause the code here
for (let index = 0; index < str.length; index++) {
		console.log(str[index]);
	}
}
logCharacters('SEGAAAAAA');
```
###### Breakpoints:
Into the sources tab on DevTools you can right click on a line of code and set a breakpoint

## Expert Git:
#### Branches: 
<span class="blue-text-bold">Branches</span> - Isolated versions of your code that won't be seen elsewhere
```bash
git branch #Shows a list of branches
git branch feature/header #Creates a new branch
git checkout feature/header #Switch to a cerrtain branch

git switch -c branch_name # Creates a new branch and switch to it

git switch main
git checkout main # same commands
git merge feature/header #Merging feature/header onto main

git branch -D feature/header #Deletes a branch
```
###### Git switch and git restore vs git checkout
- <span class="blue-text-bold">git switch</span> - for changing branches
- <span class="blue-text-bold">git restore filepath</span> - For restoring the content of files to match the last commit and overwriting any uncommitted changes
- <span class="blue-text-bold">git checkout</span> - Does both git switch and git restore

<span class="red-text-bold">If a branch implements a new feature start the branch with the word feature.</span>

<span class="red-text-bold">If the branch is to fix a bug start the branch with bugfix</span>

