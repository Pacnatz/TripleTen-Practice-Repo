## Javascript Classes:
#### Javascript Classes:
<span class="blue-text-bold">classes</span> - Used to create multiple objects with the same properties and methods
Classes can help you:
- Organize code
- Avoid repetition
- Create consistent objects
- Encapsulate data
- Build complex applications
###### Creating objects with classes:
```javascript
class Warrior {
  constructor(name, strength) {
    this.name = name;
    this.health = 100;
    this.strength = strength;
  }

  attack() {
    return `${this.name} deals ${this.strength} damage!`;
  }
}

const warrior1 = new Warrior("Thorin", 15);
const warrior2 = new Warrior("Aragorn", 12);
const warrior3 = new Warrior("Boromir", 14);

warrior3.attack();
// Boromir deals 14 damage!
```
#### Class Features:
<span class="blue-text-bold">Instantiation</span> - Creating a new object from a class using the new keyword
<span class="blue-text-bold">this</span> - Refers to the object that the method is being called on.
<span class="blue-text-bold">Methods</span> - Functions defined on an object (They do not have function keyword)
<span class="blue-text-bold">Public fields</span> - Properties that are accessible outside the class
<span class="blue-text-bold">Private fields</span> - Properties that are only accessible inside the class (Use \_)
<span class="red-text-bold">JS does not enforce public/private distinction</span>

You need to use <span class="red-text-bold">this</span> to call methods inside the class
```javascript
class Book {
  constructor(title, author) {
    if (!this._validateBookData(title, author)) {
      throw new Error("Title and author must be non-empty strings");
    }
    this.title = title;
    this.author = author;
  }

  _validateBookData(title, author) {
    return (
      typeof title === "string" &&
      typeof author === "string" &&
      title.length > 0 &&
      author.length > 0
    );
  }
}
```
#### Inheritance:
<span class="blue-text-bold">Inheritance</span> - Allows classes to share functionality by creating parent-child relationships
###### Inheritance in the DOM:
- Node is a bass class for all DOM objects
- HTMLElement inherits all properties and methods from Node and adds HTML-specific features
- Specific elements like HTMLInputElement inherit from HTMLElement and add their own specific features
###### A button element inherits:
- Basic DOM methods from Node (appendChild())
- HTML features from HTMLElement (textContent)
- Button-specific features from HTMLButtonElement (disabled property)

###### Creating an abstract Character class:
```javascript
class Character {
  constructor(name, health) {
    this.name = name;
    this.health = health;
  }

  takeDamage(damage) {
    this.health -= damage;
    return `${this.name} takes ${damage} damage! Remaining health: ${this.health}.`;
  }
}

class Mage extends Character {
  constructor(name, health, mana) {
    super(name, health); // super() calls the parent constructor
    this.mana = mana;
  }

  castSpell(spellName, target) {
    // We are hard-coding the mana cost and spell damage for simplicity
    if (this.mana >= 8) {
      this.mana -= 8;
      target.takeDamage(15);
      return `${this.name} casts ${spellName} at ${target.name}! Remaining mana ${this.mana}.`;
    }
    return "Not enough mana!";
  }
}

const gandalf = new Mage("Gandalf", 40, 100);
const merlin = new Mage("Merlin", 40, 100);
console.log(gandalf.castSpell("Fireball", merlin));
// Output:
// "Gandalf casts Fireball at Merlin! Remaining mana: 92."
// "Merlin takes 15 damage! Remaining health: 25"
```
###### Overriding existing methods:
<span class="blue-text-bold">override</span> - Replacing the parent's version with our own custom implementation
```javascript
class Warrior extends Character {
  constructor(name, health, armor) {
    super(name, health);
    this.armor = armor;
  }

  attack(target) {
    const damage = 10;
    console.log(`${this.name} attacks ${target.name} for ${damage} damage!`);
    target.takeDamage(damage);
  }

  // Override the takeDamage method to account for armor
  takeDamage(damage) {
    // Subtract armor from damage received, with a minimum of 1
    const actualDamage = Math.max(1, damage - this.armor);
    this.health -= actualDamage;
    console.log(`${this.name} takes ${actualDamage} damage (${damage - actualDamage} blocked by armor). Health remaining: ${this.health}`);
  }
}

// Let's test this:
const newbie = new Warrior("Newbie", 50, 0);
const knight = new Warrior("Sir Lancelot", 100, 8);

knight.attack(newbie);
// Sir Lancelot attacks Newbie!
// Newbie takes 10 damage (0 blocked by armor). Health remaining: 40.

newbie.attack(knight);
// Newbie attacks Sir Lancelot!
// Sir Lancelot takes 2 damage (8 blocked by armor). Health remaining: 98.
```
## Object-Oriented Programming:
#### Objects in OOP:
<span class="blue-text-bold">state</span> - State of an object includes its variables and properties aka fields
<span class="blue-text-bold">behavior</span> - Behavior of an object is defined via methods

