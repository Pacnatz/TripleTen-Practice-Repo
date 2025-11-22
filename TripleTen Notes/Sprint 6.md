## Objects:
#### Objects Review:
<span class="blue-text-bold">Objects</span> - Variables that have properties that consist of key-value pairs
```javascript
const newCommunityCenter = {
  buildings: 3, // buildings is the property
  publicRooftopAccess: true,
  bionicGarden: true, // true is value of the property
};
console.log(newCommunityCenter.lights); // undefined
console.log(newCommunityCenter["buildings"]); // Bracket Notation
```
<span class="red-text-bold">Hyphens are invalid characters for the keys you need to wrap it in quotes: "first-key": "value";</span>
<span class="red-text-bold">Use bracket notation if the key has quotes on declaration: obj ["first-key"]</span>

#### Accessing Nested Properties:
```javascript
const birthdays = {
  amelia: {
    month: "February",
    day: 12
  },
  lana: {
    month: "July",
    day: 29
  }
};

console.log(birthdays.amelia.month);
console.log(birthdays["amelia"]["month"]);
```
#### Adding Properties:
```javascript
birthdays.amelia.year = 1999; // Adding a near year property

console.log(birthdays.amelia); 
// { month: "February", day: 12, year: 1999 }
```
#### Shorthand Property Notation:
```javascript
let personName = "Laura";

function sayHi() {
  console.log("Hello, my name is Laura");
}

const person = {
  name: personName,
  greet: sayHi
};

console.log(person.name); // "Laura" 
person.greet(); // "Hello, my name is Laura"

// If we reassign the personName variable
// the person.name property is unchanged.
personName = "Lexi";
console.log(person.name); // "Laura"
```
###### Shorthand Notation:
```javascript
const name = "Laura";

function greet() {
  console.log("Hello, my name is Laura");
}

// Since the property keys and associated
// variable names are the same we can omit
// the colon and variable name.
const person = {
  name,
  greet
};
```
#### Computed Property Keys:
```javascript
const personName = "Laura";

// Declare an empty object.
const age = {};

// Add a property. The key is the value of the 
// personName variable. The value is 30.
age[personName] = 30;

console.log(age); // { Laura: 30 }

// You can access the property in the usual ways.
console.log(age.Laura);    // 30
console.log(age["Laura"]); // 30

// You can access the property using the variable too.
console.log(age[personName]); // 30
```
<span class="green-text">You can use any JavaScript expression that returns a string value can be used as an object's key</span>

<span class="red-text-bold">You cannot add properties like age.personName = 30;</span>

