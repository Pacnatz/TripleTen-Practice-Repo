## Front-End Authentication with React:
#### Protecting Routes on the Front End:
<span class="blue-text-bold">replace attribute</span> - React Router replaces the current history entry, and back button skips the current page
<span class="blue-text-bold">HOC</span> - Higher-Order Components are functions that take a component as an argument and return a new component that's the same input but with some additional or modified behavior
<span class="blue-text-bold">Wrapper Components</span> - Regular components that wrap another component to modify it's behavior
```js
// ProtectedRoute.jsx
// This is a wrapper component

import { Navigate } from "react-router-dom";

function ProtectedRoute({ isLoggedIn, children }) {
  if (!isLoggedIn) {
    // If user isn't logged in, return a Navigate component that sends the user to /login
    return <Navigate to="/login" replace />;
  }
    
  // Otherwise, render the protected route's child component.
  return children;
}

export default ProtectedRoute;
```
```jsx
<Route 
  path="/ducks"
  element={ 
	<ProtectedRoute isLoggedIn={isLoggedIn}> 
		<Ducks /> 
	</ProtectedRoute> 
  } 
/>
```
#### Registration:
If you’re returning JSX → `<Navigate />` 
If you’re inside a function → `useNavigate()`
```js
// src/utils/auth.js

// Specify the BASE_URL for the API.
export const BASE_URL = "https://api.nomoreparties.co";

// The register function accepts the necessary data as arguments,
// and sends a POST request to the given endpoint.
export const register = (username, password, email) => {
  return fetch(`${BASE_URL}/auth/local/register`, {
    method: "POST",
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ username, password, email }),
  })
    .then((res) => {
      return res.ok ? res.json() : Promise.reject(`Error: ${res.status}`);
    })
};
```
#### Logging In:

```js
// in src/utils/auth.js

// The authorize function accepts the necessary data as parameters.
export const authorize = (identifier, password) => {
  // A POST request is sent to /auth/local.
  return fetch(`${BASE_URL}/auth/local`, {
    method: "POST",
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json",
    },
    // The parameters are wrapped in an object, converted to a JSON
    // string, and sent in the body of the request.
    body: JSON.stringify({ identifier, password }),
  }).then((res) => {
    return res.ok ? res.json() : Promise.reject(`Error: ${res.status}`);
  });
};
```
#### Local Storage:
<span class="blue-text-bold">Session storage</span> - Data is removed when session ends (closing browser window)
<span class="blue-text-bold">Local storage</span> - Persist until manually deleted
- `setItem()` is used to save data
- `getItem()` is used to get saved data
- `removeItem()` is used to remove data from memory
```js
// saving username
localStorage.setItem('username', 'Dr_Marvin_Quack');

// getting username
localStorage.getItem('username'); // "Dr_Marvin_Quack"

// deleting username
localStorage.removeItem('username');

// if the key doesn't exist, null is returned
localStorage.getItem('username'); // null
```
```js
// Converting an object to JSON and saving it to local storage.
localStorage.setItem('user', JSON.stringify({
  firstName: 'Marvin',
  lastName: 'Quack'
}));

// Retrieving the JSON from local storage and parsing it into
// an object.
JSON.parse(localStorage.getItem('user'));

//  {
//    firstName: 'Marvin',
//    lastName: 'Quack'
//  }
```
#### Checking User's Token
```js
// utils/token.js

const TOKEN_KEY = "jwt";

// setToken accepts the token as an argument, and adds it to
// with localStorage the key TOKEN_KEY.
export const setToken = (token) =>
  localStorage.setItem(TOKEN_KEY, token);

// getToken retrieves and returns the value associated with 
// TOKEN_KEY from localStorage.
export const getToken = () => {
  return localStorage.getItem(TOKEN_KEY);
};
```
```js
const handleLogin = ({ username, password }) => {
  if (!username || !password) {
    return;
  }

  auth
    .authorize(username, password)
    .then((data) => {
      if (data.jwt) {
        // Save the token to local storage
        setToken(data.jwt);
        setUserData(data.user);
        setIsLoggedIn(true);
        navigate("/ducks");
      }
    })
    .catch((err) => console.log(err));
};
```
**Checking for a token on the initial page load**:
```js
// in App.jsx

useEffect(() => {
  const jwt = getToken();
    
  if (!jwt) {
    return;
  }

  // TODO - handle JWT
}, []);
```
**Authenticating the user if the token is valid:**
```js
// api.js

export const BASE_URL = "https://api.nomoreparties.co";

// getContent accepts the token as an argument.
export const getUserInfo = (token) => {
  // Send a GET request to /users/me
  return fetch(`${BASE_URL}/users/me`, {
    method: "GET",
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json",
      // Specify an authorization header with an appropriately
      // formatted value.
      Authorization: `Bearer ${token}`,
    },
  }).then((res) => {
    return res.ok ? res.json() : Promise.reject(`Error: ${res.status}`);
  });
}
```
```jsx
// Inside of App.jsx
useEffect(() => {
  const jwt = getToken();
  if (!jwt) {
    return;
  }
  getUserInfo(token)
    .then(({ username, email }) => {
      setUserData(username, email)
      setIsLoggedIn(true);
      navigate('/ducks');
    })
    .catch((err) => {
		console.error(err);
    }
```
#### The useLocation Hook:
<span class="blue-text-bold">location</span> - An object describing the current URL
```json
{
  pathname: "/ducks",
  search: "",
  hash: "",
  state: undefined,
  key: "abc123"
}
```