#### A Quick Dip into "this":
```javascript
function like() {
  this.isLiked = !this.isLiked;
}

function createSong(title, artist) {
  const newSong = {
    title,
    artist,
    isLiked: false,
    like: like
  }

  return newSong;
}

const song1 = createSong("Chanel", "Frank Ocean");

song1.like(); // we'll call the like() function on song1

console.log(song1.isLiked); // true — it worked!
```
<span class="red-text-bold">If the function is called as an object method. "this" inside that function will reference the object</span>

#### Classes:
<span class="red-text-bold">All instances share the same method in memory</span>
```javascript
song1.like === song2.like; // true for classes
```
#### Encapsulation:
<span class="blue-text-bold">User Interface</span> - Referring to the appearance of the program
<span class="green-text">Private methods are intended for the internal workings of an object</span>
###### Real Private Methods and Properties:
<span class="green-text"># actually makes the properties private However many browsers don't support this new syntax with #</span>

###### Emulation of Private Methods and Properties:
<span class="green-text">Use _ to emulate private properties</span>
```javascript
class Car {
  constructor(maxGasTankValue, fuelConsumption) {
    this._gasTankValue = 0;
    this._maxGasTankValue = maxGasTankValue;
    this._fuelConsumption = fuelConsumption; // miles per gallon
  }

  _getAvailableGasValue(gasValue) {
    if (gasValue < 0) return 0;
    if (gasValue > this._maxGasTankValue) return this._maxGasTankValue;
    return gasValue;
  }
    
  refuel(gasValue) {
    this._gasTankValue = this._getAvailableGasValue(gasValue);
  }
    
  getDistance() {
    return this._gasTankValue / this._fuelConsumption * 100;
  }
}

const car = new Car(70, 9);
car.refuel(45);

console.log(car._gasTankValue); // 45. The property isn't actually private and it's easy to change it.
console.log(car.getDistance()); // 500
```
#### Inheritance:
<span class="blue-text-bold">static values</span> - Values we want to stay the same regardless of what the user passes onto the constructor
```javascript
class Student {
  constructor(name, group) {
    this._name = name;
    this._group = group;
    this._profession = null;
    this._trainingDuration = null;
  }

  getInfo() {
    return {
      name: this._name,
      group: this._group,
      profession: this._profession,
      trainingDuration: this._trainingDuration
    }
  }
}

class WebDeveloperStudent extends Student {
  constructor(name, group) {
    super(name, group);
    this._profession = "Web developer";
    this._trainingDuration = 10;
  }
} 

class PythonDeveloperStudent extends Student {
  constructor(name, group) {
    super(name, group);
    this._profession = "Python developer";
    this._trainingDuration = 9;
  }
}

class DataAnalystStudent extends Student {
  constructor(name, group) {
    super(name, group);
    this._profession = "Data analyst";
    this._trainingDuration = 6;
  }
}

:

const student1 = new WebDeveloperStudent("Wendy Webberton", 1);
const student2 = new DataAnalystStudent("Dennis Databerg", 3);

student1.getInfo();

/*
  {
    name: "Wendy Webberton",
    group: 1,
    profession: "Web developer",
    traningDuration: 10
  }
*/

student2.getInfo();

/*
  {
    name: "Dennis Databerg",
    group: 3,
    profession: "Data analyst",
    traningDuration: 6
  }
*/
```
#### Polymorphism: 
Working with an array of students:
```javascript
function getInfoList(students) {
  if (!Array.isArray(students)) return [];
  return students.map(student => student.getInfo());
}

getInfoList([student1, student2, student3]);
```

