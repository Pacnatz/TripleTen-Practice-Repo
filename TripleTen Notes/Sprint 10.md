## Introducing React:
#### Web applications and frameworks:
<span class="blue-text-bold">AJAX</span> - Asynchronous Javascript and XML allows access to the server without refreshing
<span class="blue-text-bold">Imperative approach</span> - Describe the step-by-step process of what the program should do
#### Components:
![[13. React components.png|650]]
<span class="green-text">Majority of applications using React have a main component called "App" which contain additional components nested inside one another. Making the App component the trunk of the tree</span>
#### The Declarative Approach:
<span class="blue-text-bold">Declarative Programming</span> - allows making an interface responsive to user actions, not by making direct changes to the DOM, but instead changing variables responsible for the markup.
```javascript
const element = document.querySelector('#myElement');
let isClicked = false;

element.addEventListener('click', () => {
  isClicked = true; // Allows us to shorten to 1 line
});
```
```javascript
// This example is NOT valid HTML. But you'll be
// able to do something very similar in React.

<div id="myElement">Click me!</div>

<div id="myAnotherElement" className={isClicked ? 'active' : ''}>
  <div id="myText">
    {isClicked ? 'It was clicked!' : 'Waiting for click...'}
  </div>
</div>
```
#### React and JSX:
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8" />
  <title>React and JSX</title>
  <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>

<body>
  <div id="root">
    <!-- Remove the <h1> tag here -->
  </div>
  <!-- Add JSX code-->
  <script type="text/jsx" src="script.jsx">
    ReactDOM.render(
      <h1>Hello, world!</h1>,
      document.querySelector('#root'),
    );
  </script>
</body>

</html>
```
1st script link is the primary react library
2nd script link is the library for working with the DOM
3rd script link is the library for babel
```javascript
// the variable responsible for the app's state
let isClicked = false;

// this function is responsible for rendering the interface
// dependent on the value of isClicked
function renderAll() {
  ReactDOM.render((
    <div>
      <button type="button">Click me!</button>

      <div className={isClicked ? 'active' : ''}>
        <div>
          {isClicked ? 'It was clicked!' : 'Waiting for click...'}
        </div>
      </div>
    </div>
  ), document.querySelector('#root'));
}

// call to perform initial render when page is opened
renderAll();

// when the event occurs, we change the variables value
// then render the page again
const element = document.querySelector('button');
element.addEventListener('click', () => {
  isClicked = true;
  renderAll(); 
});
```
<span class="blue-text-bold">JSX</span> - The HTML code in the ReactDOM.render()
<span class="red-text-bold">Use className instead of class to apply CSS styles</span>
#### JSX: Basic Syntax:
###### Conditional logic:
```javascript
ReactDOM.render((
  <div>
    {isDaylight ? (
      <h2>Good morning!</h2>
    ) : (
      <h2>Good evening!</h2>
    )}
  </div>
), document.querySelector('#root'));
```
```javascript
ReactDOM.render((
  <div>
    {isLunchTime && <h2>Time for lunch!</h2>}
  </div>
), document.querySelector('#root'));
```
###### Styles:
```javascript
const cssRules = {
    // Written in camelCase and no px (use "" for %)
    width: 6792,
    height: 6752,
    borderRadius: '50%',
    background: '#934838',
    color: 'black',
};

ReactDOM.render((
    <div style={cssRules}>What planet am I?</div>
), document.querySelector('#root'));
```
###### Fragments:
```javascript
ReactDOM.render((
  <> // Or React.Fragment works
    <button type="submit">Click me!</button>
    <div>Was it clicked?</div>
    <div>Yes, it was clicked!</div>
    <div>It was clicked!</div>
  </>
), document.querySelector('#root'));
```
<span class="red-text-bold">All elements need a closing tag. Or use <.img />
</span>
#### JSX: Lists and Events:
<span class="blue-text-bold">List</span> - Any data item of the same type repeated more than once (menu, user list, image gallery)
```javascript
const comments = [{ 
  id: 1,
  author: 'Lisa',
  text: 'What is love?',
}, { 
  id: 2,
  author: 'James',
  text: 'Does anyone know where my sandwich is?',
}, { 
  id: 3,
  author: 'Greg',
  text: 'I\'m selling a moped.',
}];