<span class="blue-text-bold">useLocation</span> - Allows us to access important information about the current location-related state of the application
- <span class="blue-text-bold">pathname</span> - Stores the path segment of the current URL. `/ducks`
- <span class="blue-text-bold">state</span> - Allows us to access values that were stored in state when a \<Link> or \<Navigate> tag is rendered
**Updating the ProtectedRoute Component:**
```jsx
// New import - useLocation
import { Navigate, useLocation } from "react-router-dom";

// New prop - anonymous. This prop will be used to indicate routes
// that can be visited anonymously (i.e., without authorization). 
// The two 'anonymous' routes in this application are /register 
// and /login.
export default function ProtectedRoute({
  isLoggedIn,
  children,
  anonymous = false,
}) {

  // Invoke the useLocation hook and access the value of the
  // 'from' property from its state object. If there is no 'from'
  // property we default to "/". 
  const location = useLocation();
  const from = location.state?.from || "/"; // Check if the state has .from if it does use it if not use "/"
  
  // If the user is logged in we redirect them away from our 
  // anonymous routes.
  if (anonymous && isLoggedIn) {
    return <Navigate to={from} />;
  }

  // If a user is not logged in and tries to access a route that
  // requires authorization, we redirect them to the /login route.
  if (!anonymous && !isLoggedIn) {
    // While redirecting to /login we set the location objects
    // state.from property to store the current location value.
    // This allows us to redirect them appropriately after they
    // log in.
    return <Navigate to="/login" state={{ from: location }} />;
    // Login route now receives location.pathname === "/login"
    // location.state = {
    //   from: {
    //     pathname: "/ducks"
    //   }
    // }
    // state is a navigation state. Used with useLocation
  }

  // Otherwise, display the children of the current route.
  return children;
}
```

```jsx
import { useLocation } from react-router-dom

function App() {
	// Invoke the hook
	const location = useLocation();
	
	const handleLogin = ({ username, password }) => {
	  if (!username || !password) {
	    return;
	  }
	  auth
	    .authorize(username, password)
	    .then((data) => {
	      if (data.jwt) {
	        setToken(data.jwt);
	        setUserData(data.user);
	        setIsLoggedIn(true);
	        // After login, instead of always navigating to /ducks
	        // navigate to the location that is stored in state
	        // If there is no stored location, we default to /ducks
	        const redirectPath = location.state?.from?.pathname || "/ducks";
	        navigate(redirectPath);
	      }
	    })
	}
}
```
#### Logging Out:
1. Remove the JWT from local storage
2. Log the user out
3. Redirect the user back to the login page
```js
// in utils/token.js

export const removeToken = () => {
  localStorage.removeItem(TOKEN_KEY);
}
```
```jsx
// NavBar.jsx

// New import - useNavigate
import { NavLink, useNavigate } from "react-router-dom";

// New import
import { removeToken } from "../utils/token";
import Logo from "./Logo";
import "./styles/NavBar.css";

// Specify setIsLoggedIn as a prop. Don't forget to pass
// setIsLoggedIn as a prop from the App component!
function NavBar({ setIsLoggedIn }) {
  // Invoke the hook.
  const navigate = useNavigate();

  // The signOut function removes the token from local
  // storage, sends them back to the login page, and 
  // sets isLoggedIn to false.
  function signOut() {
    removeToken();
    navigate("/login");
    setIsLoggedIn(false);
  }

  return (
    {/* ... */}
    {/* Add the onClick listener. */}
        <li>
          <button onClick={signOut} className="navbar__link navbar__button">
            Sign Out
          </button>
        </li>
      </ul>
    </div>
  );
}

export default NavBar;
```
## Intro to Web Application Security:
#### Cross-Site Scripting (XSS):
<span class="blue-text-bold">XSS attacks</span> - Inject malware into the page that the server sends to the user.
<span class="green-text">We cannot simply just ditch innerHTML</span>
```js
// Query param example
const express = require('express');
const app = express();

// code for submitting the review
app.get('/', (req, res) => {
  res.send(`Review author: ${req.query.name}`);
});

app.listen(3001);
// This link can hack it and alert hacked
http://example.com/?name=Kevin+<script>alert('Hacked!');</script>
```
**Restrict User Input:**
- Make names latin characters and a space
- Make phone numbers digits and hyphens
- Make emails latin characters and an at sign (@)
**Escaping characters in HTML:**