#### The delete Operator:
```javascript
const obj = { one: 1 };
console.log(obj.one); // 1

// Delete the 'one' key from the 'obj' object.
delete obj.one;
console.log(obj.one); // undefined
console.log(obj); // {}
```
#### Checking if a Property Exists in an Object:
###### Using the in Operator
```javascript
const order = {
  item: 'Computer Monitor',
  quantity: 2
};

// The item and quantity properties are 
// both in the order object.
console.log('item' in order);         // true
console.log('quantity' in order);     // true

// The 'credit card number' property is not
// in the order object.
console.log('credit card number' in order); // false
```
You can just check if the property value is truthy:
```javascript
if (order["credit card number"]) {
  alert("Your purchase is being processed");
} else { 
  alert("Cannot process transaction.");
}
```
If the property has a 0 or false value it'd be better to use the in operator
<span class="blue-text-bold">Optional chaining ( ?. )</span> - Accesses an object's property or calls a function if it exists
```javascript
const user = { name: "Alice", contact: { email: "alice@example.com" } };

console.log(user?.contact?.email); // "alice@example.com"
console.log(user?.profile?.age); // undefined
```
#### Iterating Over Properties:
###### for ... in loop:
For objects
```javascript
const obj = { a: 1, b: 2}

for (let key in obj) {
    console.log(`key - ${key}`);
    console.log(`value - ${obj[key]}`);
    console.log("----");
}
```
###### for ... of loop:
For arrays
```javascript
const arr = [1, 2, 3]

for (let element of arr) {
  console.log(element);
}
```
###### Object.keys() method:
```javascript
const employee = {
  name: 'Alice',
  age: 25,
  title: 'Engineer'
};
// Returns an array of all the employee keys
const keys = Object.keys(employee);  // ["name", "age", "title"]

keys.forEach((key) => { // Now can use map() and filter() 
  console.log(`${key}: ${employee[key]}`)
});
```
###### Object.entries() and Object.values():
<span class="blue-text-bold">Object.values()</span> - Returns an array containing the object's property values
<span class="blue-text-bold">Object.entries()</span> - Returns an array containing all the object's key/value pairs
```javascript
const employee = {
  name: 'Alice',
  age: 25,
  title: 'Engineer'
};

const entries = Object.entries(employee);  
// [["name", "Alice"], ["age", 25], ["title", "Engineer"]]

entries.forEach((entry) => { 
  console.log(`${entry[0]}: ${entry[1]}`)
});
```
#### Understanding Value and Reference in JavaScript:
<span class="red-text-bold">Objects are assigned by reference</span>
```javascript
let obj1 = { a: 1 }; // obj1 is assigned an object reference.
let obj2 = obj1;     // obj2 is assigned the same reference.

// obj2.a is updated.
obj2.a = "apple";

// Both object properties have been changed.
console.log(obj2.a); // "apple"
console.log(obj1.a); // "apple"


```
Obj2 is stored by reference on the same memory slot as obj1
###### Creating a new object:
```javascript
const firstObj = {
  one: 1,
  two: 2
};

let secondObj = firstObj; // Keyword of let allows a new object
secondObj = { one: 1, two: 2 };

console.log(firstObj);  // { one: 1, two: 2 }
console.log(secondObj); // { one: 1, two: 2 }
```
#### Comparing Objects:
Javascript compares the reference in memory. Not by value
```javascript
// time and money reference the same object
const time = {};
const money = time;

console.log(time === money); 
// Output: true. 

// firstObj and secondObj reference different objects
const firstObj = { hello: "world" };
const secondObj = { hello: "world" };

console.log(firstObj === secondObj);
// Output: false
```
#### Copying Objects. Shallow Copy:
<span class="blue-text-bold">Shallow copy</span> - A new object that shares the same properties and stored in a new memory location. Nested objects and arrays are not copied. Those properties will store references to the original object / array.
<span class="blue-text-bold">Deep copy</span> - Nested objects and arrays are also copied to the new object

###### Object.assign():
<span class="green-text">Used to make shallow copies</span> 
```javascript
const obj = { a: 1, b: 2 };
const copiedObj = Object.assign({}, obj);
```
## Event Handling:
#### Keyboard Events:
<span class="green-text">Keyboard events can only be generated in input and textarea elements </span>

```javascript
const input = document.querySelector(".text-field");  

input.addEventListener("keydown", function () {
  console.log("Whenever you press a key, I'm logged");
});

// You can also put the eventListener on the entire document
document.addEventListener("keydown", function () { console.log("No matter what key you press, I'm logged"); });
```
<span class="blue-text-bold">keydown</span> - Event is fired when the user presses a key
<span class="green-text">keydown will continue firing as long as a key is held down</span>
<span class="blue-text-bold">keyup</span> - Event is fired when the user releases a key

#### Properties of the Keyboard Event Object:
<span class="green-text">You can use evt.key to get the key of the input</span>
```html
<input id="input"> 
<span id="error" style="display: none">Only alphabetic characters allowed</span>
```
```javascript
const input = document.querySelector("#input");

// The error block is hidden by default
const error = document.querySelector("#error"); 

input.addEventListener("keydown", function (evt) {
  // Check if a digit has been input
    if (Number.isNaN(Number(evt.key))) {
    // If the user enters anything but a digit, the error block will be displayed
    error.style.display = "block";
    };
});
```

#### Properties of the Mouse Event Object:
<span class="blue-text-bold">mouseover</span> - Fires when the cursor is moved over an element to which we've added an event listener
<span class="blue-text-bold">mouseout</span> - fires when the mouse is moved out of an element or any of its children.
<span class="blue-text-bold">mousedown</span> - fires once any mouse button is pressed
<span class="blue-text-bold">mouseup</span> - fires once any mouse button is released
<span class="blue-text-bold">click</span> - fires once the left mouse button is pressed and released on the same element
<span class="blue-text-bold">contextmenu</span> - fires once the right mouse button is pressed
<span class="blue-text-bold">dblclick</span> - fires once user double clicks the left mouse button

