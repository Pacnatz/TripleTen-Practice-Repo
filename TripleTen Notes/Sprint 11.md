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