```js
import escape from "escape-html";
const safeString = escape(userInput);
```
**Content Security Policy:**
<span class="blue-text-bold">Content Security Policy standard</span> - This instruction allows you to restrict the sources from which the site can load scripts, images, stylesheets, and media files
```html
 <meta http-equiv="Content-Security-Policy" content="INSTRUCTIONS">
```
```json
// HTTP header
 Content-Security-Policy: /* INSTRUCTIONS */
```
- <span class="blue-text-bold">default-src</span> - Instruction that sets the source of all kinds of resources by default
- <span class="blue-text-bold">script-src</span> - permitted source of scripts
- <span class="blue-text-bold">img-src</span> - permitted source for images
- <span class="blue-text-bold">media-src</span> - permitted source for audio and video files
- <span class="blue-text-bold">style-src</span> - permitted source for style files
<span class="blue-text-bold">self</span> - Specifies the domain of the site as the source
.

`"script-src 'self' *.site.com"; // scripts can be loaded from the site itself, or from subdomains of site.com, such as https://example.site.com`

```js
// Set as response header middleware
res.setHeader(
	Content-Security-Policy: default-src 'self'; img-src *; media-src media1.com media2.com; script-src userscripts.example.com
);
```
**Checking the Browser Version:**
```js
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
```
Some large sites don't allow older browsers but it depends on the team
#### Ways to Store JWT in the Browser:
<span class="red-text-bold">Authorization header uses localStorage and is accessible from Javascript. So it's vulnerable to XSS attacks</span>
If your site has a lot of 3rd party dependencies one of them may contain malicious code
- Cookies is an alternative the browser handles it automatically
**Cookies:**
<span class="blue-text-bold">cookies</span> - local data files that store data in the <span class="red-text-bold">BROWSER</span> not the frontend
<span class="blue-text-bold">signedCookies</span> - cookies that the user can't change the value of. e.g. admin->superadmin
- HTTP is stateless by default the server does not know WHO (the user) is it knows nothing when a request is made
- We need to use cookies to store information about the user, so if the user makes a shopping cart and closes it, they can come back and the server will know it's them
**Server sending cookie:**
```js
app.get('/', (req, res) => {
  // (name of cookie, value of cookie, options object)
  res.cookie('name', 'Elise', { maxAge: 3600000 }); // Expires 1 hour
  res.status(200).send({ msg: "hello" });
})

// Use .end() if the response has no body
res.cookie('name', 'Elise', { maxAge: 3600000, httpOnly: true }).end();
```
**Server receiving cookie:**
```js
app.get('/api/product', (req, res) => {
  // This will return undefined. Cookie is not in req body
  console.log(req.cookies);
  // You need to access the cookie from the HEADER
  console.log(req.headers.cookie); // This result will be a raw string. Not parsed yet name=Elise
})
```
**Installing cookie-parser:**
<span class="blue-text-bold">cookie-parser</span> - A node dependency package that parses raw cookies to parsed cookies. Can also be used to parse a signed cookie with a secret key
```bash
npm i cookie-parser
```
```js
import cookieParser from "cookie-parser";
import express from "express";

const app = express();

const PORT = process.env.PORT || 3000;

app.use(express.json());
// cookie-parser middleware
app.use(cookieParser()); // SecretKEY as an argument for signed cookies

app.get('/api/product', (req, res) => {
  // No longer need to use req.headers
  console.log(req.cookies); // { name: 'Elise'}
  if (req.cookies.name && req.cookies.name === 'Elise') {
    return res.send({ message: 'Cookie validated!' })
  }
  return res
    .status(401)
    .send({ message: 'Sorry you need the correct cookie!' });
}) 

// Route for signed cookies
app.get('/', (req, res) => {
  // Need the signed key in the options object.
  res.cookie('name', 'Nathan', {maxAge: 3600000, signed: true})
})

app.get('/api/product', (req, res) => {
  // Need to pass a SecretKEY argument to the cookieParser middleware
  console.log(req.signedCookies);
  if (req.signedCookies.name && req.cookies.name === 'Nathan') {
    return res.send({ message: 'Cookie validated!' });
  }
  return res
    .status(401)
    .send({ message: 'Sorry you need the correct cookie!' });
})
app.listen(PORT, () => {
  console.log(`Listening on port ${3000}`);
});
```

