## React Router:
- We can build applications that contain multiple views
- Divide our application into logical sections
- Produce dynamic routes that depend on changing input parameters (user ID in database)
#### Adding a route:
<span class="red-text-bold">In order for routes to work they need to be wrapped inside a BrowserRouter</span>
<span class="blue-text-bold">BrowserRouter</span> - keeps track of the navigation history during a React Router session and ensures that the browser URL and the UI are synchronized.
```javascript
// main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom"; // New import

import App from "./components/App/App";
import "./index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    {/* Wrap the App component in a BrowserRouter */}
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```
<span class="green-text">Whenever the path changes, Routes looks through all Route elements to find the best match and renders it.</span>
```javascript
// App.jsx

import { Routes, Route } from 'react-router-dom'; // New import
import Dashboard from '../Dashboard/Dashboard'; // New import
import Header from '../Header/Header';
import './App.css';

function App() {
  return (
    <div className="App">
      <Header />
      {/* Wrap a Route component inside a Routes component
          and specify the path and element attributes as shown. */}
      <Routes>
        <Route path="/" element={<Dashboard />} />
        {/* If the path matches the current URL render this component*/}
      </Routes>
    </div>
  );
}

export default App;
```
#### Links and Navigation:
<span class="blue-text-bold">Link</span> - Works exactly the same as the a tag but it doesn't do a full refresh
```javascript
// Dashboard.jsx 
import { Link } from "react-router-dom"; // New import
import './Dashboard.css';

function Dashboard() {
  return (
    <div className="dashboard">
      <h1>Emoji Critic — All About Emojis</h1>
      <p>
        The #1 Destination for Emoji Reviews on the World Wide Web Since 2020!
      </p>
      {/* Add a Link tag that links to our /reviews route. */}
      <Link to="/reviews">
        Click here to see my latest reviews!
      </Link>
    </div>
  )
}

export default Dashboard;
```
<span class="blue-text-bold">NavLink</span> - Works exactly like link but you can set the style of the active navlink when it's the same as the route
```javascript
import { NavLink } from "react-router-dom";
import "./NavBar.css";

function NavBar() {

  // customClassName is a function that accepts an object as a parameter.
  // This object has an isActive property that is true if the link is active.
  const customClassName = ({ isActive }) =>
    "menu__link" + (isActive ? " menu__link_active" : "");

  return (
    <nav className="menu">
      {/*NavLink takes a callback with an object that returns true or false
      */}
      <NavLink to="/" className={customClassName}>
        Home
      </NavLink>
      <NavLink to="/reviews" className={
	      ({ isActive }) => isActive ? "active" : ""
	    }>
        Emoji Reviews
      </NavLink>
      <NavLink to="/about-me" className={customClassName}>
        About Me
      </NavLink>
    </nav>
  );
}

export default NavBar;
```
#### Nested Routing:
Nested routes allow you to define routes INSIDE a parent route ( aka container component)
<span class="blue-text-bold">Relative links</span> - to="about" this will be concatenated to the existing URL
<span class="blue-text-bold">Sub Routes</span> - Routes placed inside of routes that also don't need /
<span class="blue-text-bold">Outlet</span> - Where the sub routes are displayed
```javascript
// AboutMe.jsx

import "./AboutMe.css"; // New import
import { Outlet, Link } from 'react-router-dom'; // New import

function AboutMe() {
  // Add the classNames shown below so the styles are applied.
  // Add the links as shown as shown below.
  return (
    <div className="about">
      <ul className="links">
        <li>
          <Link to="my-story">My Story</Link>
        </li>
        <li>
          <Link to="hobbies">Hobbies</Link>
        </li>
        <li>
          <Link to="contact">My Contact Info</Link>
        </li>
      </ul>
      <p>I&apos;m a simple person. I see Emojis, I write reviews.</p>
      <Outlet /> {/* Where the component will render */}
    </div>
  )
}

export default AboutMe;
```
```javascript
// App.jsx
<Route path="/about-me" element={<AboutMe />}>
  <Route path="contact" element={<Contact />} />
  <Route path="hobbies" element={<Hobbies />} />
  <Route path="my-story" element={<MyStory />} />
</Route>
```
#### Dynamic Routes:
<span class="blue-text-bold">useParams()</span> - Used to get parameters from the url that are followed by a : such as
path="/reviews/:reviewId"