ReactDOM.render((
  <div>
    <h2>Messages</h2>

    {comments.map(message => (
      // This is an important attribute: `key`
      <div key={message.id}>
        <h3>{message.author}</h3>
        <div>{message.text}</div>
      </div>
    ))}
  </div>
), document.querySelector('#root'));
```
<span class="red-text-bold">React requires us to set keys when working with lists</span>
###### Handling events:
```javascript
function handleClick() {
  console.log('Don\'t click me!');
}

function handleMouseEnter() {
  console.log('Hey, you\'re in my zone!');
}

function handleMouseLeave() {
  console.log('...where\'d you go?');
}

ReactDOM.render((
  <button
    onClick={handleClick}
    onMouseEnter={handleMouseEnter}
    onMouseLeave={handleMouseLeave}
  >
    Play with me!
  </button>
), document.querySelector('#root'));
```

<span class="red-text-bold">Parenthesis is to returning multi-line JSX
</span>
<span class="red-text-bold">Curley braces for embedding JavaScript inside JSX</span>
```jsx
list.map(item, i) => {
	return <li key={i}>{item.name}</li>;
	// Need to return if using braces
});
list.map(item, i) => (
	<li key={i}>{item.name}</li>
	// No need to return
));
```
#### Functional Components and Props:
```javascript
// a component named User
function User(props) { // User must be capitalized
  return (
    <div>
      <img src={`https://practicum-content.s3.us-west-1.amazonaws.com/web-code/react/${props.id}.png} width="75" alt="user picture" />
      <p>{props.name}</p>
    </div>
  );
}
// the main app code
ReactDOM.render((
  <>
    <h2>My imaginary friends:</h2>
    <User id="1" name="Gregory" />
    <User id="2" name="James" />
    <User id="3" name="Allison" />
  </>
), document.querySelector('#root'));
```
<span class="green-text">props are just objects whose keys are the property names we pass to our JSX components</span>
```javascript
function WrapperComponent(props) {
  console.log(props.children);
  return (
    <div>
      <h1>Hello Children!</h1>
      {props.children} // Shows children of this component
    </div>
  );
}
```
```jsx
/* the rest of the JSX */

<WrapperComponent>
 <!-- The children -->
  <div>
    <span>Here's some content!</span>
  </div>
</WrapperComponent>

/* the rest of the JSX */
```
```javascript
// Another example of props.children
function ConfirmationDialog(props) {
  return (
    <div className="dialog">
      <div className="dialog__body">{props.children}</div>
      <button onClick={props.onConfirm}>Confirm</button>
      <button onClick={props.onCancel}>Cancel</button>
    </div>
  );
}

export default function App() {
  return (
    <ConfirmationDialog
      onConfirm={() => alert("Order confirmed!")}
      onCancel={() => alert("Order cancelled!")}
    >
      Do you really want to place this order? {/* ← This is props.children */}
    </ConfirmationDialog>
  );
}
```
#### Class Components:
<span class="red-text-bold">Class components are still supported but not recommended for new projects unless maintaining older code</span>
```javascript
// functional component User
function User(props) {
  return (
    <div>
      <img src={`https://code.s3.yandex.net/web-code/react/${props.id}.png} width="75" alt="user picture" />
      <p>{props.name}</p>
    </div>
  );
}
```
```javascript
// class component User
class User extends React.Component {
  render() {
    return (
      <div>
        <img src={`https://code.s3.yandex.net/web-code/react/${this.props.id}.png} width="75" alt="user picture" />
        <p>{this.props.name}</p>
      </div>
    );
  }
}
```
1. We ended up with the `User` class which inherits from the built-in React class `React.Component`.
2. The contents of the `User` functional component were placed inside the `render()` method of the new class.
3. We are no longer passing `props` as a function argument. Instead, it's a property inside the class instance, made available to us by using the keyword `this`.
###### Component State:
<span class="blue-text-bold">state</span> - A property in React that takes an object value and holds the state
<span class="blue-text-bold">setState()</span> - a set method with "this" that sets the state: this.setState( { object } )
```javascript
// User class component
class User extends React.Component {
  constructor(props) {
    super(props);

    // starting values for component's state
    this.state = {
      rating: 0,
    };
  }

  /*
   * event handlers: change the state
   */
  handleLike = () => {
    // Sets this.state.rating to 1.
    this.setState({ rating: 1 });
  };

  handleDislike = () => {
    // Sets this.state.rating to -1.
    this.setState({ rating: -1 });
  };

  // render the javascript structure
  render() {
    return (
      <div>
        <img
          src={`https://code.s3.yandex.net/web-code/react/${this.props.id}.png}
          width="75"
        />
        <p>{this.props.name}</p>
        <div className="rating">
          <button onClick={this.handleLike}>👍</button>
          {this.state.rating}
          <button onClick={this.handleDislike}>👎</button>
        </div>
      </div>
    );
  }
}

// the main app code
ReactDOM.render(
  <>
    <h2>My Imaginary Friends:</h2>
    <User id="1" name="Gregory" />
    <User id="2" name="James" />
    <User id="3" name="Allison" />
  </>,
  document.querySelector("#root")
);
```
React will call render() again when values of this.state updates
this.setState() does this for us
<span class="red-text-bold">Event handlers are defined using arrow functions. This allows them to keep their context for this</span>
```jsx
// Use of the disabled attribute
<div className="rating">
    <button onClick={this.handleLike} 
    disabled={this.state.rating > 0}>
    👍</button>
    {this.state.rating}
    <button onClick={this.handleDislike} 
    disabled={this.state.rating < 0}>
    👎</button>
</div>
```
#### Lifecycle of Class Components:
A component updates in the following three cases:

- If `render()` is called inside the parent component
- If the state is changed as a result of calling `this.setState()`
- If an update is initialized by calling the built-in `this.forceUpdate()` method
<span class="blue-text-bold">componentDidMount()</span> - Will be called automatically once after the component is first rendered and inserted into the DOM
<span class="blue-text-bold">componentDidUpdate()</span> - Called after a component has been updated, such as state or props changed (prevProps, prevState)
<span class="blue-text-bold">componentWillUnmount()</span> - Called just before component is removed from the DOM
![[15. React component lifecycle.png]]
```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);

    this.state = { count: 0 };
  }

  tick() {
  // Another form of setState that takes a callback
    this.setState((prevState) => {
      return {
        count: prevState.count + 1
      };
    });
  }
  // Called automatically
  componentDidMount() {
    this.counter = setInterval(() => this.tick(), 1000);
    console.log("Component mounted!");
  }
  
  componentDidUpdate() {
	console.log("Component updated!");
	// Code to remove component from DOM
	if (this.state.count === 2) {
      ReactDOM.
        unmountComponentAtNode(document.getElementById('root'));
	}
  }
  
  componentWillUnmount() {
	console.log("Component will unmount!");
	clearInterval(this.counter);
  }
  
  render() {
    console.log("Component rendered!");
    return (
      <h1>{this.state.count}</h1>
    );
  }
}
/*
Component rendered!
Component mounted!
Component rendered!
Component updated!
Component rendered!
Component updated!
Component will unmount!
*/
```
#### The Virtual DOM:
Babel transpiler turns our JSX into multiple React.createElement() calls
## React Tools:
#### Vite Basics:
```javascript
import React from 'react' // we're importing the library
import ReactDOM from 'react-dom'    
// Vite handles Babel no need to import