#### Removing an Event Listener: 
<span class="blue-text-bold">removeEventListener()</span> - Removes an event listener
```javascript
function showmessage()   {
  console.log("We've declared the function beforehand, we'll use it later");
}

someElement.addEventListener("click", showmessage);
someElement.removeEventListener("click", showmessage);

// however many times you click the element, the console will remain silent

someElement.addEventListener("click", function () {
  console.log("Event fired!");
});

someElement.removeEventListener("click", function () {
    console.log("Event fired!");
});

// The message is still logged to the console upon clicking because the function references are not the same
```
#### Preventing the Browser's Default Behavior:
###### Default Browser Behavior:
- Whenever the user clicks on a link, the browser navigates to the appropriate URL, and the page refreshes.
- When the user presses a key while entering text in the text field, the browser adds the corresponding character for that key into that text field.
- When the user submits a form, the browser refreshes the page.

```javascript
evt.preventDefault();
```
#### Event Bubbling and Delegation:
The more event handlers you have, the more risk of memory leaks and performance problems
###### Event Bubbling:
When an event is triggered on an element, the event handler is fired first on the initial element, then on its parent element and then continues to fire all the way back up the hierarchy to the window object
###### Event Delegation:
We can use the target property to reference the element that the event fired on
```javascript
playlist.addEventListener("click", function (evt) {
  // If the user pressed the like button, add a like
  if (evt.target.classList.contains("song__like")) {
    like(evt.target);
  // If the user pressed the delete button, delete the song
  } else if (evt.target.classList.contains("song__delete-btn")) {
    remove(evt.target.closest(".song");
  }
});
```
###### evt.currentTarget property:
<span class="blue-text-bold">evt.currentTarget</span> - References the element the event listener was set on

#### Preventing Bubbling:
<span class="blue-text-bold">stopPropagation()</span> - Stops bubbling on the element
<span class="blue-text-bold">stopImmediatePropagation()</span> - Prevents event bubbling and AND prevents all subsequently defined handlers of the same event from firing on that element
```javascript
// stopPropagation()
const parent = document.querySelector("#parent");
const firstChild = document.querySelector("#firstChild");
const secondChild = document.querySelector("#secondChild");
const thirdChild = document.querySelector("#thirdChild");

function callback(evt) {
  evt.stopPropagation();

  // the event has fired on the element
  console.log(evt.currentTarget.getAttribute("id"));
  // Only logs thirdChild if clicked on third child
  // Logs secondChild if clicked on second child
}

parent.addEventListener("click", callback);
firstChild.addEventListener("click", callback);
secondChild.addEventListener("click", callback);
thirdChild.addEventListener("click", callback);
```
```javascript
// stopImmediatePropagation()
const credit = document.querySelector("#credit");

credit.addEventListener("click", function(event) {
  console.log("Nah, I'll do it later");
});

credit.addEventListener("click", function(event) {
  console.log("I WILL do it tomorrow");
  event.stopImmediatePropagation();
});

credit.addEventListener("click", function(event) {
  console.log("Why do I have so much work piled up?!");
});

/* Clicking on the credit element will print to the console:

  Nah, I'll do it later
  I WILL do it tomorrow

*///Third event will not fire
```
#### HTML Event Attributes:
<span class="green-text">You can add listeners directly in the tag of an HTML element</span>
```html
<body>
  <div class="container">
    <button onclick="doSomething()">
  </div>
  <script>
    function doSomething() {
      console.log("You've clicked the button!");
    }
  </script>
</body>
```
You should still use addEventListener():
1. With this method, you can attach several handlers for the same event to an element.
2. It's generally better to keep your markup and scripts in separate files (i.e., to have a separation of concerns).
3. Not all events can be defined using HTML attributes.

## Working with Forms:
#### Accessing Forms using JavaScript:
```javascript
document.forms[0]; // The first form
document.forms[1]; // The second form
```
###### The name attribute:
<span class="blue-text-bold">name</span> - Attach this to your form element to access via the forms property
```html
<!-- index.html -->

<!-- The name of this form is "form1" -->
<form name="form1">
  <h2>Form for entering the word "Google"</h2>
</form>

<!-- The name of this form is "form2" -->
<form name="form2">
  <h2>Form about the universe</h2>
</form>
```
```javascript
document.forms.form1; // the first form
document.forms.form2; // the second form
```
#### The Submit Event:
```html
<button type="submit">Submit</button>
<button type="reset">Reset form data</button>
<button type="button">Do something</button>
```
###### The default behavior of the submit event:
The submit event causes the browser to refresh
evt.preventDefault() will prevent browser refresh