```javascript
// Api Call
const [reviews, setReviews] = usetState([]);

  useEffect(() => {
    // Fetch the review data from the server.
    fetch('https://api.nomoreparties.co/emoji-critic-ens').then((res) => {
	if (res.ok) {
		return res.json();
	}
	return Promise.reject(`Error: ${res.status}`);
    }).then((data) => {
      // Pass the parsed response body to the setter function.
      setReviews(data);
    })
    .catch(console.error);
  // An empty dependency array means the hook only runs when 
  // component launches.
  }, []);
```

```javascript
// in App.jsx

<Routes>
  <Route path="/" element={<Dashboard />} />
  <Route path="/reviews" element={<Reviews reviews={reviews} />} />

  {/* New route, with new Review component. */}
  <Route
  // This will act as a variable for the useParams hook
    path="/reviews/:reviewId" 
    element={<Review reviews={reviews} />}
  />
  {/* ... */}
```
```javascript
function Reviews({ reviews }) {
  return (
    <div className="reviews">
      <ul className="reviews__list">
        {reviews &&
          reviews.map((review) => {
            return (
              <li key={review.id} className="reviews__item">
	            {/* This will link to /reviews/:reviewId*/}
                <Link to={`${review.id}``}>{review.title}</Link>
              </li>
            );
          })}
      </ul>
    </div>
  );
}
```
```javascript
// Review.jsx

import "./Review.css";
import { useParams } from "react-router-dom";

function Review({ reviews }) {
  const params = useParams();
  let id = params.reviewId;
  // Decrement the id from the parameter so we access the correct items. This
  // is necessary because the array indexes start with 0, whereas the IDs in 
  // the API begin at 1.
  id = id - 1;

  return (
    <div className="review">
      {reviews && (
        <div className="review__item">
          <h3>{reviews[id]?.title}</h3>
          <p>{reviews[id]?.text}</p>
          <p className="review__rating">Final rating:{reviews[id]?.rating}/5</p>
        </div>
      )}
    </div>
  );
}

export default Review;
```
#### Programmatic Navigation:
<span class=blue-text-bold>navigate("/reviews");</span> - Navigate back to the reviews view
<span class=blue-text-bold>navigate(-1);</span> - Emulates the browsers back button
```javascript
// Review.jsx

import { useParams, useNavigate } from "react-router-dom"; // New import

import "./Review.css";

function Review({ reviews }) {
  // Access the hook inside of the component.
  const navigate = useNavigate();
  const params = useParams();
  let id = params.reviewId;
  id = id - 1;

  return (
    <div className="review">
      {reviews && (
        <div className="review__item">
          <h3>{reviews[id]?.title}</h3>
          <p>{reviews[id]?.text}</p>
          <p className="review__rating">Final rating:{reviews[id]?.rating}/5</p>
          
          {/* Add a button. */}
          <button type="button" onClick={() => navigate("/reviews")}>
	          Back to the review list
		  </button>
        </div>
      )}
    </div>
  );
}

export default Review;
```
#### Creating a 404 Page:
<span class="blue-text-bold">path="*"</span> - The * symbolizes that any bad route endpoint will lead to this element
```javascript
// App.jsx
<Route path="*" element{<PageNotFound />} />
```
## React and Data:
#### Lifting State:
- Move the state UP into their parent
- Parent owns the state
- Parent passes it DOWN to children as props
```javascript
function App() {
  const [size, setSize] = React.useState(0);
  
  function handleChange(e) {
    setSize(e.target.value);
  }
  
  return (
    <>
      <Slider size={size} onChange={handleChange}/>
    </>
  );
}

function Slider(props) {
  return (
    <div id="slider-container">
      <input type="range" min="0" max="100" value={props.size} onChange={props.onChange} />
      <div className="counter">{props.size}</div>
    </div>
  );
}
```
#### Creating and Adding Context:
```javascript
// translationContext.js