function Switch(props) {
  // The Switch component's constructor, handler, and render function
}

ReactDOM.render((
  <>
    <Switch title="Happy" color="blue" isActive={true} />
    <Switch title="Love" color="orange" isActive={false} />
    <Switch title="Taco" color="green" isActive={false} />
  </>
), document.querySelector('#root'));
```
```bash
npm create vite@5.3 project-name
# If you already have a project you can use a . for current dir:
npm create vite@5.3 .
# navigate to the project directory
cd project-name
# install dependencies
npm install
# run the project
npm run dev
```
```json
// Modify the dev script so vite opens the webpage every time
"scripts": { 
	"dev": "vite --host --open", 
	// sometimes you need the --host flag
	// etc... 
},
```
```javascript
// vite.config.js
// Changing the default port
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {     
    port: 3000, // Change the port number to 3000
  },
});
```
#### Unboxing our project:
1. `main.jsx` is the JavaScript entry point. We'll load our root `App` component inside this file. We can also use this file for routing (navigation) throughout our app.
2. `App.jsx` is our application's root component. We'll take a look at it in a moment.
3. `App.css` is a CSS file that holds the styles for our `App.jsx` component. We can have many different CSS files inside our CSS project. In fact, it's common to give each component its own stylesheet. And thanks to Vite, we can conveniently incorporate these styles into our components with the `import` keyword.
```json
module.exports = {
  // ...
  rules: {
    "react-refresh/only-export-components": [
      "warn",
      { allowConstantExport: true },
    ],
    "react/prop-types": 0,  // New setting
  },
};
```
#### File Structure:
<span class="red-text-bold">Files need to be the same as the compnent itself and should be capitalized for example: Header.jsx or Footer.jsx</span>
```
└── src/
        ├── assets/
        ├── App.jsx
        ├── App.css
        ├── Header.jsx
        ├── Header.css
        ├── Footer.jsx
        ├── Footer.css
        ├── main.jsx
        ├── index.css