###### Sending Data to the Server:
<span class="blue-text-bold">action attribute</span> - Can be applied to any form. Used to specify the URL to which our form data should be sent
<span class="green-text">Forms are rarely submitted this way</span>
```html
<form name="form" action="https://myserverdomain.com/form-handler">
  <input type="text">
  <button type="submit">Submit</button>
</form>
```
Use this instead:
1. Default form submission is interrupted
2. Form is validated using JavaScript
3. Then sends data off to the server
###### The Submit Button:
<span class="red-text-bold">If you don't specify the type attribute it will default to a submit button</span>
```html
<!-- These two buttons are identical inside the form: --> 
<form>
  <button>Submit</button>
  <button type="submit">Submit</button>
</form>
```
#### Accessing Form Elements:
Form elements are stored in a pseudo-array in the elements property
```javascript
// elements of the first form
document.forms[0]elements;

// elements of the form with name="myForm"
document.forms.myForm.elements;
```
<span class="green-text">You can use name attributes for form elements to access them in the pseudo array</span>
```javascript
const searchForm = document.forms.searchForm;
const answerForm = document.forms.answerForm;

searchForm.elements.google; // <input type="text" name="google" ...
answerForm.elements.answer; // <input type="number" name="answer" ...
answerForm.elements.earth;  // <input type="text" name="earth" ...
```
<span class="red-text-bold">If the name attribute isn't a valid javascript variable name you need to use bracket notation</span>
```javascript
const searchForm = document.forms["search-form"];
```
#### Getting the Value of Form Elements:
###### Text Fields:
```javascript
  console.log(input.value); // the value of input
  console.log(textArea.value); // the value of textArea
```
###### Checkbox and Radio Button Values:
```javascript
  console.log(radio.checked); // true or false
  console.log(checkbox.checked); // true or false
```
###### Radio Button Groups:
<span class="green-text">When giving radio the same name attribute only one radio button can be checked at a given time</span>
```html
<form name="myForm">
  <input type="radio" name="myRadio" value="good">
  <input type="radio" name="myRadio" value="awesome">
  <button class="button">Submit</button>
</form>
```
```javascript
console.log(radio.checked); // now it's undefined!
console.log(radio.value); // here goes the value of the selected radio button
```
###### Select and Option Field Value:
<span class="green-text">The select element's value is the value of the currently chosen option element</span>
```html
<form name="myForm">
  <select name="mySelect">
	<!-- Default value -->
    <option value="right">To the right</option>
    <option value="left">To the left</option>
    <option value="forward">Straight ahead</option>
  </select>
  <button class="button">Submit</button>
</form>
```
```javascript
console.log(select.value); // the console will log what was selected
```
#### The change and input Events:
<span class="blue-text-bold">input event</span> - Fires continuously after each change to text / select input
<span class="blue-text-bold">change event</span> - Fires only after change is finalized (such as deselecting input field)
```javascript
textInput.addEventListener("input", callback); textInput.addEventListener("change", callback);
```
#### reset() and submit() methods:
###### Resetting all form fields:
```javascript
const form = document.forms.myForm;

form.addEventListener("submit", function (evt) {
	// processing form ...
	evt.preventDefault();
	// resetting form fields
	form.reset();
});
```
###### Programmatic Form Submission:
```javascript
const form = document.forms.myForm;
const input = form.elements.myInput;

form.addEventListener("input", function (evt) {
  // if four characters are entered
  // the submit event will be generated 
  if (input.length === 4) {
    form.submit();
  }
});

form.addEventListener("submit", function (evt) {
  // processing of the submit event
});
```
## Form Validation:
#### Browser Built-In Form Validation:
<span class="green-text">Type validation provides a form of validation already i.e. url, email, checkbox</span>
Other types of validation attributes:
- type
- required
- minlength
- maxlength
- pattern (study more with node.js)
#### Styling Invalid Form Fields:
###### Styling Input FIelds Using Pseudo-Classes:
- :valid - indicates the input's current value is valid
- :invalid - indicates that input's current value is invalid
- :checked - Applies styles to currently checkedboxes (type="checkbox") and (type="radio")
<span class="red-text-bold">An empty field with required attribute has the :invalid pseudo-class by default</span>