export const TranslationContext = React.createContext();

export const translations = {
  en: {
    greeting: 'Hello World',
  },
  ru: {
    greeting: 'Привет, мир!',
  },
};
```
```javascript
// App.js

// import the context object
import { TranslationContext, translations } from './translationContext';

function App() {
  // state responsible for the current language
  const [lang, setLang] = React.useState('en');

  return (
        // Use data from translations[lang] using Context.Provider
    <TranslationContext.Provider value={translations[lang]}>
            {/* The subtree, in which the context will be accessed */}  
      <Main />
    </TranslationContext.Provider>
  );
}
```
#### Subscribing to a Context:
```javascript
function Header() {
  // Subscribing to TranslationContext, and assigning it to a variable.
  const translation = React.useContext(TranslationContext);

    // Now we can access the context's values via this variable.
  return (
    <h1>
      {translation.greeting}
    </h1>
  );
}
```
```javascript
// Class Component equivilant
import { TranslationContext } from './translationContext';

class Header extends React.Component {
  // Assigning a contextType, which let's the component know which context.  
  // React will find the closest translation context provider and make its
  // value available as `this.context`.
  static contextType = TranslationContext;

  // Now we can access the translation context's values via `this.context`.
  render() {
    return (
      <h1>
        {this.context.greeting}!
      </h1>
    );
  }
}
```
<span class="red-text-bold">Class components can only subscribe to one context at a time</span>
## Advanced React
#### Lists and Keys:
- Type (it can be an element, component, string, or empty node)
- Tag name or component name
If one of these 2 aren't met the node and child nodes must be completely replaced.
<span class="red-text-bold">With list keys you can move the list element around without having to update all the affected list elements. Sometimes the state of an element gets mixed up</span>
#### Working with Forms:
<span class="blue-text-bold">controlled inputs</span> - Form input elements that are controlled within the component state
```javascript
function Form() {
  const [inputValue, setInputValue] = useState('');

  const handleChange = (event) => {
    setInputValue(event.target.value);
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log(inputValue);
  };
  
  const handleReset = () => {
    setInputValue("");
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Name:
        <input 
          type="text" 
          {/* React re-renders the component with the new state */}
          value={inputValue} {/* value makes it a controlled component */}
          onChange={handleChange}
        />
      </label>
      <button type="submit">Submit</button>
      <button onClick={handleReset} type="reset">Reset</button>
    </form>
  );
}

export default Form;
```
How to handle the data from the form submit:

1. Create a handler function in the component that you need to access the state.
2. Pass this handler to the form component (as a prop or via context).
3. Call the passed handler when the form is submitted and pass it the form submission data.
#### Custom Hooks: useForm:
The previous way of making forms doesn't scale well. Especially with 20-50 input fields
```javascript
import { useState } from 'react';

function RegistrationForm() {
  // A single state variable instead of one per input
  const [values, setValues] = useState({
    name: '',
    email: '',
    password: '',
    confirmPassword: ''
  });

    // A single change handler instead of one per input field
  const handleChange = (event) => {
    const { name, value } = event.target;
    setValues({ ...values, [name]: value });
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log(values);
  };
  
  // Each input has its value controlled by a property of the
  // values object, and they all use the same handler
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="name"
        placeholder="Name"
        value={values.name}
        onChange={handleChange}
      />
      <input
        type="email"
        name="email"
        placeholder="Email"
        value={values.email}
        onChange={handleChange}
      />
      <input
        type="password"
        name="password"
        placeholder="Password"
        value={values.password}
        onChange={handleChange}
      />
      <input
        type="password"
        name="confirmPassword"
        placeholder="Confirm Password"
        value={values.confirmPassword}
        onChange={handleChange}
      />
      <button type="submit">Register</button>
    </form>
  );
}
```
<span class="blue-text-bold">Custom Hooks</span> - JS functions that use React hooks internally and allow you to extract stateful logic into separate functions for reuse between components.
- They start with the word "use" (like `useState` or `useEffect`)
- They can call other hooks inside them
- They return values that components can use
```javascript
import { useState } from "react";