```
```
Organized file structure
└── src/
        ├── assets/
        ├── components/
        │   ├── App.jsx
        │   ├── App.css
        │   ├── Header/
        │   │   ├── Header.jsx
        │   │   ├── Header.css
        │   ├── Footer/
        │   │   ├── Footer.jsx
        │   │   ├── Footer.css
        ├── main.jsx
        ├── index.css
```
#### Importing Components:
```jsx
// App.jsx

import Header from './Header' // We don't need .jsx extension
import Main from './Main'
import './App.css' // we'll talk about CSS soon

function App() {
  return (
    <div>
      <Header />  // Inserts the HTML returned from Header.jsx
      <Main />
    </div>
  )
}

export default App
```
```jsx
// Header.jsx

function Header() {
  return (
    <div>
      <h1>Hello Vite!</h1>
    </div>
  );
};

export default Header;
```
#### Images:
```jsx
// Header.jsx
// headerLogo contains the SRC from the image 
// Also optimize it (compress, cache) final URL
import headerLogo from "../assets/logo.png";

function Header() {
  return (
    <div>
      <img src={headerLogo} alt="App logo" />
      <h1>Hello Vite!</h1>
    </div>
  );
}

export default Header;
```
#### CSS:
```css
/* Header.css */

.header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
}

.header__logo {
  width: 64px;
  height: 42px;
}

.header__title {
  font-size: 40px;
  margin-left: 12px;
}
```
```jsx
// Header.jsx

import headerLogo from "../assets/logo.png";

function Header() {
  return (
    <div className="header">
      <img className="header__logo" src={headerLogo} alt="App logo" />
      <h1 className="header__title">Hello Vite!</h1>
    </div>
  );
}

export default Header;
```
#### Fonts:
```css
/* Header.css */