```javascript
class WebDeveloperStudent extends Student {
  constructor(name, group) {
    // ...
  }
  
  getInfo() {
    const info = super.getInfo(); // Calls student's getInfo
    // Returns an object of the students info
    info.language = "Javascript"; // Modify the language
    return info;
  }
}
```
## Interfaces in OOP:
#### Working with a Markup Template inside a Class:
<span class="blue-text-bold">Static Elements</span> - Stays the same across multiple pages
<span class="blue-text-bold">Dynamic Elements</span> - Can change without the page being refreshed
###### Chat Example:
```html
<template class="card-template">
  <div class="card">
    <img src="" alt="Userpic" class="card__avatar">
    <div class="card__text">
      <p class="card__paragraph"><!-- the text of the message will appear here --></p>
    </div>
  </div>
</template>
```
```javascript
class Card {
    constructor(text, image) {
        this._text = text;
        this._image = image;
    }
	
	_getTemplate() {
	  // taking the markup from HTML and cloning the element
	    const cardElement = document
	    .querySelector(".card-template")
	    .content
	    .querySelector(".card")
	    .cloneNode(true);
	    
	  // return the DOM element of the card
	    return cardElement;
	}
    } 
}

const card = new Card("Hi! How are you?", "userpic.jpg"); 
```
#### Adding Data to Markup and Inserting into the DOM:
Add a public method to return card instances to the interface
```javascript
generateCard() {
  // Store the markup in the private field _element
  // so that other elements can access it
  this._element = this._getTemplate();

    // Add data
    this._element.querySelector(".card__avatar").src = this._image;
    this._element.querySelector(".card__paragraph").textContent = this._text;

  // Return the element
    return this._element;
}
```

