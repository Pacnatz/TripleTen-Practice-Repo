## Destructuring Syntax:
#### Object Destructuring:
```javascript
const greekGods = { love: "Aphrodite", war: "Ares", trade: "Hermes" };

/* list the properties you need to get in curly braces */
const { love, war, trade } = greekGods; 

// the variable names match the object keys, so the variables 
// are assigned the corresponding values
console.log(love, war, trade); // Aphrodite Ares Hermes


// Renaming destructured variables
const {
  love: goddessOfLove,
  war: godOfWar,
  trade: godOfTrade
} = greekGods;

console.log(goddessOfLove, godOfWar, godOfTrade); // Aphrodite Ares Hermes

// Nonexistent properties
const obj = { one: 1, two: 2 };
const { one, two, three } = obj;
console.log(three); // undefined


const car = {
  model: "Volvo",
  year: "2012",
  owner: {
    name: "Elise",
    purchasedDate: "2020-09-21"
  }
}


// You don't need to destructure the whole object
// Example 1
const { name } = car.owner;
console.log(name); // Elise

// Example 2
const { name: ownerName } = car.owner; // using a different name for the variable
console.log(ownerName); // Elise


// Default values if the object doesn't specify one
const user = {
  name: "Jane Doe",
  avatar: "jane.png"
}

const { name, avatar = "placeholder-avatar.png" } = user;

console.log(avatar); // "jane.png"
```
#### Array Destructuring:
```javascript
const precipitation = ["rain", "sleet", "snow"];
const [liquid, freezing, frozen] = precipitation;

console.log(liquid, freezing, frozen); // rain sleet snow

// Skipping elements
const [, , frozen] = precipitation;
console.log(frozen); // snow
```
#### Arguments Destructuring and Default Values:
```javascript
const userData = {
  firstName: "William",
  lastName: "Webberton",
  age: 55
};

// instead of naming the parameter, we name the properties inside 
// the parameter, using destructuring syntax 
const printUserParams = ({ firstName, lastName, age = 0 }) => {
  // we can no access the properties by their names, without using
  // dot notation
  console.log(firstName);
  console.log(lastName);
  console.log(age);
};

printUserParams(userData);

/*
  William
  Webberton
  55
*/
```
###### Method and class constructor parameters:
```javascript
// code with parameter destructuring
class Card {
  // immediately access object keys
  constructor({ text, image, description }) { 
    // now, the "data" object's keys
    // are stored in variables of the same name
    this._text = text;
    this._image = image;
    this._description = description;
  }
}
```
## Interfaces in OOP - Part 2:
#### Project File Structure:
<span class="blue-text-bold">Component files</span> - Stored in components directory. (Reusable classes)
<span class="blue-text-bold">Utility module files</span> - Stored in utils directory (Functions that don't belong to a particular class and not unique to any page)
<span class="blue-text-bold">Script files</span> - Script files of a specific page go into the pages directory
![[9. File Structure|500]]
#### Creating Several Classes in a Project:
```javascript
// ./components/Section.js

class Section {
  constructor({ data }, containerSelector) {
    this._initialArray = data;
    this._container = document.querySelector(containerSelector);
  }

renderItems() {
  // Iterate over the _renderedItems array of messages
  this._initialArray.forEach((item) => {

    // Based on the isOwner field, create instances of the classes
    const card = item.isOwner ? 
        new UserCard(item, ".card-template_type_user") : new DefaultCard(item, ".card-template_type_default");

    const cardElement = card.generateCard();

    // Insert the markup on the page
    // using the setItem() method of the Section() class
    this.setItem(cardElement);
  }); 

    setItem(element) {
    this._container.append(element);
  }
}
```

```javascript
// ./pages/index.js

import Section from '../components/Section.js';
import { messageList, cardListSection } from '../utils/constants.js';

const cardList = new Section({
  data: messageList
}, cardListSection);

cardList.renderItems();
```
#### Relationships Between Classes:
<span class="blue-text-bold">Tight coupling</span> - Like a car with a gas engine. Such an engine will only run on gasoline
<span class="blue-text-bold">Loose coupling</span> - Like a hybrid engine, which can run on both electricity and gasoline. This connection is more flexible as it can work in different situations

<span class="green-text">Section class in tightly coupled</span>
```javascript
// ./components/Section.js
import DefaultCard from "./DefaultCard.js";
import UserCard from "./UserCard.js";

renderItems() {
  this._initialArray.forEach((item) => {
    const card = item.isOwner
      ? new UserCard(item, ".card-template_type_user")
      : new DefaultCard(item, ".card-template_type_default");
  
    const cardElement = card.generateCard();

    this.setItem(cardElement);
  });
}
```
<span class="red-text-bold">Section class is dependent on both UserCard and DefaultCard</span>
To make the section class independent and loosely coupled:
```javascript
// ./components/Section.js

class Section {
  constructor({ data, renderer }, containerSelector) {
	this._initialArray = data;
    this._renderer = renderer; // assign renderer to this
    
    this._container = document.querySelector(containerSelector);
  }

  renderItems() {
    this._initialArray.forEach(item => {
      this._renderer(item); // call renderer() and pass item to it
    });
  }
  
  setItem(element) {
    this._container.append(element);
  }
}

// ./pages/index.js

const cardsList = new Section({
    data: messageList,
    renderer: (cardItem) => { // notice the cardItem parameter
      const card = cardItem.isOwner
        ? new UserCard(cardItem, ".card-template_type_user")
        : new DefaultCard(cardItem, ".card-template_type_default");

      const cardElement = card.generateCard();

      cardsList.setItem(cardElement);
    }
  },
  cardListSection
);

cardsList.renderItems();

// You can make another Section to instantiating different items
```
#### Working with Event Listeners - Part 1:
```javascript
// ./components/SubmitForm.js

class SubmitForm {
	constructor({ formSelector }) {
		this._formSelector = formSelector;
	}
	
	_getTemplate() {
	    const formElement = document
	    .querySelector(this._formSelector)
	    .content
	    .querySelector(".form")
	    .cloneNode(true);
	    
	    return formElement;
  }
	_setEventListeners() {
	    // when the form is submitted
	    this._element.addEventListener("submit", (evt) => {
	    // we'll cancel the default behavior
	    evt.preventDefault();
	    // and reset its fields
	    this._element.reset();
    })
  }
	generateForm() {
		this._element = this._getTemplate(); // create an element
		this._setEventListeners(); // add listeners
		return this._element; // return the markup
	}
}
```
```javascript
// ./pages/index.js

// create a form instance
const form = new SubmitForm({
  formSelector: ".form-template",
});

// generate form markup
const formElement = form.generateForm();

// initialize a class responsible
// for adding the form to the page
const formRenderer = new Section({
    data: []  // we can pass an argument with an empty array
}, ".form-section"); // renderer property is optional we use the renderer property to distinguish between default card and user card

// add the form to the page
formRenderer.setItem(formElement);
```
#### Working with Event Listeners - Part 2:
```javascript
// ./components/SubmitForm.js
export default class SubmitForm {
  constructor({ formSelector, handleFormSubmit }) {
    this._formSelector = formSelector;
    this._handleFormSubmit = handleFormSubmit;
  }

  _getTemplate() {
  	const formElement = document
      .querySelector(this._formSelector)
      .content
      .querySelector(".form")
      .cloneNode(true);

    return formElement;
  }

  _setEventListeners() {
    this._element.addEventListener("submit", (evt) => {
      evt.preventDefault();
      this._handleFormSubmit(this._getInputValues());

      this._element.reset();
    })
  }

  _getInputValues() {
    this._inputList = this._element.querySelectorAll(".form__input");
    
    this._formValues = {};
    this._inputList.forEach(input => 
	    this._formValues[input.name] = input.value);
    
    return this._formValues;
  }

  generateForm() {
    this._element = this._getTemplate();
    this._setEventListeners();

  	return this._element;
  }
}
```
```javascript
// ./pages/index.js

const form = new SubmitForm({
  formSelector: ".form-template",
  handleFormSubmit: (formData) => {
    // we pass the formData object containing data from the form
    // to a new instance of UserCard class
    const card = new UserCard(formData,
      ".card-template_type_user");

    const cardElement = card.generateCard();

    cardsList.setItem(cardElement);
  }
});
```