**Sending Cookies with fetch():**
<span class="blue-text-bold">credentials</span> - The credentials options need to be enabled in our fetch request if our frontend and backend are on different domains
```js
fetch('/api/products', {
  method: 'POST',
  credentials: 'include',
});
```
#### CSRF, Brute Force and DDOS:
<span class="blue-text-bold">CSRF</span> - Cross-Site Request Forgery Attack. The browser sends a cookie to the corresponding domain. 
- A user is already logged into a legitimate site (like their bank)
- User visits a malicious website
- Website / Hacker knows (or correctly guesses) the banks endpoints and uses the user's cookies stored in the browser to send requests to those endpoints and the correct parameters
- Browser sends cookies with the same domain to the site with that domain even if the user is not on the webpage
```js
res
  .cookie('jwt', token, {
    maxAge: 3600000,
    httpOnly: true, // Can only be accessed by http not javascript
    sameSite: strict // Only uses the cookie if the user is on that domain
  })
```
For large apps and banking institutions you should implement CSRF tokens
**Brute Force and DDOS attacks:**
To protect against them you must use the `express-rate-limit` middleware
```js
const express = require('express');
const rateLimit = require('express-rate-limit');

const app = express();

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // in 15 minutes
  max: 100 // You can make a maximum of 100 requests from one IP
});

// applying the rate-limiter
app.use(limiter);
```
#### Using External Components:
Using libraries with the \<script> element can cause vulnerabilities because you cannot control the codebase.
- You can explicitly specify versions of the attached libraries
- Even safer is to download the module and put it into the project directory (though this can increase build time and file size)
```html
<script src="https://code.jquery.com/jquery.slim.min.js"></script>
```
npm dependencies can have vulnerabilities to fix it you can run the audit command
```bash
npm audit
# If there are vulnerabilities you can run
npm audit fix
```
**Vulnerability Databases:**
<span class="blue-text-bold">CVE Details</span> - One of the largest open-source databases of well-known vulnerabilities. All components should be checked with this database before using them in a project.
#### Web Application Security: Conclusion and Security Checklist:
<span class="blue-text-bold">helmet module</span> - an express middleware that automatically sets various HTTP security headers
```bash
npm i helmet
```
```js
import helmet from "helmet":
app.use(helmet());
```
**Checklist:**
- Set response headers (helmet)
- Validate user data (escape-html)
- Think about where the data is stored on the client side. (localStorage, cookies)
- Remember CSRF (SameSite option)
- Be prepared for Brute Force and DDOS attacks (express-rate-limit)
- Check dependencies (npm audit)
## Intro to Functions under the Microscope:
#### The scope chain:
<span class="blue-text-bold">scopes</span> - Regions where variables and functions are accessible
<span class="blue-text-bold">scope chain</span> - Inside a function it looks for variables in the current scope first then moves to the global scope works outwards.
#### What are closures:
<span class="blue-text-bold">closure</span> - Created when a function retains access to the variables from its outer scope. Even if the outer scope returned, closures allow functions to remember the environment in which they were created
```js
function outerFunction() {
  const outerVariable = "Hello, world!";

  function innerFunction() {
    console.log(outerVariable); // Accessing a variable from the outer scope
  }

  return innerFunction;
}

// Create a closure
const myClosure = outerFunction();

// Call the closure
myClosure(); // Logs: "Hello, world!"
```
<span class="red-text-bold">Every function in javascript remembers the scope in which it was created. The memory is stored in an internal property called [[Environment]] (not accessible in JS code)</span>
1. Private variables
```js
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```
2. Function factories
```js
function greet(name) {
  return function () {
    console.log(`Hello, ${name}!`);
  };
}

const greetJohn = greet("John");
const greetJane = greet("Jane");

greetJohn(); // Logs: "Hello, John!"
greetJane(); // Logs: "Hello, Jane!"
```
#### Event Listeners and Closures:
```js
function createCounter() {
  let count = 0; // Private variable

  // Public methods
  function increment() {
    count += 1;
    console.log(count);
  }

  function reset() {
    count = 0;
    console.log(count);
  }

  return { increment, reset };
}

const counter = createCounter();

counter.increment(); // Logs: 1
counter.increment(); // Logs: 2
counter.reset(); // Logs: 0
```
Same as classes:
```js
class Counter {
  constructor() {
    this.count = 0;
  }

  increment() {
    this.count += 1;
    console.log(this.count);
  }

  reset() {
    this.count = 0;
    console.log(this.count);
  }
}

const counter = new Counter();
counter.increment(); // Logs: 1
counter.increment(); // Logs: 2
counter.reset(); // Logs: 0
```