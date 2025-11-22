## Project Building with Webpack
#### NPM: Node Package Manager:
```bash
# Run this in the project folder
npm init
# Initializes package.json
```
1. `package name`: Keep the default or choose an appropriate name for the project.
2. `version`: Keep the default, or choose `0.1.0`. The latter is good for projects that are still in the early stages of development (i.e., pre-release).
3. `licence`: The terms and conditions under which code in a package can be distributed or reused. The default value is fine.
4. `type`: Enter `module`. The `commonjs` type doesn’t allow the more modern ES6 `import`/`export` syntax.
###### Installing packages:
```bash
# Make sure you're in the project folder
npm install jquery
```
###### Semantic versioning:
major.minor.patch
3.5.1
3 is the major version
5 is the minor version
1 is the first patch of version 3.5
```json
"dependencies": {
   "jquery": "^3.5.1" // ^ means "pin the major version number"
   // Means I want version 3 don't update beyond this
   "lodash": "4.17.21" // means never install 4.18 or 5.0 automatically
 }
```
^ - Pin major, allow minor + patch update
~ - Pin major + minor, allow patch update
None - Pin exact version only
```bash
# install the most recent version
npm install package-name

# install version 1.2.x, where x is the most recent patch
npm install package-name@~1.2.3

# install version 1.x.y, where x.y is the most recent minor.patch version
npm install package-name@^1.2.3

# install specific version 1.2.3 
npm install package-name@1.2.3 --save-exact
```
###### Configuring --save-exact set automatically:
1. Add a file .npmrc to project root
2. Insert into file: save-exact=true