/* The font can also be in a subdirectory inside assets called fonts and imported into the component css*/

@font-face {
  src: url("../assets/yellowtail-regular.woff") format("woff");
  font-family: "Yellowtail";
}

.header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
}
.header__logo {
  width: 64px;
  height: 42px;
}
.header__title {
  font-family: "Yellowtail";
  font-size: 40px;
  margin-left: 12px;
}
```
#### Dynamic Images:
```javascript
// Use this format to import images with Vite
// It will clearly show vite where the image is even with different build env
new URL("path/to/image", import.meta.url).href
```
```javascript
// utils/constants.js

export const data = [
  { 
    name: "cat",
    image: new URL("../assets/animals/cat.png", import.meta.url).href
  },
  { 
    name: "dog",
    image: new URL("../assets/animals/dog.png", import.meta.url).href
  },
  // etc...
];
```
```javascript
// components/Animals.jsx

import { data } from "../utils/constants.js";

// selectedAnimal is a string, such as "cat", "dog", etc.
function Animal({ selectedAnimal }) { // Needs to be destructured because if not it's the argument for the props object

  // Find the first animal that matches selectedAnimal.
  const animal = data.find((item) => {
    return item.name == selectedAnimal;
  });

  // Use the selected animal's image and name in the JSX.
  return <img src={animal.image} alt={animal.name} />;
}

export default Animal;
```
#### Building our project:
```bash
npm run build
npm i -g serve
# i for install -g for global serve is the name of package
serve -s dist
# -s for single page application dist for compiled / production ready files
```
## React hooks:
#### Local State: the useState() Hook:
```javascript
// This is a hook rewrite of the class component example
// the User functional component
function User(props) {
  // the hook that manages internal state
  const [rating, setRating] = React.useState(0);

  /*
   * event handlers to update internal state
   */
  function handleLike() {
    setRating(1);
  }

  function handleDislike() {
    setRating(-1);
  }

  return (
    <div className="user">
      <img src={`img/${props.id}.png`}` width="75" />
      {props.name}
      <div className="rating">
        <button onClick={handleLike} disabled={rating > 0}>👍</button>
        {rating}
        <button onClick={handleDislike} disabled={rating < 0}>👎</button>
      </div>
    </div>
  );
}
```
![[16. React Hook Syntax.png]]
<span class="red-text-bold">setRating will also call the User function again which also calls React.useState with the updated value</span>
###### The Main Rule of Hooks:
- The order of hooks should not change between component calls
- Hooks cannot be placed  inside of conditional blocks, loops, etc...
- Call them only from the main function component
```javascript
const [rating, setRating] = React.useState(0);

// this condition might be fufilled, but might not be
if (isRaining) {
    const [isBlocked, setIsBlocked] = React.useState(false);
}

// this hook might end up the second in the component, or the third 
// that's a big no-no!
const [notes, setNotes] = React.useState(['no notes yet']);
```
###### Complex Objects:
The spread operator (...) creates a modified copy of the source object
```javascript
const [array, setArray] = React.useState(['One', 'Two', 'Three']);

// this is not the correct way!
array.push('Four');
setArray(array); 

// this is the right way
setArray([...array, 'Four']);
```
```javascript
const [object, setObject] = React.useState({ name: 'James', surname: 'Wilson' });

// this is not the correct way!
object.name = 'Gregory';
setObject(object); 

// this is the right way
setObject({
  ...object,
  name: 'Gregory',
});
```
<span class="red-text-bold">This also applies when working with states on class components</span>
#### Effects: The useEffect() hook:
<span class="blue-text-bold">React.useEffect()</span> - This will have a callback that's fired everytime componentDidMount() and componentDidUpdate()
Basically after your render this component run some side code
- Fetching data from an API
- Setting up a timer
- Updating the document title
- Listening to events
```javascript
// again, with hooks
function NeonCursor() {
  const [position, setPosition] = React.useState({ top: 0, left: 0 });

  React.useEffect(() => {
    function handleMouseMove(event) {
      setPosition({
        top: event.pageY,
        left: event.pageX,
      });
    }

    // list of actions inside one hook
    document.addEventListener('mousemove', handleMouseMove);
    document.body.classList.add('no-cursor');

    // we're returning a function that remove our effects
    return () => {
      document.body.classList.remove('no-cursor');
      document.removeEventListener('mousemove', handleMouseMove);
    };
  });

  return (
    <img
      src="./cursor.png"
      width={30}
      style={{
        position: 'absolute',
        top: position.top,
        left: position.left,
        pointerEvents: 'none',
      }}
    />
  );
}
```
<span class="red-text-bold">useEffect() accepts a callback function, you can pass a dependency array as the 2nd argument</span>

|Goal|Dependency Array|When it runs|
|---|---|---|
|Run after every render|none|Every time component renders|
|Run once (on mount)|`[]`|When component first mounts|
|Run when specific value changes|`[value]`|When that value changes|
The return for useEffect is the componentWillUnmount()
## Advanced Javascript: this:
#### Strict Mode:
1. If a var hasn't been declared yet you can't assign a value to it
2. You can't override the value of an argument by accessing its index inside of arguments
3. Function parameters can't have identical names
```javascript
'use strict';

let greeting = 'Hi';

const age = Number(prompt('Enter age: ', 0));

if (age > 18) {
  // This will now throw and error
  greting = 'Hello';
}
```
```javascript
 'use strict';
 
 function consoleDog(dog) {
   arguments[0] = 'Chihuahua'; // let's try to change the first argument
   console.log(dog);
 }
 
 consoleDog('Labradoodle'); // 'Labradoodle'
```
```javascript
 'use strict';
 
 function consoleDog(dog, dog) { // SyntaxError
   console.log(dog);
 }
 
```
Using strict inside a single function:
```javascript
function callMe() {
  'use strict';
  
  // strict mode will be enabled here
}

// but not here! 
```
<span class="red-text-bold">You can't cancel strict mode</span>
#### Ways to set "this":
There are 4 ways the value of this is set inside functions:
1. A simple function call
2. When calling a function as an object method
3. Explicit binding by using call(), apply(), and bind() methods
4. When the function is used as a constructor, using the new operator

#### Default "this" binding:
```javascript
function globalFunction() {
  console.log(this);
}
// undefined in strict mode
// window in sloppy mode
globalFunction(); // Window — this refers to the global window object
```
#### Calling Object Methods:
<span class="blue-text-bold">Calling a function as an object method (implicit binding)</span>
```javascript
// Implicit Binding
window.myData = 'Important data';

function globalFunction() {
  'use strict';
  console.log(this.myData);
}

window.globalFunction(); // 'Important data'
/*********************************************/

const counter = {
  count: 0,
  increment() {
  // this is bound to the counter object
    this.count++;
    console.log('Count: ' + this.count);
  }
};

counter.increment(); // Count: 1
```
###### Loss of Context:
```javascript
const user = {
  username: 'Peter',
  auth() {
    console.log(`${this.username} has logged in`);
  }
}
// Works via implicit binding rule
user.auth(); // Peter has logged in

const adminAuth = user.auth;
// Called from the global scope (does not work)
adminAuth(); // undefined has logged in

// this also does not work when passed in as an argument
// Only when you make a annonymous function and call the object's method
button.addEventListener('click', function () {
  sendButton.click();
});
```
#### Explicit Binding: the call(), apply(), and bind() methods:
###### call():
- First parameter is the context
- Second parameter are the actual parameters
```javascript
const user = {
  username: 'Peter',
  auth(greeting) { // now this function has the greeting parameter
    console.log(`${greeting} ${this.username}`);
  }
};

const adminAuth = user.auth;

adminAuth.call(user, 'Hello'); // Hello Peter
```
###### apply():
- First parameter is the context
- Second parameter will be an array containing all the arguments to pass into the function
```javascript
const car = {
  registrationNumber: 'O287AE',
  brand: 'Tesla'
};

function displayDetails(greeting, ownerName) {
  console.log(`${greeting} ${ownerName}`);
  console.log(`Car info: ${this.registrationNumber} ${this.brand}`);
}

displayDetails.apply(car, ['Hello', 'Matt']);
// You can just use a call method with a spread operator for the arguments

/*

  Hello Matt
  Car info: O287AE Tesla

*/
```
###### bind():
- Returns a new function whose this is whatever argument we pass to bind()
```javascript
const car = {
  registrationNumber: 'O287AE',
  brand: 'Tesla'
};

function displayDetails(ownerName, greeting) {
  console.log(`${greeting} ${ownerName}`);
  console.log(`Car info: ${this.registrationNumber} ${this.brand}`);
}

// create a new function with the context bound to it. Wherever we call the boundDisplayDetails() function, this value inside it will always be the car object
const boundDisplayDetails = displayDetails.bind(car);

// Now you can call it by name - the context is bound to it
boundDisplayDetails('Matt', 'Hello');

/*

  Hello Matt
  Car info: O287AE Tesla

*/
/******************************************/
// DOM example
const sendButton = {
  buttonName: '"Send" button',
  click() {
     console.log('I am the ' + this.buttonName);
  }
};

const button = document.querySelector('.btn');

// binding the context when passing the function
button.addEventListener('click', sendButton.click.bind(sendButton));
```
#### The new Operator:
1. It constructs a new object
2. It sets the object to be the this value for the invoked function
3. It returns this
```javascript
// This breaks the implementation of this
function Plane(model) {
  this.model = model;

  // Constructor function returns an object
  return { model: 'TU-134' };
}

// Invoke Plane with new, expecting it to return the
// this object (which has this.model set to 'Airbus')
const airbus = new Plane('Airbus');

// I am surprised to find that the model isn't 'Airbus'!
console.log(airbus); // { model: "TU-134" }
/********************************/
// Do this instead
function Plane(model) {
  this.model = model;
  return this;
}

const airbus = new Plane('Airbus');
console.log(airbus); // Plane {model: "Airbus" }
```
<span class="red-text-bold">The new operator doesn't work with the call(), apply(), bind() methods. You need to set up function context later</span>
```javascript
function Windows(name) {
  this.name = 'Windows 10';
  this.printInfo = function () {
    console.log('Your OS: ' + this.name);
  };
}

const Linux = {
  name: 'Ubuntu',
};

const theOs = new Windows();

theOs.printInfo(); // Your OS: Windows 10
theOs.printInfo.call(Linux); // Your OS: Ubuntu
theOs.printInfo.apply(Linux); // Your OS: Ubuntu
theOs.printInfo.bind(Linux)(); // Your OS: Ubuntu
```
#### Arrow Functions:
<span class="red-text-bold">Arrow functions have no this. call() apply() and bind() don't work</span>
Arrow functions cannot be called with new
```javascript
const arrowFunction = () => {
  console.log(this);
};

const obj = new arrowFunction(); // TypeError: arrowFunction is not a constructor
```
You should use arrow functions in callbacks because of their short syntax
But if your function refers to this inside the body you should not use it


```javascript
/*If you declare an arrow function inside of a constructor function, its `this` value will forever be the `this` value from the constructor:*/

function Table() {
  this.rows = 4;
  this.columns = 3;
  this.printInfo = () => {
    console.log('The table has ' + this.rows + ' rows and ' + this.columns + ' columns');
  };
}

const myTable = new Table();

myTable.printInfo(); // The table has 4 rows and 3 columns
```
<span class="red-text-bold">Arrow functions don't provide `this` and rely on the scope in which they're written.</span>