```javascript
// beginning of the index.js file

const messageList = [
    {
        image: "https://practicum-content.s3.us-west-1.amazonaws.com/web-code/moved_card__image.jpg",
        text: "Hi, we need to tune up our chat ASAP!"
    },
    {
        image: "https://practicum-content.s3.us-west-1.amazonaws.com/web-code/moved_card__image-lake.jpg",
        text: "Now we can create as many cards as we need!"
    },
];

class Card {
  // code of the class
}

messageList.forEach((item) => {
  // Create a card instance
    const card = new Card(item.text, item.image);
  // Fill up the card and return it
    const cardElement = card.generateCard();

  // Add it to the DOM
  document.body.append(cardElement);
});
```
#### Scaling a Javascript Class:
###### Passing an Object to the constructor() Method:
```javascript
class Card {
    constructor(data) { // now the constructor gets an object
        this._text = data.text;
        this._image = data.image;
    }

  // the rest of the Card class's methods
}

// iterate over the entire original array
messageList.forEach((item) => {
    const card = new Card(item); // pass an object as an argument
    const cardElement = card.generateCard();
    document.body.append(cardElement);
});
```
###### Using a class with different template elements:
```javascript
class Card {
  constructor(data, cardSelector) { // added second parameter
    this._text = data.text;
    this._image = data.image;
    this._cardSelector = cardSelector; // assign the selector to the private field
  }

  _getTemplate() {
    const cardElement = document
      .querySelector(this._cardSelector) // use this._cardSelector here
      .content
      .cloneNode(true);

    return cardElement;
  }
  
}

messageList.forEach((item) => {
  // pass the template selector on creating
    const card = new Card(item, ".card-template_type_default");
    const cardElement = card.generateCard();

    document.body.append(cardElement);
});
```
#### Event Handlers:
```javascript
class Card {
  constructor(data, cardSelector) {
    this._text = data.text;
    this._image = data.image;
    this._cardSelector = cardSelector;
  }

  _getTemplate() {
    const cardElement = document
      .querySelector(this._cardSelector)
      .content
      .querySelector(".card")
      .cloneNode(true);

    return cardElement;
  }

  generateCard() {
    this._element = this._getTemplate();
    this._setEventListeners();

    this._element.querySelector(".card__avatar").src = this._image;
    this._element.querySelector(".card__paragraph").textContent = this._text;

    return this._element;
  }

  _setEventListeners() {
    this._element.querySelector(".card__text").addEventListener("click", () => {
      this._handleMessageClick();
    });
  }

  _handleMessageClick() {
    this._element.querySelector(".card__text").classList.toggle("card__text_is-active");
  }
}

messageList.forEach((item) => {
  const card = new Card(item, ".card-template_type_default");
  const cardElement = card.generateCard();

  document.body.append(cardElement);
});
```
#### Applied Inheritance:
```javascript
class Card {
  constructor(cardSelector) { // now there is just one parameter — selector
    this._cardSelector = cardSelector;
  }

  // all the class methods go next
}

class UserCard extends Card {
  constructor(data, cardSelector) {
    // the super keyword calls the constructor of the parent
    // class with a single argument which is template selector
    super(cardSelector);

    // user card has text only
    this._text = data.text;
  }


  generateCard() {
    this._element = super._getTemplate(); 
    // this replaced with super
    super._setEventListeners(); 
    // this replaced with super
    this._element.querySelector(".card__paragraph").textContent = this._text;

    return this._element;
  }

}

class DefaultCard extends Card {
  constructor(data, cardSelector) {
    // call the constructor of the parent the same way
    super(cardSelector);

    // the person on the other end now has both an avatar and a text
    this._text = data.text;
    this._image = data.image;
  }

  // generateCard() goes next
}

messageList.forEach((item) => {
  // If the isOwner value === true,
  // a UserCard instance is created,
  // otherwise DefaultCard is created

  const card = item.isOwner
    ? new UserCard(item, ".card-template_type_user")
    : new DefaultCard(item, ".card-template_type_default");

  const cardElement = card.generateCard();

  document.body.append(cardElement);
});
```
#### Polymorphism:
```javascript
class Card {
  // the constructor and other methods

  _handleMessageClick() {
    this._element.querySelector(".card__text").classList.toggle("card__text_is-active");
  }
}

class UserCard extends Card {
  // the constructor and other methods

  _handleMessageClick() {
    super._handleMessageClick(); // calling a parent method

    // adding new functionality to _handleMessageClick:
    // this._element stores a card element
    // let's add the card_is-active class to it
    this._element.classList.toggle("card_is-active");
  }
}
```
<span class="red-text-bold">FINAL CODE</span>
```javascript
const messageList = [
	{
		image: "https://code.s3.yandex.net/web-code/card__image.jpg",
		text: "Hi, we need to tune up our chat ASAP!"
	},
	{
		text: "Here is the user's chat card",
    isOwner: true
	},
	{
		image: "https://code.s3.yandex.net/web-code/card__image.jpg",
		text: "The response!"
	},
];

class Card {
	constructor(cardSelector) {
    this._cardSelector = cardSelector;
	}

  _getTemplate() {
  	const cardElement = document
      .querySelector(this._cardSelector)
      .content
      .querySelector(".card")
      .cloneNode(true);

    this._element = cardElement;
  }

	_setEventListeners() {
		this._element.querySelector(".card__text").addEventListener("click", () => {
			this._handleMessageClick();
		});
	}

  _handleMessageClick() {
    this._element.querySelector(".card__text").classList.toggle("card__text_is-active");
  }
}

class UserCard extends Card {
	constructor(data, cardSelector) {
    super(cardSelector);
		this._text = data.text;
	}
 
  generateCard() {
    super._getTemplate();
    super._setEventListeners();

  	this._element.querySelector(".card__paragraph").textContent = this._text;

  	return this._element;
  }

  _handleMessageClick() {
    super._handleMessageClick();
     
    this._element.classList.toggle("card_is-active");
  }
};

class DefaultCard extends Card {
	constructor(data, cardSelector) {
    super(cardSelector);
		this._text = data.text;
		this._image = data.image;
	}

  generateCard() {
    super._getTemplate();
    super._setEventListeners();

    this._element.querySelector(".card__avatar").src = this._image;
  	this._element.querySelector(".card__paragraph").textContent = this._text;

  	return this._element;
  }
}

messageList.forEach((item) => {
  const card = item.isOwner
    ? new UserCard(item, ".card-template_type_user")
    : new DefaultCard(item, ".card-template_type_default");

	const cardElement = card.generateCard();

	document.body.append(cardElement);
});
```
## Modular Javascript:
#### IIFE in Detail:
<span class="blue-text-bold">IIFE</span> - Immediately Invoked Function Expressions
```javascript
(function () {
	let test = "Hello world!"; // local
	console.log(test);
})(); // let's add some parentheses at end of the code to call the function

// These are all local variables
(function () { 
	const button = document.querySelector("button"); 
	function handleClick(evt) { 
	// the code of the handler for the click event 
	} 
	button.addEventListener("click", handleClick); 
})();
```
#### Encapsulation and Modules:
<span class="blue-text-bold">Encapsulation</span> - Programming principle to keep related code together and hiding the internal details
<span class="blue-text-bold">Modules</span> - Self-contained pieces of code that:
- Keep their internal variables private
- Only expose the function that other code needs to use
- Don't pollute the global scope with unnecessary variables
<span class="green-text">One way to create a module is to use an IIFE</span>
```javascript
// Basic IIFE structure
const myModule = (function() {
  // private variables - only accessible inside this function
  const privateVariable = "I'm hidden!";

  // return an object with public methods
  return {
    publicMethod: function() {
      console.log("This method can be called from outside");
    }
  };
})(); // The () at the end immediately runs this function

// Usage:
myModule.publicMethod(); // This works
console.log(myModule.privateVariable); 
// This is undefined - the variable is private!
```
#### What are Modules?
A module is simply a file with some code that implements a particular functionality
###### Loading and using modules:
```html
<script type="module" src="script-01.js"></script>
<script type="module" src="script-02.js"></script>
```
###### Module scope:
Variables and functions declared inside a module don't pollute the global namespace