#### JavaScript Form Validation:
Browser form validation has a few issues:
- Can't style an error message
- If multiple fields are invalid the error message will only appear for the first field
- Live validation won't display error messages
- Can't validate one field based on the contents of another (Password and Confirm Password)
JavaScript fixes these:
- Form fields can be instantly validated while typing. See error messages before clicking submit
- Customize your errors by changing styles, text, and way it's displayed
###### novalidate Attribute:
Disables HTML standard error messages. Applied on the form element
```html
<form class="form" novalidate>
  <input class="form__input" type="url" placeholder="URL" required>
  <button class="form__submit">Sign in</button>
</form>
```
###### ValidityState Object:
<span class="blue-text-bold">ValidityState</span> - Each HTML field has its own ValidityState object stored on a property called validity
document.forms\[0]\[0].validity:
```javascript
validity: ValidityState
	valueMissing:true // ture if field is empty
	typeMisMatch: false // true if input value is not correct type
	tooLong: false // exceeds maxLength
	tooShort: false
	rangeUnderflow: false
	rangeOverflow: false
	stepMismatch: false
	badInput: false
	customError: false
	valid: false // final decision
	__proto__: ValidityState
```
###### Instant Form Validation:
```javascript
formInput.addEventListener("input", function (evt) {
  // Log the values of the validity.valid property belonging to the input field, 
  // on which we're listening for the input event, to the console
  console.log(evt.target.validity.valid);
});
```
#### Connecting Javascript Validation Methods to DOM:
###### Changing Field Styles when an Error Occurs:
```html
<form class="form" novalidate>
  <label class="form__field">
    Enter email adress
    <input class="form__input" type="email" placeholder="Email" required>
  </label>
  <button class="form__submit">Sign in</button>
</form>
```
```css
.form__input_type_error {
  border-bottom-color: red;

  /* The bottom border will become red every time
  the input data is invalid */
}
```
```javascript
// First, select all the needed form elements, and assign them to variables
const formElement = document.querySelector(".form");
const formInput = formElement.querySelector(".form__input");

// Code the 1st function, which shows the error element
const showInputError = (element) => {
  element.classList.add("form__input_type_error");
};

// Code the 2nd function, which hides the error element
const hideInputError = (element) => {
  element.classList.remove("form__input_type_error");
};

// Code the 3rd function, which checks if the field is valid
const checkInputValidity = () => {
  if (!formInput.validity.valid) {
    // If NOT (!), show the error element
    showInputError(formInput);
  } else {
    // If it's valid, hide the error element
    hideInputError(formInput);
  }
};
 
formElement.addEventListener("submit", function (evt) {
  // Cancel the browser default action, so that clicking on the submit button won't refresh the page
  evt.preventDefault();
});

// Call the checkInputValidity() function for each character input
formInput.addEventListener("input", checkInputValidity);
```
###### Adding an Error Message with span:
```html
<form class="form" novalidate>
  <label class="form__field">
    Enter email adress
    <input id="email-input" class="form__input" type="email" placeholder="Email" required>
    <span class="form__input-error">This field is required.</span>
  </label>
  <button class="form__submit">Sign in</button>
</form>
```
```javascript
const formInput = formElement.querySelector(".form__input");
console.log(formInput.id); // "email-input"

// Select each error message element using its unique class name
const formError = formElement.querySelector(`.${formInput.id}-error`);

const showInputError = (element) => {
  element.classList.add("form__input_type_error");
  
  // Apply a class to show the error element. 
  formError.classList.add("form__input-error_active");
};

const hideInputError = (element) => {
  element.classList.remove("form__input_type_error");
  
  
  // Apply a class to hide the error element.
  formError.classList.remove("form__input-error_active");
};
```
```css
/* Initially, input error elements will be invisible. */
.form__input-error {
  color: red;
  opacity: 0;
}

/* Applying the modifier class will make it visible. */
.form__input-error_active {
  opacity: 1;
}
```
###### Changing the Error Message:
```javascript
// The error message will be the function's second parameter
const showInputError = (element, errorMessage) => {
  element.classList.add("form__input_type_error");
  // Replace the content of the error message with the passed errorMessage argument
  formError.textContent = errorMessage;
  formError.classList.add("form__input-error_active");
};

const hideInputError = (element) => {
  element.classList.remove("form__input_type_error");
  formError.classList.remove("form__input-error_active");
  // Reset the error
  formError.textContent = "";
};

const checkInputValidity = () => {
  if (!formInput.validity.valid) {
    // The error message itself is the function's second parameter
    showInputError(formInput, formInput.validationMessage);
  } else {
    hideInputError(formInput);
  }
};
```
#### Validation of Several Fields and Forms:
```javascript
const showInputError = (formElement, inputElement, errorMessage) => {
  // Find the error message element inside the very function
  const errorElement = formElement.querySelector(`.${inputElement.id}-error`);
  // The rest remains unchanged
  inputElement.classList.add("form__input_type_error");
  errorElement.textContent = errorMessage;
  errorElement.classList.add("form__input-error_active");
};

const hideInputError = (formElement, inputElement) => {
  // Find the error message element
  const errorElement = formElement.querySelector(`.${inputElement.id}-error`);
  // The rest remains unchanged
  inputElement.classList.remove("form__input_type_error");
  errorElement.classList.remove("form__input-error_active");
  errorElement.textContent = "";
}


// Change checkInputValidity() so that it has formElement and inputElement
// parameters instead of taking corresponding variables from the outer scope

const checkInputValidity = (formElement, inputElement) => {
  if (!inputElement.validity.valid) {
    // The parameter of showInputError() is now a form,
    // which contains a field to be checked
    showInputError(formElement, inputElement, inputElement.validationMessage);
  } else {
    // The same for hideInputError(), Its parameter is now a form,
    // which contains a field to be checked
    hideInputError(formElement, inputElement);
  }
};
```
Set event handlers to all field forms:
```javascript
const setEventListeners = (formElement) => {
  // Find all fields inside the form, and
  // make an array from them using the Array.from() method
  const inputList = Array.from(formElement.querySelectorAll(".form__input"));

  // Iterate over the resulting array
  inputList.forEach((inputElement) => {
    // add the input event handler to each field
    inputElement.addEventListener("input", () => {
      // Call the checkInputValidity() function inside the callback,
      // and pass the form and the element to be checked to it
      checkInputValidity(formElement, inputElement)
    });
  });
};
```
Set event handlers to all forms:
```javascript
const enableValidation = () => {
  // It will find all forms with the specified class in DOM, and
  // make an array from them using the Array.from() method
  const formList = Array.from(document.querySelectorAll(".form"));

  // Iterate over the resulting array
  formList.forEach((formElement) => {
    formElement.addEventListener("submit", (evt) => {
      // Cancel default behavior for each form
      evt.preventDefault();
    });

    // Call the setEventListeners() function for each form,
    // taking a form element as an argument
    setEventListeners(formElement);
  });
};

// Call the function
enableValidation();
```
#### Interaction with Other DOM Elements:
checkInputValidity() only validates the input element
hasInvalidInput() will take an array of form fields and return true if one is invalid
```javascript
// The function takes an array of fields

const hasInvalidInput = (inputList) => {
  // iterate over the array using the some() method
  return inputList.some((inputElement) => {
        // If the field is invalid, the callback will return true.
    // The method will then stop, and hasInvalidInput() function will return true
    // hasInvalidInput returns true

    return !inputElement.validity.valid;
  })
};


// The function takes an array of input fields
// and the button element, whose state you need to change

const toggleButtonState = (inputList, buttonElement) => {
  // If there is at least one invalid input
  if (hasInvalidInput(inputList)) {
    // make the button inactive
    buttonElement.classList.add("form__submit_inactive");
  } else {
        // otherwise, make it active
    buttonElement.classList.remove("form__submit_inactive");
  }
};
```
```javascript
const setEventListeners = (formElement) => {
  // Find all the form fields and make an array of them
  const inputList = Array.from(formElement.querySelectorAll(".form__input"));
  // Find the submit button in the current form
  const buttonElement = formElement.querySelector(".form__submit");

  // Call the toggleButtonState() before we start listening to the input event 
  toggleButtonState(inputList, buttonElement);

  inputList.forEach((inputElement) => {
    inputElement.addEventListener("input", () => {
      checkInputValidity(formElement, inputElement);
      // Call the toggleButtonState() and pass an 
      // array of fields and the button to it
      toggleButtonState(inputList, buttonElement);
    });
  });
};
```