// useForm() accepts an object of default values as an argument,
// creates a state object, its setter, and a change handler, and
// returns them.
export function useForm(defaultValues) {
  const [values, setValues] = useState(defaultValues);

  const handleChange = (event) => {
    const { value, name } = event.target;
    setValues({ ...values, [name]: value });
  };

  return { values, handleChange, setValues };
}
```
```javascript
// Import the useForm hook
import { useForm } from './useForm';

function RegistrationForm() {
  // Call useForm() to receive the state and handler
  const { values, handleChange, setValues } = useForm({
    name: '',
    email: '',
    password: '',
    confirmPassword: ''
  });
 
  // Use values, setValues, and handleChange
  // in the same way as before
}
```
#### Refs:
<span class="blue-text-bold">internal component variables</span> - Local variables that can be used inside components
When you should use refs:
- Managing focus, text selection, or media playback
- Triggering imperative animations
- Integrating with third-party DOM libraries
<span class="blue-text-bold">useRef()</span> - Returns an object which we can assign to any element in our JSX code via the ref attribute
```javascript
// Functional components
function VideoPlayer() {
  const videoRef = React.useRef(); // assigning the object returned by a hook to a variable

  function handleClick() {
    videoRef.current.play(); // calling the required method on the current property of the object
  }

  return (
    <>
      <video ref={videoRef} src="./clip.mp4" /> // pointed a ref attribute to the element => got direct access to the DOM element
      <button onClick={handleClick}>▶️</button> /* attached a handler to a button */
    </>
  );
}
```
```javascript
// Class components
class VideoPlayer extends React.Component {
  constructor() {
    super();
    this.videoRef = React.createRef(); // created a ref and assigned it to a variable - it will be a property of this
  }

  handleClick = () => {
    this.videoRef.current.play(); // similarly, we call the required method on the current field of the object
  };

  render() {
    return (
      <>
        <video ref={this.videoRef} src="./clip.mp4" /> //
        <button onClick={this.handleClick}>▶️</button> //
      </>
    );
  }
}
```
<span class="red-text-bold">refs do not rerender the page!</span>
Use refs when :
- Storing and manipulating DOM elements.
- Storing other objects that aren't necessary to calculate the JSX.
- Integrating with third-party libraries.
#### Pure Components:
<span class="red-text-bold">When a parent component rerenders it triggers all the children to rerender too</span>
<span class="blue-text-bold">Pure components</span> - Components that skips re-rendering if its props haven't changed

```javascript
// Making a functional component pure
const Chat = React.memo((props) => {
  return (
    <div className="chat">
      <img src={`img/${props.id}.png width="75" />
      <h2>{Math.random()}</h2>
      <div className="date">{props.lastMessageAt}</div>
    </div>
  );
});
```
```javascript
// Making a class component pure
class Chat extends React.PureComponent {
  render() {
    return (
      <div className="chat">
        <img src={`img/${this.props.id}.png width="75" />
        <h2>{Math.random()}</h2>
        <div className="date">{this.props.lastMessageAt}</div>
      </div>
    );
  };
}
```
###### Shallow Comparisons:
React only checks the first level of an object array and uses the === operator to compare values.
Objects and Functions are compared by reference
```javascript
// These look the same to us, but JavaScript sees them as different objects
const array1 = ['Gregory', 'James', 'Allison'];
const array2 = ['Gregory', 'James', 'Allison'];

console.log(array1 === array2); // false! Different memory locations
```
<span class="red-text-bold">React will rerender every frame because new objects are being made. Move objects and functions outside</span>
```javascript 
const USER_NAMES = ['Gregory', 'James', 'Allison']; // Created once 
const handleClick = () => console.log(1); // Created once

function ParentComponent() { 
	return ( 
		<MyPureComponent 
			userNames={USER_NAMES} // Same reference every time 
			onClick={handleClick} // Same reference every time 
		/> 
	); 
}
```
#### Higher-Order Components (HOCs):
<span class="blue-text-bold">High-order components</span> - Enhance the functionality of one or several existing components i.e. pure components, React.memo(). 