###### Exporting from a module:
```javascript
// script-01.js

export const str = "I am a variable from the module script-01.js";

export function myFunc() {
  console.log("I am a function from the module script-01.js");
}
```
###### Importing to a module:
```javascript
// script-02.js

// importing the function and variable by using their names
import { str, myFunc } from "./script-01.js";

// now you can use them

console.log(str); // "I am a variable from the module script-01.js"
myFunc(); // "I am a function from the module script-01.js"
```
#### Export and Import statements:
1. We use **default** exports when we want to export/import a single value.
2. We use **named** exports when we want to export/import multiple values.
###### Named exports and imports:
``` javascript
const array = [1, 2, 3, 4];
const arrSquared = arr.map(item => item * item);

// export a list
export { array, arrSquared };

//-----------
// data.js

const array = [1, 2, 3, 4];
const arrSquared = arr.map(item => item * item);

// rename on export
export { array as arr, arrSquared as sq };

// everything that has been exported from the data.js file will be imported 

console.log(data.array); // [1, 2, 3]
console.log(data.arrSquared); // [1, 4, 9]

//---------

// index.js

// use new names in the import statement
import { arr, sq } from "./data.js";
```
###### Named import statements:
```javascript
// index.js

// to destructure the exports, list them separated
// by a comma inside curly brackets
import { array, arrSquared } from "./data.js"

console.log(array); // [1, 2, 3]
console.log(arrSquared); // [1, 4, 9]


//--------------- NOT RECOMMENDED
// index.js
import * as data from "./data.js";

//------

// index.js
import { array as arr, arrSquared as sq } from "./data.js"

console.log(arr); // [1, 2, 3]
console.log(sq); // [1, 4, 9]

```
###### Default export and import
```javascript
// Let's imagine that the different export functions below
// are all in different files

// render-items.js
export default function render() {
  // ...
}

// song.js
export default class {
  constructor() { }
}

// data.js
export default [12, 22, 31];
```

When we import a default export, we don’t need to destructure it:

```javascript
// index.js

// We can give the import any name we choose
import renderItems from "./render-items.js";

renderItems();
```

**Export syntax:**

- `export const array = [1, 2, 3]` — export on declaration. It can be used multiple times in a file.
- `export { dog, cat }` — export multiple entities separate from their declaration.
- `export default data` — export a single variable, function, or class.

**Import syntax:**

- `import { myArray, myFunc } from "./data.js"` — import using the named export’s original name.
- `import * as myImports` — import all exports in an object called `myImports`.
- `import { array as arr } from "./data.js"` — rename the value on import.
- `import data from "./data.js"` — import of a default export (no curly braces needed).
#### Browser-Specific Features of Modules:
- Old browsers don't understand type="module" will ignore the script completely
- Add a fallback
```html
<!-- Modern browsers: load this module, ignore the nomodule script -->
<script type="module" src="modern-script.js"></script>

<!-- Older browsers: ignore the module, load this regular script -->
<script nomodule src="legacy-script.js"></script>
```
1. **Older browsers don’t understand modules** - But we typically don’t need to worry about this, since build-tools can automatically correct the issue
2. **Modules are deferred by default** - They don't block HTML parsing and wait for the DOM to be ready
3. **Modules load in parallel** - Multiple modules can download simultaneously while the page loads
4. **Modules execute in order** - Even though they load in parallel, they still execute in the order they appear in your HTML