#### Understanding a Complete Webpack Configuration:
```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: {
    main: "./src/index.js",
  },
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
    publicPath: "",
  },
  mode: "development",
  devtool: "inline-source-map",
  stats: "errors-only",
  devServer: {
    static: path.resolve(__dirname, "./dist"),
    compress: true,
    port: 8080,
    open: true,
    liveReload: true,
    hot: false,
  },
  target: ["web", "es5"],
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: "babel-loader",
        exclude: "/node_modules/",
      },
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: "css-loader",
            options: {
              importLoaders: 1,
            },
          },
          "postcss-loader",
        ],
      },
      {
        test: /\.(png|svg|jpg|jpeg|gif|woff(2)?|eot|ttf|otf)$/,
        type: "asset/resource",
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin(),
  ],
};
```
###### Imports and exports:
<span class="blue-text-bold">require()</span> - Statements that are similar to import statements (We're importing 4 modules)
<span class="blue-text-bold">module.exports</span> - Object where we'll write all the settings (similar to export)
###### The entry point (where to start bundling):
<span class="green-text">The filepath in the entry object tells webpack to start by looking in src/index.js and follow all the imports from there</span>
###### Output (where to put the bundled code):
<span class="green-text"> The bundle would be placed in a directory called "dist" and the JS file would be named "main.js"</span>
###### The local development server:
<span class="green-text">When working with webpack, you won't use Live Server to run your code. Webpack comes with its own development server, the devServer object configures this server</span>
###### Module rules (how to process different file types):
- When you encounter a .js file, process it with Babel (a tool introduced later)
- When you see a .css file, process it with these CSS tools (omitted for now)
- When you find images or fonts with any of those extensions optimize and include them in the bundle
###### Plugins:
<span class="blue-text-bold">HtmlWebpackPlugin</span> - automitcally generates and HTML that includes all your webpack bundles. No need to manage the script and link tags
<span class="blue-text-bold">CleanWebpackPlugin</span> - Removes files from the output directory before building a new bundle. This prevents an accumulation of files
<span class="blue-text-bold">MiniCssExtractPlugin</span> - Extracts CSS from your JavaScript bundles into separate CSS files for better caching and performance
###### Other configuration files:
<span class="blue-text-bold">babel.config.js</span> - a Javascript transpiler that converts modern JS into older browser-compatible versions
```javascript
const presets = [
  ["@babel/preset-env", {
    targets: "defaults, IE 11, not dead",
    useBuiltIns: "entry",
    corejs: "^3",
  }],
];

module.exports = { presets };
```
<span class="blue-text-bold">postcss.config.js</span> - Controls how CSS is processed. 
1. autoprefixer - automatically adds vendor prefixed to CSS properties (webkit)
2. cssnano - minifies and optimizes our CSS
```javascript
const autoprefixer = require("autoprefixer");
const cssnano = require("cssnano");

module.exports = {
  plugins: [
    autoprefixer,
    cssnano({ preset: "default" }),
  ],
};
```
<span class="blue-text-bold">package.json</span> - Contains direct dependencies and metadata like scripts, version, author
<span class="blue-text-bold">package-lock.json</span> - Records exact versions of all dependencies, including nested dependencies, to ensure reproducible builds
#### Managing Node Modules:
###### Managing Dependencies:
<span class="blue-text-bold">Explicit Dependencies</span> - These are packages you directly decide to use in your project. Such as import React from 'react'. Will get listed in your package.json file under dependencies or devDependencies
<span class="blue-text-bold">Implicit dependencies</span> - These are packages that your chosen packages need to work. Such as React might need other helper packages to function

<span class="red-text-bold"> Avoid checking node modules into version control. Use .gitignore to instruct Git to ignore them (add node_modules/ line)</span>

###### Regular dependencies vs dev dependencies:
- jquery is a regular dependency meaning it will be used in both production and development
- webpack is a dev dependency meaning it will be used in development only
```bash
npm install webpack --save-dev
# --save-dev tells npm that this package is only needed for development. Adds package to devDependencies
```
#### Build and Dev Scripts:

| Tool                           | Development build                                                        | Production build                                                     |
| ------------------------------ | ------------------------------------------------------------------------ | -------------------------------------------------------------------- |
| Old syntax and vendor prefixes | **Not needed.** They have no useful information and make our code messy. | **Needed.** These solve the problem of cross-browser compatibility.  |
| Webpack                        | **Needed.** The project needs to be rebuilt as we update the code.       | **Not needed.** The project is already built; no need to rebuild it. |
###### Adding scripts to package.json:
```json
"scripts": {
  "commandName": "echo 'script executed'"
},
```
```bash
npm run commandName
# Causes the following command to run: echo 'script executed'
```
```json
"scripts": {
  "build": "webpack --mode production",
  "dev": "webpack serve --mode development"
},
```
###### The dev script:
```bash
npm run dev
```
Will run a local server with the devServer object
```javascript
// Excerpt from webpack.config.js
 
devServer: {
  static: path.resolve(__dirname, "./dist"),
  compress: true,
  port: 8080,
  open: true,
  liveReload: true,
  hot: false,
},
```
###### The build script:
An actual production-ready build. It minimizes, optimizes, and transpiles your code, and outputs the results to the directory that we specified
###### Ignoring copiled files:
Add dist/ to gitignore

#### Babel: A JS Transpiler:
1. @babel/core (The main translator)
2. babel-loader (The Bridge to Webpack)
3. @babel/preset-env (Premade translation rules collection)
4. Polyfills - Instruction manuals to teach old browsers how to do new things (such as map() function)
#### HTML Webpack Plugin:
###### HTMLWebpackPlugin setup:
```javascript
// Excerpt from webpack.config.js

const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  
  // plugins array
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
    // ... more plugins
  ],
};
```
#### CSS Processing:
We cannot connect stylesheets to our HTML with \<link> anymore
We need to import our primary CSS file into index.js
```javascript
// index.js
import "./pages/index.css";
// All the CSS and CSS imported into that file will be used in the final build
```
###### Packages for processing CSS files:
- css-loader (tells webpack how to handle CSS files just like babel-loader)
- mini-css-extract-plugin (optimizes how CSS is delivered to browser allow better caching and performance)
- cssnano (removes whitespace, comments, and unnecessary code, optimizes selectors and reduces file size)
- postcss-loader (PostCSS is to CSS as Babel is to JS)
- autoprefixer (Enable PostCSS to automatically add vendor prefixes to our css (webkit & moz))
#### Image and Font Files Processing:
```html
<!-- Vanilla HTML -->
<img src="./images/logo.png" alt="Logo">

<!-- With webpack -->
<img src="<%=require('./images/logo.png')%>" alt="Logo">
```
<%= %> - lodash templating language https://lodash.com/docs/4.17.15#template
###### Importing pictures with webpack:
```javascript
// we can now import images
// webpack will add the correct paths to the variables
const jordanImage = new URL("./images/jordan.jpg", import.meta.url);
const jamesImage = new URL("./images/james.jpg", import.meta.url);
const bryantImage = new URL("./images/bryant.jpg", import.meta.url);

const whoIsTheGoat = [
  // replace original paths with variables
  { name: "Michael Jordan", image: jordanImage },
  { name: "Lebron James", link: jamesImage },
  { name: "Kobe Bryant", link: bryantImage },
];
```
## Asynchronous Programming:
#### Asynchronous Operations:
<span class="blue-text-bold">Synchronous code</span> - Javascript needs to wait for a loop to finish executing before moving on. 
```javascript
// Simulating asynchrony
function brewCoffee() {
  setTimeout(() => {
    console.log("Coffee is ready!");
  }, 3000); // 3000 milliseconds or 3 second
}

function cookEggs() {
  setTimeout(() => {
    console.log("Eggs are ready!");
  }, 2000); // 2000 milliseconds or 2 seconds
}

// start brewing coffee
brewCoffee();
// start cooking eggs right away, don't wait for the coffee to be ready
cookEggs(); 
```
#### Callbacks:
<span class="red-text-bold">A callback is basically a function passed to a function that will be run whenever the function deems necessary</span>
```javascript
// this function will be passed as a callback and executed if the tweetContainer isn't found
function createTweet(tweet) {
  const newTweetContainer = document.createElement("div");
  newTweetContainer.textContent = tweet;
  document.body.append(newTweetContainer);
}

// add a third parameter, which is a callback
function insertTweet(tweet, containerSelector, callback) {
    const tweetContainer = document.querySelector(containerSelector);
    // If the right container isn't found, let's create it
    if (!tweetContainer) {
		callback(tweet); 
        // callback is a function passed through the insertTweet
        // argument
        return;
    }

    tweetContainer.textContent = tweet;
}

// the call will look like this:
insertTweet("a reply to Marissa Mayer's tweet", ".tweets", createTweet);
```
#### Asynchronous Callbacks:
HTML image element has 2 built in properties:
- <span class="blue-text-bold">onload</span> - a function with an event that will be called when the image loads
- <span class="blue-text-bold">onerror</span>  - a function with an event that will be called when the image cannot load
```javascript
// This callback needs to be executed after the image is loaded.
function imageLoadCallback(evt) {
  // After the image is loaded, add the image element to the DOM.
  document.body.append(evt.target);
}

// Now we specify a second parameter for the onload callback.
function loadImage(imageUrl, loadCallback) {
  const img = document.createElement("img");
  img.src = imageUrl;

  // Assign loadCallback to the image's onload property.
  img.onload = loadCallback;
}

// Now the image will only be added to the page after
// the image is loaded.
loadImage(
  "https://yastatic.net/q/logoaas/v1/Practicum.svg",
  imageLoadCallback
);
```
#### Timers:
<span class="blue-text-bold">Timers</span> - Special functions that allow us to delay the execution of code by telling the browser to wait some time before running the code
<span class="blue-text-bold">setTimeout()</span> - Executes code after a certain time has elapsed
<span class="blue-text-bold">setInterval()</span> - Sets a repeating timer which allows you to run a callback function multiple times at a specified time interval
```javascript
function showMessage(message) {
    console.log(message);
}

let timer = setTimeout(showMessage, 10000, "10 seconds have passed since the page was loaded"); 

/* 10 seconds (or 10 thousand milliseconds) after,
a message will appear in the console. */ 

// clears the timer
clearTimeout(timer);
```
```javascript
function checkEmail() {
    // Here is the code for checking new emails
}

// The inbox will refresh automatically every 10 seconds
let interval = setInterval(checkEmail, 10000);

// If the user has switched tabs, a blur event fires on the window
window.addEventListener("blur", function () {
  // cancel the timer
    clearInterval(interval); 
});

// If the user has returned
window.addEventListener("focus", function() {
    // start the timer again
    interval = setInterval(checkEmail, 10000);
});
```
#### The Event Loop:
```javascript
console.log("first log");

setTimeout(function () {
  console.log("second log");
}, 1);

console.log("third log");
// Logs: first log, third log, second log
```
<span class="red-text-bold">Browser time delays are not exact. Treat them as scheduling hints rather than precise time guarantees</span>

<span class="blue-text-bold">call stack</span> - A stack of all the instructions that sequentially get called
```javascript
/* Warning: if you run this code in the console,
your browser tab will freeze for several minutes. Don't do this if
you don't want to wait until the code stops running. */

console.log("This message will be the first to appear in the console");

setTimeout(function () {
  console.log("When do you think this message will be logged?");
}, 1);

for (let i = 0; i <= 1000000; i++) {
  console.log("This message will be logged one million and one times");
}
```
Only after the call stack is empty will the queued timeout callback is added to the callstack and executed
#### Promises:
```javascript
// create a promise
const newPromise = new Promise(function(resolve, reject) {
  /* whether a promise will resolve or reject will be determined by logic
  that has been written inside the promise's executor. In this example   
  we are simply randomly determining whether to resolve or reject */
    const rand = Math.random() > 0.5 ? true : false;

    if (rand) {
        resolve("Request processed successfully");
    } else {
        reject("Request rejected");
    }
});

newPromise
  .then(function (value) { // executed if the promise has been resolved

    /* the value parameter stores the value passed to 
    the resolve() method when creating the promise, i.e. 
    the string "Request processed successfully" */
      console.log(value);
  })
  .catch(function (value) { // executed if the promise has been rejected

    /* in this case, the value parameter stores the value
    passed to the reject() method, i.e. 
    the string "Request rejected" */
      console.log(value + ". We are sorry for the inconvenience.");
  })
  .finally(function () { // executed in either case
      console.log("We promise we got your request");
  });
```
###### Promise Chaining:
You can chain multiple then() and catch() methods
```javascript
const newPromise = new Promise(function (resolve, reject) {
    resolve("One Mississippi"); // immediately get the resolved promise
});

function firstAction(value) {
  /* the value parameter will receive what we passed 
  to the resolve() method when creating the promise, 
  i.e. the string "One Mississippi" */

    return `${value}, two Mississippi`;
}

function secondAction(value) {
  /* the value will be equal to the value returned 
  by the previous then() method, i.e. the string "One Mississippi, two Mississippi" */

    return `${value}, three Mississippi`;
}

function thirdAction(value) {
    console.log(value);
}
// It will continue down the .then() train unless theres an error
newPromise.then(firstAction).then(secondAction).then(thirdAction);

/* we'll see the following in the console: "One Mississippi, two Mississippi, three Mississippi" */
```
```javascript
const newPromise = new Promise(function (resolve, reject) {
    reject("One Mississippi"); // immediately get the rejected promise
});

function firstAction(value) {
    return `${value}, two Mississippi`;
}

function secondAction(value) {
    return `${value}, three Mississippi`;
}

function thirdAction(value) {
    console.log(value);
}

newPromise
  .then(firstAction)
  .then(secondAction)
  .catch(thirdAction);

/* the console log: "One Mississippi" -- because newPromise was rejected,
and we immediately got to the catch() call */
```
Catching errors
```javascript
const newPromise = new Promise(function (resolve, reject) {
    resolve("1.0");
});

newPromise
  .then(function (value) {
    // this line throws a TypeError because strings don't have toFixed() method
    return value.toFixed(0);
  })
  // this then() won't run because we got a TypeError in the previous one
  .then(function (value) {
    console.log(value);
  })
  // without this catch, our code will crash with an "Uncaught (in promise)" error
  .catch(function (error) {
    console.log(error);
  });
```
<span class="red-text-bold">Always end a chain of promises with a catch() call</span>
###### Promise Static Methods:
<span class="blue-text-bold">Promise.resolve()</span> - Create a promise and change its state to resolved
<span class="blue-text-bold">Promise.reject()</span> - Create a promise and change its state to reject
```javascript
Promise.resolve("This promise has been resolved")
  .then(function (value) {
    console.log(value); // "This promise has been resolved"
  });

Promise.reject("This promise has been rejected")
  .catch(function (value) {
    console.log(value); // "This promise has been rejected"
  });
```
<span class="blue-text-bold">Promise.all()</span> - Takes an input array with promises and executes the code written in then() only when the state of each promise is resolved. If one promise is rejected it will return the rejected promise and we have to add catch() call to handle it
```javascript
// create the first promise
const picturePromise = new Promise((resolve, reject) => {
  if (someCondition) {
    resolve("Picture");
  } else {
    reject();
  }
});

// create the second promise
const textPromise = new Promise((resolve, reject) => {
  if (secondCondition) {
    resolve("Text");
  } else {
    reject();
  }
});

// create an array of promises
const promises = [picturePromise , textPromise ]

// pass the array of promises to the Promise.all() method
Promise.all(promises)
  .then((results) => {
    console.log(results); // ["Picture", "Text"]
  })
  .catch((error) => {
    console.log(error);
  });
```
#### Extra Promise Code Snippets:
```javascript
function waitForClick() {
  return new Promise(resolve => {
    document.addEventListener('click', () => resolve('good'), { once: true });
  });
}

waitForClick().then(value => console.log(value));
```

```javascript
let resolveNew;
const newPromise = new Promise((resolve) => { resolveNew = resolve; });

newPromise.then(value => console.log(value));

document.addEventListener('click', () => {
  resolveNew?.('good'); // call resolve when clicked
});
```

```javascript
function getPromise() {
  if (turnOn) return Promise.resolve('good');
  return new Promise(resolve => {
    document.addEventListener('click', () => resolve('good'), { once: true });
  });
}

getPromise().then(v => console.log(v));
```
## Working with APIs:
#### HTTP: Hypertext Transfer Protocol:
<span class="green-text">We can think of computers and servers as exchanging letters. The computer "requests" to send a letter. The server can process the request or return it to sender if something's wrong</span>
Browser requests can be divided into four categories:
1. HTML document requests
2. Images, CSS, JS, and other file requests
3. Default form submission, accompanied by a page reload
4. JavaScript code requests
###### Request Format:
- The HTTP method. This defines the operation that will be performed. There are several such methods, the most popular of which are the GET and POST methods. The first one is used when getting data from the server, while the second one is used to send data.
- The path to the resource. This is the part of the web address after the website's name. Take a look at `example.com/hello`, in this case, the path is `/hello`.
- The version of the HTTP protocol that is used to send the request, e.g. HTTP/1.1.
- Request headers. They can be used to transfer additional information to the server.
- The request body. However, not all requests have this. For example, the body of a POST request is the data being sent.
![[10. Request Format.png|800]]
###### Response Format:
- The HTTP version.
- An HTTP status code. For example, we might get "200 OK" if everything went swimmingly. If things didn't go so well, we might get back "404 Not Found," an infamous response which is received whenever the requested resource wasn't found.
- Headers providing additional information for the browser.
- The response body. For example, when you visit yandex.com, the HTML code of this page will be contained in the response body.
![[11. Response Format.png|650]]
#### Making Requests with JavaScript:
<span class="blue-text-bold">fetch()</span> - Creates a server request then returns a response. First argument is URL (required) second argument is an options object (optional)
<span class="blue-text-bold">options objects</span> - method headers and body
```javascript
fetch("https://example.com/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    username: "arthur"
  })
});
```
<span class="red-text-bold">The fetch() method is asynchronous. When we call it, it creates a promise.</span>
<span class="red-text-bold">When an answer is received the response from the server is written to the promise which we can use then() and catch()</span>
```javascript
fetch("https://example.com")
  .then((res) => {
    console.log(res); // if everything is ok, we receive the response
  })
  .catch((err) => {
    console.error("Error. The request failed");
  });
```
#### Advanced Promise Patterns:
###### Promise Chaining:
<span class="red-text-bold">One of the most powerful features of promises is chaining. When you return a value from a .then() handler, it becomes the input for the next .then() in the chain</span>
```javascript
function fetchUser(id) {
  return fetch(`/api/users/${id}`).then(response => response.json());
}

function fetchUserPosts(userId) {
  return fetch(`/api/users/${userId}/posts`).then(response => response.json());
}

// Chain the operations
fetchUser(123)
  .then(user => {
    console.log("User:", user.name);
    return fetchUserPosts(user.id); // Return a promise
  })
  .then(posts => {
    console.log("Posts:", posts.length);
  })
  .catch(error => {
    // Errors from the entire chain are processed here
    console.error("Something went wrong:", error);
  });
```
###### Promise.resolve() and Promise.reject():
```javascript
fetch("/some-url")
  .then((res) => {
    if (res.ok) {
      // handle success
    }
    return Promise.reject(`Error: ${res.status}`
  })
  .catch(console.error)
```
###### Promise.all():
```javascript
const promise1 = fetch('/api/users');
const promise2 = fetch('/api/posts');
const promise3 = fetch('/api/comments');

Promise.all([promise1, promise2, promise3])
  .then(responses => {
    // All three requests have completed
    console.log("All requests finished!");
    // responses is an array with results in the same order
    return Promise.all(responses.map(r => r.json()));
  })
  .then(data => {
    const [users, posts, comments] = data;
    console.log("Users:", users.length);
    console.log("Posts:", posts.length);
    console.log("Comments:", comments.length);
  })
  .catch(error => {
    // If ANY promise fails, this catch runs
    console.error("One of the requests failed:", error);
  });
```
#### JSON Format:
JSON keys need to be enclosed in double quotes
Key values can be strings, numbers, bools, null, objects, and arrays
No functions variables or comments

<span class="blue-text-bold">JSON.stringify()</span> - Makes a JSON string out of an object
<span class="blue-text-bold">JSON.parse()</span> - Converts a JSON string into a Javascript Object
```javascript
const songs = [
  {
    title: "ALOHAnet",
    artist: "Palmbomen II"
  },
  {
    title: "The Other Stranger",
    artist: "Doxa Sinistra"
  },
  {
    title: "Ariadna",
    artist: "Kedr Livansky"
  }
];

const songsJSON = JSON.stringify(songs);

console.log(songsJSON);
/* [
     {"title":"ALOHAnet","artist":"Palmbomen II"},
     {"title":"The Other Stranger","artist":"Doxa Sinistra"},
     {"title":"Ariadna","artist":"Kedr Livansky"}
] */
console.log(typeof songsJSON); // "string"

const songsObject = JSON.parse(songsJSON); 

console.log(typeof songsObject); // "object" 
console.log(songsObject[0].title); // "ALOHAnet"
```
<span class="blue-text-bold">res.json()</span>
```javascript
fetch("https://example.com")
  .then((res) => { // res is a response object
    return res.json(); // return the method's result and call the then() callback
  })
  .then((data) => {
    console.log(data.user.name); // if this callback is called, the data is an object
  })
  .catch((err) => {
    console.error("Error. Request failed");
  });
```
<span class="red-text-bold">.json() is an asynchronous method the code below will fail</span>
```javascript
fetch("https://example.com")
  .then((res) => {
    const data = res.json();
    console.log(data.user.name); // the data is a promise, so it's not ready yet!
  })
  .catch((err) => {
    console.error("Error. Request failed");
  });
```
<span class="red-text-bold">Promise chaining needs to return a promise. If it returns a value then it will convert it to a resolved promise</span>

<span class="green-text">You can use JSON.parse(JSON.stringify(object)) to make a deep copy</span>
#### HTTP Requests:
- GET - Retrieving Data (default)
- POST - Sends data
- PUT - Completely updates a resource
- PATCH - Partially updates a resource
- DELETE - Deletes a resource
###### GET:
```javascript
fetch("https://example.com/images/random?type=portrait", {
method: "GET"
});

// below is a GET request, even though we're not explicitly stating so in the fetch() method

fetch("https://example.com")
  .then((res) => {
    return res.json();
  })
  .then((data) => {
      console.log(data.user.name); // if this then() block is executed, the data is an object
  })
  .catch((err) => {
    console.error("Error. The request has failed: ", err);
  });
```
###### POST:
```javascript
fetch("https://example.com", {
  method: "POST",
  body: JSON.stringify({
    name: "Paul",
    age: 30
  })
});
```
<span class="blue-text-bold">MIME</span> - Multipurpose Internet Mail Extensions (Written in the Content-Type)
```
text/plain
text/html
text/javascript
text/css
image/jpeg
image/png
audio/mpeg
audio/ogg
video/mp4
application/json # Transmitting data in JSON format
application/octet-stream
application/x-www-form-urlencoded #Encoding form to be sent via url
multipart/form-data # Sending files to server (imgs, etc..)
```
<span class="red-text-bold">Headers are not case-sensitive</span>
<span class="green-text">Here's an example of a complete POST request</span>
```javascript
const userToAdd = {
  name: "Elise",
  email: "elise@mail.com",
};

fetch("https://jsonplaceholder.typicode.com/users", {
  method: "POST",
  body: JSON.stringify(userToAdd),
  headers: {
    "Content-Type": "application/json",
  },
})
  .then((res) => {
    return res.json();
  })
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.log(err);
  });
```
#### Server Responses:
Each server response always has a status code. 
- The server can reject a request
- Redirect it
- Return an error message
First digit of a status code:
- 2 - Request has succeeded
- 3 - Redirected
- 4 - Something wrong with the request
- 5 - Server has internal error
Common response statuses:
- 200 - OK
- 401 - Unauthorized
- 403 - Forbidden
- 404 - Not Found
- 500 - Internal Server Error

```javascript
fetch("https://se-quotes-api.onrender.com/v1/quotes/random")
    .then(res => {
    console.log(res.status, res.statusText); // 200 OK
  });
```
Boolean property ok:
```javascript
fetch("https://se-quotes-api.onrender.com/v1/quotes/random")
  .then(res => {
    console.log(res.ok); // true
  });
```
Catching response fails:
```javascript
const quoteElement = document.querySelector("div.quote");

fetch("https://se-quotes-api.onrender.com/v1/quotes/random")
    .then((res) => {
    if (res.ok) {
      return res.json();
    }
    /* if the server returns an error,
       reject the promise and move to the catch block */
      return Promise.reject(`Something went wrong: ${res.status}`);
  })
  .then((data) => {
    quoteElement.textContent = data.quote;
  })
  .catch((err) => {
    console.error(err); // "Something went wrong: ..."
  });
```
###### Response Header:
Requests can also have headers containing additional info from the server
```javascript
fetch("https://se-quotes-api.onrender.com/v1/quotes/random")
  .then((res) => {
    console.log(res.headers); // headers {}
  });
```
<span class="blue-text-bold">get()</span> - A special function for header objects to retrieve key values
```javascript
fetch("https://se-quotes-api.onrender.com/v1/quotes/random")
  .then((res) => {
  // Not case-sensitive
    if (res.headers.get("Content-Type").includes("application/json")) {
      return res.json();
    }
  });
```
Header objects also have more methods at https://developer.mozilla.org/en-US/docs/Web/API/Headers
<span class="red-text-bold">Response headers are read only</span>
- `res.json()` — converts the JSON-encoded body of a response into a JavaScript object.
- `res.text()` — converts the response body into text.
- `res.blob()` — returns the response body as binary data. It's often used to exchange various files: videos, images, PDF docs. This method starts a stream of decoded data, that's received gradually.
These all read response bodies asynchrously
```javascript
/* methods of parsing the response body return a promise,
and they need to be used asynchronously */ 
fetch("https://se-quotes-api.onrender.com/v1/quotes/random")
  .then(res => res.json())
  .then((result) => {
    console.log(result);
  });

fetch("https://se-quotes-api.onrender.com/v1/quotes/random")
  .then(res => res.text())
  .then((result) => {
    console.log(result);
  });
```
#### API Class Example:
```javascript
class RecipeApi {
  constructor({ baseUrl, headers }) {
    this._baseUrl = baseUrl;
    this._headers = headers;
  }

  _handleServerResponse(res) {
    if (res.ok) {
      return res.json();
    }
    return Promise.reject(`Error: ${res.status}`);
  }

  getRecipes() {
    return fetch(`${this._baseUrl}/recipes`, {
      headers: this._headers,
    }).then(this._handleServerResponse);
  }

  addRecipe({ title, ingredients, instructions }) {
    return fetch(`${this._baseUrl}/recipes`, {
      method: "POST",
      headers: this._headers,
      body: JSON.stringify({
        title,
        ingredients,
        instructions,
      }),
    }).then(this._handleServerResponse);
  }
}
```
```javascript
function handleRecipeSubmit(evt) {
  evt.preventDefault();

  // Get the form data from the inputs
  const recipeData = {
    // ...
  };

  // Call the API method and pass it the form data
  // The passed object must have the shape that is
  // expected by the addRecipe() method.
  api.addRecipe(recipeData)
    .then(newRecipe => {
      console.log("Recipe added:", newRecipe);
      // Update your UI with the new recipe
      // Clear the form
      // etc...
    })
    .catch(error => {
      console.error("Failed to add recipe:", error);
    });
}
```
## Object Prototypes:
#### The \_\_proto__ Property:
<span class="green-text"> Every JS object has the __proto__ property, which allows one object to access properties of another object</span>
```javascript
const obj1 = {
  messageSpanish: "¡Hola Mundo!"
};

const obj2 = {
  messageEng: 'Hello, World!'
};

obj2.__proto__ = obj1; // Links the spanish to obj2

console.log(obj2.messageEng); // "Hello, World!" - the messageEng property
console.log(obj2.messageSpanish); // "¡Hola Mundo!" — the messageSpanish property was also found

// Objects can also be linked in a chain
const obj3 = {
  messageSpanish: "¡Hola Mundo!"
};

const obj4 = {
  messageEng: "Hello, World!"
};

const obj5 = {
  messageFrench: "Bonjour le Monde!"
};

obj4.__proto__ = obj3;
obj5.__proto__ = obj4;

console.log(obj5.messageFrench); // "Bonjour le Monde!" - found in obj5
console.log(obj5.messageEng); // "Hello, World!" — wasn't found in obj5, but was accessed  through the __proto__ link in obj4 
console.log(obj5.messageSpanish); // "¡Hola Mundo!" — wasn't found in obj5 and also through the __proto__ link in obj4, but was accessed through obj4.__proto__, or in obj3
```
object.\_\_proto__ property stores the object.prototype
obj3 is a prototype of the obj4 object

<span class="red-text-bold">It's best to not rewrite the __proto__ property directly as it can impact performance</span>
#### The Object.create() method:
<span class="blue-text-bold">Object.create</span> - Creates a new empty object. The first argument is the object that is to act as the prototype of the new object
```javascript
// CODE THAT REWORKS THIS DOWN BELOW
// the createSong function creates objects
function createSong(title, artist) {
  const newSong = {
    title,
    artist,
    isLiked: false,
    // each of these objects contains the like() function
    // This function is seperate on each object. We want to combine them to all be the same
    like: function () {
      newSong.isLiked = !newSong.isLiked;
    }
  };

  return newSong;
}

// we can create several songs using one function
const song1 = createSong("Chanel", "Frank Ocean");
const song2 = createSong("Circles", "Mac Miller");
const song3 = createSong("Until I Walk Through the Flames", "Wicca Phase Springs Eternal");
```
<span class="red-text-bold">---------------------------------------------------------------------</span>
```javascript
//  The prototype newSong object
const songPrototype = {
  like: function () {
    this.isLiked = !this.isLiked;
  }
};

function createSong(title, artist) {
  // Create an empty object with the prototype
  const newSong = Object.create(songPrototype);

  // Add all the necessary properties to the object
  newSong.title = title;
  newSong.artist = artist;
  newSong.isLiked = false;

  // Return the song object
  return newSong;
}

const song1 = createSong("Chanel", "Frank Ocean");
const song2 = createSong("Circles", "Mac Miller");
const song3 = createSong("Until I Walk Through the Flames", "Wicca Phase Springs Eternal");

console.log(song1); // { title: "Chanel" ... }
console.log(song2); // { title: "Circles" ... }
console.log(song1.like === song2.like); // true
```
#### The new Operator:
<span class="red-text-bold">Any JS function can be called with the new operator</span>
```javascript
const song = new createSong("Circles", "Mac Miller");
```
1. Before executing code, the engine creates a new empty object and assigns it to the value of `this` inside the function being called.
2. The engine then executes the function code.
3. Finally, `this` is returned.
```javascript
// FUNCTION TO REWRITE / REFACTOR
function createSong(title, artist) {
  // 1. Create an empty object with a prototype
  const newSong = Object.create(songPrototype);

  // 2. Add the necessary values
  newSong.title = title;
  newSong.artist = artist;
  newSong.isLiked = false;

  // 3. Return the object
  return newSong;
}
```
<span class="red-text-bold">------------------------------------------------------------------------</span>
```javascript
// Name it as Song so people know to use "new" Should be capitalized
function Song(title, artist) {
  // this = {}; — The engine will do it itself

  // Add necessary values to this
  this.title = title;
  this.artist = artist;
  this.isLiked = false;

  // return this; —  The engine will deal with it
}

Song.prototype = songPrototype;

const song = new Song("Circles", "Mac Miller");

console.log(song);
```
#### The prototype Property:
```javascript
// We get an error because our songPrototype isn't being used
const songPrototype = {
  like: function () {
    this.isLiked = !this.isLiked;
  }
};

function Song(title, artist) {
  this.title = title;
  this.artist = artist;
  this.isLiked = false;
}

const song1 = new Song("Chanel", "Frank Ocean");

console.log(song1.like); // undefined — this property doesn't currently exist in the object or the prototype
song1.like(); // error — undefined - it's not a function
```
<span class="red-text-bold">Functions are objects too. A constructor function also has its own prototype property</span>
```javascript
function Song(title, artist) {
  this.title = title;
  this.artist = artist;
  this.isLiked = false;
}

// add the like() function to the Song.prototype object
Song.prototype.like = function () {
  this.isLiked = !this.isLiked;
};

const song1 = new Song("Chanel", "Frank Ocean");

/* now the like() function is in the prototype again
 and is available in new objects */

console.log(song1.isLiked); // false
song1.like();
console.log(song1.isLiked); // true
```
<span class="blue-text-bold">__proto__</span> - Belongs to an object. Tells engine where to look for a property if there's no property in that object
<span class="blue-text-bold">prototype</span> - prototype property is connected to a constructor function. Tells engine which object the \_\_proto__ link leads to
#### The Secret Behind Javascript Classes
<span class="red-text-bold">Classes are an easy-to-use tool for writing constructor functions</span>
```javascript
// Create the Song class
// CLASS EQUIVILANT TO CONSTRUCTOR FUNCTIONS
class Song {
  constructor(title, artist) {
    // In the class contrustor, modify this
    this.title = title;
    this.artist = artist;
    this.isLiked = false;
  }

    // Add methods to the class, which will appear in the object prototype
  // that such class will create
  like() {
    this.isLiked = !this.isLiked;
  }
}

// Create an object using the Song class
const song1 = new Song("Chanel", "Frank Ocean");
```
#### Built-in Constructors and Their Prototypes:
###### The \_\_proto__ property of built-in objects:
Arrays prototype contains built-in methods
```javascript
const arr2 = [1, 2, 3];

console.log(arr2.__proto__ === Array.prototype); // true, because the __proto__ property refers to Array.prototype

const obj = {};
const num = 4;
const str = "Hello";
const bool = true;
const func = function () {
  console.log("Hello world!");
};

console.log(obj.__proto__ === Object.prototype); // true
console.log(num.__proto__ === Number.prototype); // true
console.log(str.__proto__ === String.prototype); // true
console.log(bool.__proto__ === Boolean.prototype); // true
console.log(func.__proto__ === Function.prototype); // true

console.log(String.prototype.__proto__ === Object.prototype); // true
console.log(Number.prototype.__proto__ === Object.prototype); // true
console.log(Boolean.prototype.__proto__ === Object.prototype); // true
console.log(Function.prototype.__proto__ === Object.prototype); // true
console.log(Array.prototype.__proto__ === Object.prototype); // true
```

![[12. Object Heirarchy.png|500]]
<span class="green-text">__proto__ is basically base class</span>
#### Inheritance and the Prototype Chain:
