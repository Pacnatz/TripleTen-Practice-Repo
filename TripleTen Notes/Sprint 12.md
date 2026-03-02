## Server-Side Engineering with Node.js
#### How node.js works:
<span class="blue-text-bold">DSL</span> - Domain Specific Langauge
<span class="blue-text-bold">GPL</span> - General Purpose Language
What is node.js made of?
- A javascript engine
- A set of modules
<span class="blue-text-bold">server</span> - A remote computer that can receive requests and return responses.
![[17. Node.js map.png]]
```bash
node index.js # This allows you to run code outside of the web browser
```
#### Node.js Servers:
<span class="blue-text-bold">NIC</span> - Network interface card
Each NIC has multiple entry points known as "ports" 1-65,535
<span class="blue-text-bold">The request object</span> - usually called req. All information about the request is stored in this object
<span class="blue-text-bold">The response object</span> - usually called res. This object contains properties and methods for working with a response
```javascript
// run this file and navigate to
// http://localhost:3000/hello
// in the browser

const http = require('http');

const server = http.createServer((req, res) => {
  // req object
  console.log(req.url); // /hello
  console.log(req.method); // GET
  console.log(req.headers); // the request headers will be here
  console.log(req.body); // and the request body will be here. But the GET request doesn't have it
  
  // res object
  res.statusCode = 200; // the response's status code 
  res.statusMessage = 'OK'; // the content of the message 
  res.setHeader('Content-Type', 'text/plain'); // add a header to the response 
  res.write('Hello, '); 
  // send the first part of the message - the "Hello, " string 
  res.write('world!'); 
  // send the second part of the message - the "world!" string 
  res.end(); 
  // end the response
  res.write('world!'); // an error will occur 
  
  // writeHead() to pass both the response's status code and the header at the same time
  res.writeHead(200, {'Content-Type': 'text/html; charset=utf8'});
  // we can also pass data to the end() method
  res.end('<h1>Hello, World!</h1>', 'utf8');
});

server.listen(3000);
```
###### Using environment variables:
```bash
NODE_ENV=production node index.js
```
<span class="green-text">In the scripts</span>
```javascript
if (process.env.NODE_ENV !== 'production') {
  console.log('Code executed in development mode');
}
```
<span class="green-text">Environment variables are written with capital letters and spaced with an underscore</span>
```bash
PORT=3000 node app.js #Sets an env variable port to 3000 and then runs a node server
```

```javascript
const { PORT = 3000 } = process.env;
server.listen(PORT);
```
#### Request Body - Streams: 
<span class="blue-text-bold">requests are processed as follows: data is broken down into smaller packages, each of which is treated as a separate event that needs to be processed asynchronously. We install an event handler that listens for packages, and whenever one arrives, it's added to the others that came before it. In doing things this way, we collect the entire request, package by package.</span>

<span class="blue-text-bold">Stream</span> - A sequence of data coming from some source such as a network for file system. Data in a stream is transmitted in chunks of 64KB. To get the entire request body, you need to merge all the chunks together

In Node.js, **HTTP request bodies arrive in chunks**, not all at once.
###### The event model:
<span class="blue-text-bold">on()</span> - How you listen to events on an incoming HTTP request
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  let data = '';

  // data event is triggered when a data package is received
  req.on('data', (chunk) => {
    data += chunk.toString();
  });
  // end event is triggered when the last package arrives
  req.on('end', () => {
    console.log(JSON.parse(data));
  });
});

server.listen(3000);
```
###### The url and method request properties:
<span class="blue-text-bold">req.url</span> - The path and query/hash parameters (but without the protocol and hostname)
<span class="blue-text-bold">req.method</span> - The HTTP method (in all caps) of the request:
```json
{
  url: '/songs/submit',
  method: "POST", 
}
```
```javascript
const server = http.createServer((req, res) => {
  if (req.url === '/songs' && req.method === 'GET') {
    console.log("GET the list of songs!");
  }
})
```
###### Attributes for the form element :
<span class="green-text">When submitting a regular HTML form submission there are attributes that change how form data is sent that goes to the server. We need to make sure to set them up so the request goes to the correct route on your server </span>
- `action` — The URL/path to send the request to, e.g. `"/submit"`
- `method` — The HTTP method to use when submitting the form, e.g. `"get"`, `"post"`
    - Note that `"put"` and `"delete"` are [not supported here](https://softwareengineering.stackexchange.com/a/211790).
    - It's standard to use lowercase in the HTML here, even though they will appear as uppercase when read on the server.
- `enctype` — The encoding type of the request body, e.g. `"text/plain"`
    - Default is `"application/x-www-form-urlencoded"`
    - This changes the format of the text sent to the server.
    - Can only be changed from the default if `method="post"`
```html
<form action="/submit" method="post" enctype="text/plain">
  ...
</form>
```
###### API Headers:
- ✅ `application/json` → for API data (most MERN apps)
    
- ✅ `text/html` → for web pages
    
- ✅ `text/plain` → simple text
    
- ✅ `multipart/form-data` → file uploads
    
- ✅ `application/x-www-form-urlencoded` → classic forms
    
- ✅ media types → files (images, pdf, etc.)
```javascript
const todos = [];

const server = http.createServer((req, res) => {
  if (req.url === '/submit' && req.method === 'POST') {
    // POST lets you have a data stream that you can read
    let body = '';

    req.on('data', (chunk) => {
      body += chunk;
    });

    req.on('end', () => {
      todos.push(body.split('=')[1]);
      console.log(todos);
      res.writeHead(200, {
        'Content-Type': 'text/html'
      });

      res.end(submitSuccessMarkup);
    });
  }

  if (req.url === '/' && req.method === 'GET') {
    res.writeHead(200, {
      'Content-Type': 'text/html'
    });

    res.end(mainPageMarkup);
  }
});

server.listen(PORT);
```
#### Node.js Modules:
<span class="blue-text-bold">http</span> - A built-in Node.js module 
```javascript
const http = require('http');
```
###### Importing modules:
```javascript
// index.js
const utils = require('./utils'); // path to the module
// since all modules have the .js extension, we can omit that part when importing the module
const someFunction = utils.someFunction;
const someValue = utils.someValue;
const { someFunction, someValue } = require('./utils');
```
###### Exporting modules:
```javascript
// utils.js
module.exports.someFunction = () => {
  console.log('I was exported!');
};

module.exports.someValue = 42;

// Alternative way
module.exports = {
  someFunction,
  someValue
};
```
###### To use ES6 modules in Node.js:
To use ES6 modules in Node.js, specify the type of your project in the package.json file by adding a `"type"` field with the value `"module"`.

Another way to use ES6 modules by default is to create files with the `.mjs` extension. Regardless of the value of the `"type"` field, `.mjs` files are always treated as ES modules

#### Working with File Systems:
<span class="blue-text-bold">fs module</span> - A module that comes with node.js that allows us to access and manipulate the file system
<span class='blue-text-bold'>readFile()</span> - Takes the name of the file, an options object (optional), and a callback
###### Reading a file:
```javascript
const fs = require('fs');

fs.readFile('data.json', { encoding: 'utf8' }, (err, data) => {
  if (err) {
    console.log(err);
    return;
  }
  // If options object
  console.log('data: ', data);
  // If no options object
  console.log('data: ', data.toString('utf8'));
});
```
###### Reading a directory:
```javascript
const fs = require('fs');

fs.readdir('.', (err, files) => {
  if (err) {
    console.log(err);
    return;
  }

  console.log('data: ', files);
});
```
###### Creating a folder:
```javascript
const fs = require('fs');

fs.mkdir('incomingData/data', (err) => {
  if (err) console.log(err);
});
```
###### Writing data to a file:
```javascript
const fs = require('fs');
// Will create a file if the file doesn't exist
fs.writeFile('data.json', JSON.stringify([1, 2, 3]), (err) => {
  if (err) console.log(err);
});
```
###### Deleting files using fs:
```javascript
const fs = require('fs');

fs.unlink('data.json', (err) => {
  if (err) {
    console.log(err);
    return;
  }

  console.log('The file was deleted!'); 
});
```
###### Promises with Files:
```javascript
const fsPromises = require('fs').promises;

fsPromises.readFile('data.json', { encoding: 'utf8' })
  .then((data) => {
    console.log(data);
  })
  .catch(err => {
    console.log(err);
  });
```
<span class="red-text-bold">File paths are determined at the root of your project not relative to the directory of your script</span>
###### Modifying a file path with the path module:
```javascript
// read-file.js

const fs = require('fs');
const path = require('path');

module.exports.readFile = () => {
  // joining the path segments to create an absolute path
  const filepath = path.join(__dirname, 'file.txt'); 
  const file = fs.readFile(filepath, { encoding: 'utf8' }, (err, data) => {
    console.log(data);
  }); 
};
```
#### Streams for Reading and Writing Files:
- **Readable**: streams from which data can be read
- **Writable**: streams to which data can be written
- **Duplex**: streams that are both readable and writable
- **Transform**: a subtype of duplex streams that can modify or transform the data while it is being read and written
```javascript
const fs = require('fs');

const reader = fs.createReadStream('./in.txt', { encoding: 'utf8' });
const writer = fs.createWriteStream('./out.txt', { encoding: 'utf8' });

reader.on('data', (data) => {
  writer.write(data);
});

reader.on('end', (data) => {
  writer.end();
});

// add an event listener to the error event
reader.on('error', (err) => {
  console.log(err);
});
```
<span class="red-text-bold">Using streams is more performant on memory because readFile and writeFile has to read the whole file</span>
```javascript
// pipe makes it so you don't have to close streams
const fs = require('fs');

const reader = fs.createReadStream('./in.txt', { encoding: 'utf8' });
const writer = fs.createWriteStream('./out.txt', { encoding: 'utf8' });

reader.pipe(writer);
```
#### Debugging your Node.js App:
```bash
// To begin a node program normally
node path/to/myProgram.js

// To run the program in debug mode
node inspect path/to/myProgram.js
```

- `cont`, `c`: Continue execution
- `next`, `n`: Step next
- `step`, `s`: Step in
- `out`, `o`: Step out
- `pause`: Pause running code
- `watch(var)`: watches a variable value
#### Server Testing - Postman:
<span class="blue-text-bold">Postman</span> - An application that lets you test GET POST PATCH PUT DELETE requests for your APIs
## Express.js 101:
#### Creating an Express App:
```bash
npm init # Creates a package.json
npm i express # Installs express
npm i nodemon -D # Installs nodemon
```
```JSON
{
  "scripts": {
    "dev": "nodemon index.js"
  }
}
```
```javascript
const express = require('express');
// listen to port 3000
const { PORT = 3000 } = process.env;

const app = express();

app.listen(PORT, () => {
  // if everything works fine, the console will show which port the application is listening to
  console.log(`App listening at port ${PORT}`);
})
```
#### How the Client and the Server Communicate:
<span class="blue-text-bold">get()</span> - Express method to handle GET requests
- A string which represents a server path
- A callback handler, which is fired if the server receives that request and the route matches
###### Response object:
<span class="blue-text-bold">Response object</span> - Has a send() method that sends the response (such as the markup)
Same thing as res.end() with node.js
```javascript
app.get('/', (req, res) => {
  res.send(
    `<html>
    <body>
      <p>Response to the signal from distant space</p>
    </body>
    </html>`
  );
});
```
<span class="blue-text-bold">send()</span> - Can accept different types of arguments: markup, text, JSON and will automatically set the Content-Type header for you
```javascript
app.get('/', (req, res) => {
  // Changing the status
  res.status(404).send('<h1>Page not found</h1>');
});
```
###### Request object: 
```javascript
const pokemon = [
  {
    type: 'fire',
    stage: 1,
    name: 'Charmander'
  },
  {
    type: 'fire',
    stage: 2,
    name: 'Charmeleon'
  },
  {
    type: 'fire',
    stage: 3,
    name: 'Charizard'
  }
]; 

GET `${BASE_PATH}/pokemon?type=fire&stage=3`; // This is a query parameter "Charizard"
```
<span class="red-text-bold">req.query returns the query parameters of the URL</span>
**Use req body When**:
- Identifying one specific resource
- It's required to find the resource
- It represents the identity of the resource
**Use query params When**:
- You are filtering, sorting, or searching a list
- They are optional
- You might combine many of them
- Order doesn't matter
```javascript
app.get('/pokemon', (req, res) => {
  // make a mutable copy of our pokedex
  let result = pokemon;

  // filter out all pokemon that aren't of the desired type
  if (req.query.type) {
    result = result.filter(item => item.type === req.query.type)
  }

  // filter out all pokemon that aren't of the desired stage
  if (req.query.stage) {
    result = result.filter(item => item.stage === req.query.stage)
  }

  res.send(result);
});
```
`http://example.com/path?key1=value1&key2=value2`
- `?` - Starts the query string
- `&` - Separates multiple key-value pairs
- Everything after `?` is optional
`http://localhost:3000/search?term=react&page=2`
- `/search` - route/path
- `term=react` - first query parameter
- `page=2` - second query parameter
#### Routing with Express:
###### Dynamic Routes:
```javascript
app.get('/users/:id', (req, res) => {
  res.send(req.params);
  /* 
    upon sending a request to "http://localhost:3000/users/123",
    this JSON-object will appear in req.params: { "id": "123" } 
  */
     
});

app.get('/users/:id/albums/:album/:photo', (req, res) => {
  const { id, album, photo } = req.params;
  /* 
    when requesting "http://localhost:3000/users/123/albums/333/2"
    the request parameters will be written like this:
    {"id":"123","album":"333","photo":"2"}
    we assigned them to the id, album, and photo consts
  */ 

  res.send(`We're on a user's page with the id of ${id}, looking through album #${album}, photo #${photo}`); 
});
```
###### Divide this code into modules:
<span class="red-text-bold">Router is like a mini-application that handles a group of related routes</span>
```javascript
// index.js

// here, is the entry point setup
const express = require('express');

const { PORT = 3000 } = process.env;
const app = express();

// here we have data
const users = [
  { name: 'Jane', age: 22 },
  { name: 'Hugo', age: 30 },
  { name: 'Juliana', age: 48 },
  { name: 'Vincent', age: 51 }
];

// here's where we'll do our routing
app.get('/users/:id', (req, res) => {
  if (!users[req.params.id]) {
    res.send(`This user doesn't exist`);

    // it's important we don't forget to exit from the function
    return;
  }

  const { name, age } = users[req.params.id];
  
  res.send(`User ${name}, ${age} years old`);
});

app.listen(PORT, () => {
    console.log(`App listening on port ${PORT}`);
});
```
First things first, let's move our data into an individual file called `db.js`:
```javascript
// db.js

module.exports = {
  users: [
    { name: 'Jane', age: 22 },
    { name: 'Hugo', age: 30 },
    { name: 'Juliana', age: 48 },
    { name: 'Vincent', age: 51 }
  ]
};
```
Routing logic should also be moved into an individual file
<span class="blue-text-bold">Router()</span> - Creates a new router object we can attach our handlers to (since app can only be created once)
```javascript
// routes.js

const router = require('express').Router(); // creating a router
const { users } = require('./db.js'); // since this data is necessary for routing, we need to import it

router.get('/users/:id', (req, res) => { 
  if (!users[req.params.id]) {
    res.send(`This user doesn't exist`);
    return;
  }

  const { name, age } = users[req.params.id];
  
  res.send(`User ${name}, ${age} years old`);
});

module.exports = router; // exporting the router
```
<span class="red-text-bold">app.use() connects middleware</span>
<span class="blue-text-bold">use()</span> - A method to execute routers
- Base URL
- The router itself
```javascript
// index.js 

const express = require('express');
const router = require('./routes.js'); // importing the router

const { PORT = 3000 } = process.env;
const app = express();

app.use('/', router); // starting it

app.listen(PORT, () => {
    console.log(`App listening on port ${PORT}`);
});
```
#### Middleware:
```bash
curl http://localhost:3000/users/1 # GET request like postman
curl -X "PATCH" http://localhost:3000/users/1 # PATCH request
```

<span class="blue-text-bold">middleware</span> - A function that takes the repetitive code from each route. (Basically a refactor function)
- Execute during the lifecycle of a server request
- Write our request processing code inside a separate module
<span class="red-text-bold">Middleware needs to send a response and return with send() OR it need to continue to the next function next()</span>
```javascript
router.get('/users/:id', (req, res) => {
  // request processing begins here
  const { id } = req.params;
  // We can refactor this function so it works for get post delete put patch
  if (!users[id]) {
    res.send({ error: `This user doesn't exist` });
    return;
  }

  const { name, age } = users[req.params.id];
  // the request is sent. request processing ends here
  res.send(`User ${name}, ${age} years old`);
});
```
###### Creating a middleware function:
```javascript
// check whether the user exists
const doesUserExist = (req, res, next) => {
  if (!users[req.params.id]) {
    res.send(`This user doesn't exist`);
    return; // stop the function nothing else will happen
  }

  next(); // call the next function (sendUser)
};

// move the code for the server's response into a separate function
const sendUser = (req, res) => {
  const { name, age } = users[req.params.id];
  res.send(`User ${name}, ${age} years old`);
};

router.get('/users/:id', doesUserExist); // pass the middleware here
router.get('/users/:id', sendUser); // Handlers are the parts that use res.send()
// Can also be written as this
router.get('users/:id', doesUserExist, sendUser)
```
###### Connecting Middleware:
<span class="red-text-bold">If you want a global middleware you'll need the use() method</span>
```javascript
// index.js

const express = require('express');
const routes = require('./routes.js');

const { PORT = 3000 } = process.env;
const app = express();

const logger = (req, res, next) => {
  console.log('The request has been logged!');
  next();
};

app.use(logger); // This will run for ALL routes
app.use("/api", logger); // This will run for API route and anything after
app.use("/api", timeLog, handler); // ONLY for 1 route
app.use('/', routes);

app.listen(PORT, () => {
  console.log(`App listening on port ${PORT}`);
});
```
#### Parsing a Request Body:
<span class="red-text-bold">Express can parse a request body easier than vanilla node.js</span>
```javascript
app.use(express.json());
app.use(express.urlencoded({ extended: true })); // For vanilla form submissions

// Express can do this instead of the req.on('data', chunk => {body += chu})
app.post("/users", (req, res) => {
  console.log(req.body); // already parsed object
});

```
AJAX form submissions is the one that uses e.preventDefault()
#### Serving HTML and Static Files with Express:
<span class="blue-text-bold">express.static(__dirname)</span> - A middleware that sends a response in the form of a file / resource

<span class="red-text-bold">If express finds a file it will send a response if not it will continue with next()</span>
```javascript
// This will send a response for all the files within the public dir
app.use(express.static(path.join(__dirname, 'public'))); 
// now the client only has access to public files
```
```
GET /
// Returns public/index.html

GET /style.css
// Returns public/style.css
```
```javascript
app.use('/users/:id', checkRequest); // ✅ runs for all methods
router.get('/users/:id', doesUserExist); // ✅ only runs for GET
router.put('/users/:id', updateUser);   // ✅ only runs for PUT
```
## Building a REST API:
#### What is REST?
<span class="blue-text-bold">REST</span> - Representational State Transfer
- Easier to adapt an application to various platforms
- Possible to create public APIs
- Easier and faster to build and test server software
###### Constraints of REST:
1. **Client - Server Separation** - Client is responsible for making request to server. Server is responsible for storing and sending data
2. **Stateless** - Each request must contain all the information the server needs
3. **Uniform Interface** - The API has a standard and consistent way to access that interface
	- **Resource identification in requests** - The request identifies the resource that it wants
	- **Resource manipulation through representations** - The data sent back representing the resource contains enough info to modify or delete that resource
	- **Self-descriptive messages** - Response should include info about how to parse it
	- **Hypermedia as the engine of application state (HATEOAS)** - When requesting a resource, data sent back includes links to other related resources.
4. **Layered System**  - We can use multiple APIs to create layered systems.
5. **Cacheable** - Response data can be cached. This means data is stored on the client to later be retrieved and used.
6. **Code-on-demand** - A server can extend a client's functionality using server code.
#### HTTP Methods:
<span class="blue-text-bold">URI</span> - Uniform Resource Identifier, an address similar to a URL
```
https://test.nomoreparties.co/cards/5d1f0611d321eb4bdcd707dd
# This URI points to a resource that is addressed using its id
```
<span class="blue-text-bold">HEAD</span> - Head method is used the same way as GET but HEAD allows us to get a response header without the response body
<span class="blue-text-bold">OPTIONS</span> - Options method is used to describe which HTTP methods the server supports
###### Handling HTTP requests in Express:
```javascript
app.get('/books', getBooks);
app.post('/books', createBook);
app.put('/books/:id', replaceBook);
app.patch('/books/:id', updateBookInfo);
app.delete('/books/:id', deleteBook);

// or

router.get('/books', getBooks);
router.post('/books', createBook);
router.put('/books/:id', replaceBook);
router.patch('/books/:id', updateBookInfo); // The updateBookInfo() function will be called only if the PATCH method is called on the corresponding URI
router.delete('/books/:id', deleteBook);
```
#### Rest Resource Naming Rules:
<span class="blue-text-bold">resource</span> - Document, image, blog post, social network user, anything that can be accessed with an HTTP request
###### Basic naming guidelines:
**Plural** names if they are a collection of resources
**Singular** nouns are less common and used for individual documents
**Verbs** when we are naming `controller` resources
```
https://nomoreparties.co/users
https://nomoreparties.co/cards
https://nomoreparties.co/users/{user-id}/profile
https://nomoreparties.co/users/{user-id}/cart/checkout // Checkout verb
```
<span class="red-text-bold">Use hyphens in a URI since URI can't use spaces</span>
```
// do this:

GET /users/{user-id}/user-devices

// and not this:

GET /users/{user-id}/user_devices
```
<span class="red-text-bold">URIs are case sensitive. We always use lowercase characters</span>
<span class="red-text-bold">Don't reference the HTTP Method name</span>
```
// do this: 
GET /users POST /users

// and not this:

GET /get-users

POST /create-user
```
#### Server Response Status Codes:
- **1xx: Informational response** - Indicates that the request was received by the server and the client must wait for the final response
- **2xx: Success response** - The request was successful!
- **3xx: Redirection** - The request has been redirected to another server and client needs to take some further action to complete the request
- **4xx: Client Error** - Error on the client side. Request wasn't generated correctly or the client doesn't have the required access rights
- **5xx: Server Error** - Something has broken or the server is overloaded
###### Common Status Codes:
**200 OK:** This request means the request was successful. A response with this status must contain a body. Most often, this status code is used when responding to a GET request on a resource.

**201 Created:** This means that a resource was successfully created on the server. For example, this is a valid code to send upon creating a new blog post.

**202 Accepted:** The server has started processing the request but hasn't finished it yet. This status is used to respond to requests that take a long time to process, for example, when processing a large amount of data.

**301 Moved Permanently:** The API has been modified and the resource was moved to a new URI. This is indicated in the "Location" header of the server response.

**302 Found:** The request must be redirected to a different URI. In the Location header, the server should send a new URI. When a response is received, the browser will automatically send a request to the new URL.

**400 Bad Request:** This represents a client-side error. For example, we might see this error if the request was malformed. This is a general status. It is sent when no other 4xx status fits the context.

**401 Unauthorized:** The request requires authorization, but the corresponding authorization headers are missing or malformed.

**403 Forbidden:** The request is correct, but the client doesn't have the rights to complete it. For example, this can happen when a client tries to delete someone else's post.

**404 Not Found:** This means a resource could not be found. For example, when trying to find a user and the requested id doesn't exist. This is probably the most recognizable code for many people.

**405 Method Not Allowed:** The requested resource doesn't support the HTTP method that was used to make the request.

**500 Internal Server Error:** This is a generic status for server-side errors. This isn't the client's fault.

**501 Not Implemented:** The resource is on the server, but the request method is not supported by the server.
###### Express response Status:
```javascript
// this is ok

res.status(404);
res.send({ message: 'User not found' });

// but this is better

res.status(404).send({ message: 'User not found' });
```
#### Response Caching:
GET requests are cached by browsers.
POST requests are not cached by default
PUT, PATCH, DELETE requests are not cached at all
###### Caching Headers:
<span class="blue-text-bold">Expires</span> - Expires header indicates a time frame over which cached data remain valid. When this time ends, the data will again be requested from the server.
```
Expires: Fri, 20 May 2016 19:20:49 IST
```
<span class="blue-text-bold">Cache</span> - A temporary storage of data that can be quickly retrieved without having to recompute or fetch it from the original source
<span class="blue-text-bold">Cache-Control</span> - Tells clients and CDNs how to store, reuse, or revalidate responses
```
// cache control

Cache-Control: only-if-cached // If the response is in the cache, it will be returned from there. If not, the browser will respond with a 504 error

Cache-Control: no-cache // The browser makes a conditional validation request to the server. If there are changes on the server, the cache will be updated.

// max-age management

Cache-Control: max-age=30000 // the maximum time in seconds during which a resource is considered relevant

// cache update management

Cache-Control: must-revalidate // if the cache has expired, a request will be sent to check the data in the cache

// other

Cache-Control: no-store // cache disabled

Cache-Control: private // data can only be cached on the client side
Cache-Control: public // data can be cached on proxy servers

Cache-Control: proxy-revalidate // before using data from the proxy server cache, you should check if it's relevant

Cache-Control: no-transform // proxies can't apply any changes to the resource. other settings — private, public and proxy-revalidate — don't impose such restrictions

Cache-Control: max-age=30000, must-revalidate
```
<span class="blue-text-bold">ETag</span> - The ETag header contains a hash. A hash is a string from an algorithm that analyzes the cache
If the response body changes, so does the hash
```
ETag: "abcd1234567n34jv"
```
<span class="blue-text-bold">Last-Modified</span> - Stores the date that the data was last updated on the server
```
Last-Modified: Fri, 10 May 2016 09:17:49 IST
```
###### Express cache headers:
By default, express works as follows:

- Creates an `ETag` response header that is sent in every server response. Thus, every response is cached.
- The browser remembers the value of the header and at the next `GET` request it sends the `ETag` inside another header — `If-None-Match`.
- If the value of the `If-None-Match` header matches some cache on the server, the response will be under the `304` Not Modified status code. As a result, the browser will take the response value from its cache.
#### CORS:
<span class="blue-text-bold">CORS</span> - Cross-Origin Resource Sharing. Security feature implemented by browsers to restrict how resources on a web page can be requested by another domain
<span class="green-text">http://example.com cannot fetch data from http://api.anotherdomain.com unless the API server allows it via CORS</span>
###### Installation:
```
npm install cors
```
###### Allow all CORS request:
```javascript
const express = require('express');

// (1) Import the package
const cors = require('cors');

const app = express();

// (2) Enable CORS for all origins
app.use(cors());

app.get('/api', (req, res) => {
  res.json({ message: 'CORS is enabled for all origins!' });
});

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```
###### Restricting access with a whitelist:
```javascript
// (1) Declare an array of origins to allow access to.
const allowedOrigins = [
  "https://my-website.com",
  "http://my-webiste.com",
  "http://localhost:3000", // Use the port your frontend is served on
];

// (2) Pass a configuration object to the middleware.
app.use(cors({ origin: allowedOrigins }));
```
## Intro to Databases: 
#### Types of Databases - Relational and Non-Relational:
<span class="blue-text-bold">Relational Databases</span> - organize data into tables which can be linked together

**Stock**

| ID    | Name                          | Price | Amount |
| ----- | ----------------------------- | ----- | ------ |
| 19960 | Hermione Granger's wand       | 10    | 15     |
| 79789 | Delorean DMC-12               | 500   | 7      |
| 51339 | Luke Skywalker's lightsaber   | 15    | 3      |
| 16307 | Daenerys Targaryen's necklace | 20    | 2      |
**Customers**

|ID|Name|Age|
|---|---|---|
|85334|Selina ~Catwoman~ Kyle|23|
|48222|Nicolas Cage as John Travolta|55|
|87130|John Travolta as Nicolas Cage|65|
|17185|Lisa Simpson|8|
**Orders**

|ID|User_ID|Item_ID|
|---|---|---|
|1|48222|19960|
|1|48222|79789|
|1|48222|51339|
|2|48222|19960|
|3|17185|51339|
|3|17185|16307|
Will search stock table to select the Item_IDs that were ordered by the user in the Orders table
<span class="green-text">Relational databases take up a lot of RAM and power the bigger it is</span>
<span class="blue-text-bold">Non-Relational Databases</span> - Use collections, which are non-rigid data structures consisting of documents
"madeOrder" is an optional parameter that makes this not rigid
```json
{
    "id": "1234252",
    "name": "Yasmin Palmer",
    "madeOrder": true
},
{
    "id": "1208772",
    "name": "Misha Luna Jimenez",
}
```
Discount example that gives a discount if a person made a previous purchase
```json
// Stock document
{
    "id": "19960",
    "name": "Hermione Granger's wand",
    "price": 10,
    "amount": 15
},
{
    "id": "79789",
    "name": "Delorean DMC-12",
    "price": 500,
    "amount": 7
},
{
    "id": "51339",
    "name": "Luke Skywalker's lightsaber",
    "price": 15,
    "amount": 3
},
{
    "id": "16307",
    "name": "Daenerys Targaryen's necklace",
    "price": 20,
    "amount": 2
}
```
```json
// Сustomers document 
{
    "id": "85334",
    "name": "Selina ~Catwoman~ Kyle",
    "age": 23
},
{
    "id": "48222",
    "name": "Nicolas Cage as John Travolta",
    "age": 55
},
{
    "id": "87130",
    "name": "John Travolta as Nicolas Cage",
    "age": 65
},
{
    "id": "17185",
    "name": "Lisa Simpson",
    "age": 8
}
```
```json
// Orders document
{
    "id": "39439",
    "user": {
        "id": "48222",
        "name": "Nicolas Cage as John Travolta",
        "age": 55
    },
    "items": [
        {
            "id": "19960",
            "name": "Hermione Granger's wand",
            "price": 10,
            "amountOrdered": 2
        },
        {
            "id": "79789",
            "name": "Delorean DMC-12",
            "price": 500,
            "amountOrdered": 1
        },
        {
            "id": "51339",
            "name": "Luke Skywalker's lightsaber",
            "price": 15,
            "amountOrdered": 1
        }
    ]
},
{
    "id": "48241",
    "user": {
            "id": "17185",
            "name": "Lisa Simpson",
            "age": 8
        },
    "items": [
        {
            "id": "16307",
            "name": "Daenerys Targaryen's necklace",
            "price": 20,
            "amountOrdered": 1
        },
        {
            "id": "51339",
            "name": "Luke Skywalker's lightsaber",
            "price": 15,
            "amountOrdered": 1
        }
    ]
},
```
Now we don't need to look up and combine information from various tables. All the information is in the Orders document.
<span class="green-text">NoSQL lacks consistent data and standardization</span>
<span class="red-text-bold">Use NoSQL if the data isn't structured (such as a note that contains photos, text, and links) and is working with big sets of data</span>
#### Installing MongoDB:
NoSQL Databases:
- RavenDB
- Cassandra
- MongoDB
- Redis
- BigTable
[https://www.mongodb.com/docs/v7.0/tutorial/install-mongodb-on-windows/#install-mongodb-community-edition](https://www.mongodb.com/docs/v7.0/tutorial/install-mongodb-on-windows/#install-mongodb-community-edition)
#### MongoDB Compass:
<span class="blue-text-bold">MongoDB Compass</span> - Lets you create, view, and edit databases and their contents using a graphical interface
<span class="blue-text-bold">Database Name</span> - Name of the data base
<span class="blue-text-bold">Collection Name</span> - A folder for a list of of documents e.g. users, stock, orders
**ADD DATA button**:
- Import JSON or CSV
- Insert Document
<span class="blue-text-bold">_id</span> - Represents a unique identifier that is created by MongoDB for each document
<span class="blue-text-bold">ObjectID</span> - $oid is a representation of an object ID
```json
{
  "_id": {
    "$oid": "671acd8ee0f128fc95749c82"
  },
  "name": "Elise",
  "age": 38
}
```
<span class="green-text">To filter you need to write a query structured as an object e.g. { name: "Jane" }</span>
```
// Find all the documents in the collection
{}

// Find all documents that match both the name and age field
{ name: 'Elise', age: 38 }
 
// Find all documents with age > 30
// For >=, use $gte instead
{ age: { $gt: 30 }}

// Find all documents with age < 30
// For <=, use $gte instead
{ age: { $lt: 30 }}
```
#### Connecting to MongoDB from Javascript via Mongoose:
**Installing Mongoose**:
```bash
# Install mongoose with major version 8
npm i mongoose@^8.9.5
```
**Connecting to a MongoDB Server**:
```javascript
// app.js — input file

const express = require('express');
const mongoose = require('mongoose');

const app = express();

// connect to the MongoDB server
mongoose.connect('mongodb://127.0.0.1:27017/mydb');
//address of the mongodb server along with the name of our database
// It's preferable to use 127.0.0.1

// connect the middleware, routes, etc...

app.listen(3000);
```
#### Schemas and Models:
<span class="blue-text-bold">Schema</span> - A set of rules for your data. Sets the number of fields a record has, the length of each field value, which characters are allowed etc..
<span class="green-text">MongoDB doesn't support schemas by default but you can add them via Mongoose</span>
**Creating a Schema:**
```js
// models/user.js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: { // every user has a name field, the requirements for which are described below:
    type: String, // the name is a string
    required: true, // every user has a name, so it's a required field
    minlength: 2, // the minimum length of the name is 2 characters
    maxlength: 30, // the maximum length is 30 characters
  },
  pronouns: {
    type: String, // the pronouns are a string
    enum: ['they/them', 'she/her', 'he/him', 'other pronouns'] // every user can choose their pronouns
  },
  about: String, // type: String
  // String, Number, Boolean, ARray, Int32 are SchemaTypes
});
```
Every user has:
 - A name between 2-30 characters long
 - pronouns that can take one of the 4 assigned values
 - about property which is a string
<span class="red-text-bold">Validator is an npm package you need to install before using</span>
<span class="blue-text-bold">validation property</span> - An object which has a validator and a message
<span class="blue-text-bold">validator</span> - A validation function. It returns a boolean value
<span class="blue-text-bold">message</span> - An error message. it is rendered if the validator function returns false
```js
const userSchema = new mongoose.Schema({
  age: { // every user has an age field
    type: Number, // the age type is a number
    required: true, // the user has to specify their age
    validate: { // describe the validate feature
      validator(v) { // validator is a data validation feature
        return v >= 18; // if the age is less than 18, it will
      },
      message: 'Sorry. You have to be 18 years old', // Will be displayed
    }
  }
})
```
**Subdocuments:**
If you use a schema as a property of your schema, you'll have to use the `Schema` method twice
```js
// models/user.js

const mongoose = require('mongoose');

// create a schema for the "Pet" document 
const petSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    minlength: 2,
    maxlength: 30,
  },
  age: Number
});
// once the schema is ready, pass it to the property which should correspond to this template:
const userSchema = new mongoose.Schema({
  ...
  pet: petSchema // describe the pet property with this schema
});
```
**Array Properties:**
Arrays are used to store multiple data items of the same type
```js
// models/user.js

const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  ...
  hobbies: [{ // describe the schema for a single element and put it in square brackets
    type: String,
    minlength: 2,
    maxlength: 30,
  }]
});
```
```js
hobbies: [ "hello", "world" ]
// This field will store multiple values all following the same rules
// Each item in hobbies must follow the schema inside the brackets
// Allows you to store more than one hobby object per user
```
**Creating a Model Based on a Schema:**
<span class="blue-text-bold">model</span> - A wrapper around the schema. Lets us read, add, delete, and update documents
```js
// models/user.js

const mongoose = require('mongoose');
// Describe the schema:
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    minlength: 2,
    maxlength: 30,
  },
  about: String,
});

// This connects it to the "users" collection in MongoDB
const User = mongoose.model('user', userSchema);
// Use the model
const user = new User({ name: "Alice", about: "Test"});
await user.save();
// User is the model
// user is a document

```
- Pass the name of the model (singular noun)
- Pass the schema which will describe future documents
Schema -> Model -> Documents
```js
// Models give you crud operations
User.find()
User.findById(id)
User.findOne({ name: "Alice" })
User.create({ name: "Bob" })
User.updateOne(...)
User.deleteOne(...)
```
#### Creating, Reading, Updating, and Deleting Documents:
- Create: `create()`
- Read: `findById()`, `findOne()`, `find()`
- Update: `findByIdAndUpdate()`, `findOneAndUpdate()`, `updateOne()`, `updateMany()`
- Delete: `findByIdAndRemove()`, `findOneAndRemove()`, `delete()`, `deleteMany()`
**Creating Documents:**
```js
// routes/users.js

/*
    code for creating routers, etc.
*/

// import the model
const User = require('../models/user');

router.post('/', (req, res) => {
  const { name, about } = req.body; // get the name and description of the user

  User.create({ name, about }) // create a document based on the input data 
    // returns the recorded data
    .then(user => res.send({data: user}))
    // data not recorded, prompting an error
    .catch(err => res.status(500).send({ message: 'Error'}));
});
```
**Reading Documents:**
<span class="blue-text-bold">findById()</span> - Use \_id property to find a specific document
```js
// routes/users.js

/*
    code for creating routers, etc.
*/

const User = require('../models/user');

router.get('/:id', (req, res) => {
  User.findById(req.params.id)
    .then(user => res.send({ data: user }))
    .catch(err => res.status(500).send({ message: 'Error' }));
});
```
<span class="blue-text-bold">findOne()</span> - returns the first document that matches the req parameters
```js
// find the first match with a field that equals "Elise Taylor"
User.findOne({ name: 'Elise Taylor' });
```
<span class="blue-text-bold">find()</span> - Same as findOne but it returns all documents that match the req parameters
```js
// find all 30-year-old users
User.find({ age: 30 });

// find all users
User.find({});
```
**Updating Documents:**
<span class="blue-text-bold">findByIdAndUpdate()</span> - First arg accepts an identifier in string, Second arg is an object with the properties that need to be updated
```js
  // routes/users.js

  // ...

  const User = require('../models/user');

  router.patch('/:id', (req, res) => {
    // updating the name of the user found by _id
    User.findByIdAndUpdate(req.params.id, { name: 'Henry George' })
      .then(user => res.send({ data: user }))
      .catch(err => res.status(500).send({ message: 'Error' }));
  });
```
<span class="blue-text-bold">findOneAndUpdate()</span> - Matches the first object and updates it
```js
  // find the first match with the name field that equals to "Sam Taylor" and replace is with "Henry George"
  User.findOneAndUpdate({ name: 'Sam Taylor' }, { name: 'Henry George' }));
  // find all matches with the name field that equals to "Sam Taylor" and replace it with "Henry George"
  User.updateMany({ name: 'Sam Taylor' }, { name: 'Henry George' });
```
<span class="red-text-bold">The .then parameter is the original document before the update</span>
```js
User.findByIdAndUpdate(req.params.id, { name: 'Henry George' })
  // user here means the document before the update
  .then(user => res.send({ data: user }));
```
To change this value we can add an options object
```js
User.findByIdAndUpdate(
  req.params.id,
  { name: 'Henry George' },
  // pass the options object:
  {
    new: true, // the then handler receives the updated entry as input
    runValidators: true, // the data will be validated before the update 
    upsert: true // if the user entry wasn't found, it will be created
  })
  .then(user => res.send({ data: user }))
  .catch(user => res.send({ message: 'Data validation failed or another error occured.' }));
```
**Deleting Documents:**
<span class="blue-text-bold">findByIdAndRemove()</span> - Finds the identifier and deletes it- :
```js
// routes/users.js

// ...

const User = require('../models/user');

router.delete('/:id', (req, res) => {
  User.findByIdAndDelete(req.params.id)
    .then(user => res.send({ data: user }))
    .catch(err => res.status(500).send({ message: 'Error' }));
});
```
<span class="blue-text-bold">findOneAndRemove()</span> - Deletes the first match
```js
// delete a user with a specific name
User.findOneAndRemove({ name: 'Sam Taylor' });
```
<span class="blue-text-bold">deleteMany()</span> - Deletes all matches
```js
// delete all 30-year-old users
User.deleteMany({ age: 30 });
```
#### Code Structuring. Controllers:
```js
// routes/users.js

const User = require('../models/user');

router.get('/:id', (req, res) => {
  // search the database
  User.findById(req.params.id)
    // return the found data to the user
    .then(user => res.send({ data: user }))
    // if the record was not found, display an error message
    .catch(err => res.status(500).send({ message: 'Error' }));
});

router.post('/', (req, res) => {
  const { name, about } = req.body;

  User.create({ name, about })
    .then(user => res.send({ data: user }))
    .catch(err => res.status(500).send({ message: 'Error' }));
});

// and so on
```
We can separate them into a separate files. A controller file and a routes file.
```js
// controllers/users.js
// this file is the user controller

const User = require('../models/user');

// the getUser request handler
module.exports.getUser = (req, res) => {
  User.findById(req.params.id)
    .then(user => res.send({ data: user }))
    .catch(err => res.status(500).send({ message: 'Error' }));
};

// the createUser request handler
module.exports.createUser = (req, res) => {
  const { name, about } = req.body;

  User.create({ name, about })
    .then(user => res.send({ data: user }))
    .catch(err => res.status(500).send({ message: 'Error' }));
};
```
```js
// routes/users.js
// this is the routes file
    
const { getUser, createUser } = require('../controllers/users');

// route definitions
router.get('/:id', getUser)
router.post('/', createUser);
```
<span class="blue-text-bold">Controller</span> - A collection of request handler functions, while routes defines the routes itself
#### Relationships Between Schemas:
1. Building a Relationship between two schemas
Imagine an app has 2 entities users and ads
```js
const userSchema = new mongoose.Schema({
  name: { // the user only has a name
    type: String,
    minlength: 2,
    maxlength: 20,
    required: true,
  },
});

module.exports = mongoose.model('user', userSchema);
```
```js
const adSchema = new mongoose.Schema({
  title: {
    type: String,
    minlength: 2,
    maxlength: 20,
    required: true,
  },
  text: {
    type: String,
    minlength: 2,
    required: true,
  },
  // add the creator field 
  creator: { 
    type: mongoose.Schema.Types.ObjectId, // Links the _id
    ref: 'user', // Assign ref to the name of the modal we are linking
    required: true 
  },
  likes: { // Make an array of types
    type: [{ type: mongoose.Schema.Types.ObjectId, ref: "user" }],
    default: [],
  },
});

module.exports = mongoose.model('ad', adSchema);
```
<span class="green-text">type: ObjectId will reference another _id from mongoDB and will need a ref property</span>
2.  Including the \_id field during document creation
```js
// controllers/ads.js

const Ad = require('../models/ad');

module.exports.createAd = (req, res) => {
  const { title, text, creatorId } = req.body;

  Ad.create({ title, text, creator: creatorId })
    .then(ad => res.send({ data: ad }));
};
```
creator: creatorId is required because creatorId is not a shorthand for creator
3. Acquiring Complete Information via the populate() method:
<span class="blue-text-bold">populate()</span> - Replaces an ObjectID with the actual referenced document
```js
// controllers/ads.js

const Ad = require('../models/ad');

module.exports.getAds = (req, res) => {
  Ad.find({}) // Finds all ads in database
    .populate('creator') // Gets the creator field and references it
    .then(ad => res.send({ data: ad }));
};
```
<span class="red-text-bold">populate(['creator', 'item']) populates multiple fields</span>
shorthand for:
```js
.populate({ path: 'creator' })
.populate({ path: 'item' })
```
#### Introduction to SQL:
<span class="blue-text-bold">Structured Query Language</span> - Most common computer language for managing data in relational databases
<span class="blue-text-bold">statement (aka query)</span> - A request written according to SQL syntax
**Select:**
Takes the selection of one or many rows or columns from a table
```sql
-- Select columns from the table
SELECT 
    Name,
    Price,   
FROM 
    Stock;
```
<span class="blue-text-bold">select</span> - specifies the necessary columns
<span class="blue-text-bold">from</span> - specifies the table from which data should be taken
**Stock**

|**ID**|**Name**|**Price**|**Amount**|
|---|---|---|---|
|19960|Hermione Granger's wand|10|15|
|79789|Delorean DMC-12|500|7|
|51339|Luke Skywalker's lightsaber|15|3|
|16307|Daenerys Targaryen's necklace|20|2|
**Stock**

|**Name**|**Price**|
|---|---|
|Hermione Granger's wand|10|
|Delorean DMC-12|500|
|Luke Skywalker's lightsaber|15|
|Daenerys Targaryen's necklace|20|
<span class="red-text-bold">To select all columns from the table, add * to SELECT operator</span>
<span class="blue-text-bold">where</span> - A condition to filter results
```sql
SELECT 
    *
FROM 
    Stock
WHERE 
    Price < 100; -- Defining the condition of row selection
```
**Stock**

|**ID**|**Name**|**Price**|**Amount**|
|---|---|---|---|
|19960|Hermione Granger's wand|10|15|
|51339|Luke Skywalker's lightsaber|15|3|
|16307|Daenerys Targaryen's necklace|20|2|
**PostgreSQL: Using Relational Databases in Express.js Apps:**
<span class="blue-text-bold">DBMS</span> - database management system e.g. PostgreSQL
```bash
npm i pg-promise #Installing postgres
```
<span class="blue-text-bold">pg-promise</span> - A PostgreSQL client for Node.js
```js
var pgp = require('pg-promise')(/* options */)
var db = pgp('postgres://username:password@host:port/database') //connect to your local databasw

db.one('SELECT $1 AS value', 123) //create a query to pull desired data
  .then(function (data) {
    console.log('DATA:', data.value)
  })
  .catch(function (error) {
    console.log('ERROR:', error)
  })
```
## Error Handling:
#### How Errors Work in Javascript:
```js
// Standard error class
throw new Error("look out below");
```
**Structure of a JS error:**
- message - contains error text
- stack - contains the sequence of function calls that led to the error
- name - contains the name of the error
Error class doesn't let us specify names when creating the object but you can set it later
```js
let err = new Error("Object not found");
err.name = "NotFoundError";
console.log(err.name);
```
#### Handling Errors in JS:
**Error handling in synchronous code:**
We use `try...catch` statements
**Error handling in asynchronous code: async/await:**
```js
// Moved the code returning the promise with an error to an external function
function returnPromiseError() { 
  return Promise.reject(new Error("Something went wrong...")); 
}

(async function testAsyncAwaitError() {
  try {
    console.log("Function execution started");
    await returnPromiseError(); // wait till returnPromiseError() is executed
  } catch (err) {
    console.error(`${err.name} with the message ${err.message} has occured, but we've handled it`);
  }
  console.log("Function execution completed successfully");
})();
```
**Error handling in promises: the .catch handler:**
If an error occurs when executing code in a promise. The error goes to the nearest .catch block
```js
function returnPromiseError() { 
  return Promise.reject(new Error("Error. Something went wrong..."));
}

(function testPromiseRejectHandler() {
  returnPromiseError();
  .catch((err) => {
    console.error(`Error ${err.name} with the message ${err.message} has occurred while executing the code, but we've handled it`);
  });
})();
```
**console.error:**
console.error is useful for backend because it can show connection errors or errors related to specific routes / middleware
**Error handling and Mongoose: the orFail() helper:**
<span class="red-text-bold">orFail() converts a null result into an error</span>
```js
Card.find({ _id: "507f1f77bcf86cd799439011" }) // some nonexistent ID
  .then((cardData) => {
    // incorrectly sends `null` back to the client with a 200 status!
    res.send(cardData);
  })
  .catch((error) => {
    // does not run because no error was thrown
  });
```
```js
Card.find({ title: "nonexistant card" })
  .orFail() // throws a DocumentNotFoundError
  .then((cardData) => {
    res.send(cardData); // skipped, because an error was thrown
  })
  .catch((error) => {
    // now this does run, so we can handle the error and return an appropriate message
  });
```
```js
// Create a custom method to the orFail method
.orFail(() => {
  const error = new Error("No card found with that id");
  error.statusCode = 404;
  throw error; // Remember to throw an error so .catch handles it instead of .then
})
```
#### Global Error Handlers and Custom Errors:
<span class="blue-text-bold">Global Error Handlers</span> - Mechanism to deal with errors that can go unhandled
- subscribe to uncaughtException event in process module
```js
const process = require('process');

process.on('uncaughtException', (err, origin) => {
  console.log(`${origin} ${err.name} with the message ${err.message} was not handled. Pay attention to it!`);
});

// Throw a synchronous error
throw new Error(`The missed error`);
```
**Defining custom errors in JS:**
```js
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
    this.statusCode = 400;
  }
}
```
#### Approaches to Determine the Error Type:
- Using the name of an error (stored in the name field)
- Using the error class with the `instanceof` operator
**The main rules for error handling:**
```js
...
} catch (err) {
  if (check_condition_for_error_1) {
    // Logic for handling error 1    ...
    return;
  }

  if (check_condition_for_error_2) {
    // Logic for handling error 2
    ...
    return;
  }

  // Logic for handling unknown errors
  // In this example we only log them
  console.log(`An unknown error ${err.name} has occurred: ${err.message}`);
}
```
1. At the end of the catch block you must describe logic for handling unknown errors
2. At the end of each block that handles an error add an execution stop with return to exit
**Determining the error type using its name:**
```js
...
} catch (err) {
  // Check the name of the error
  if (err.name === "ErrorName") { 
    // Describe the logic for handling the error
    ...
    return;
  }
  ...
} 
```
**Hierarchy of errors: the instanceof operator:**
```js
class SomeError extends Error {};
class AnotherError extends Error {};
class ChildOfAnotherError extends AnotherError;

let someError = new SomeError("SomeError")
let anotherError = new AnotherError("AnotherError")
let childOfAnotherError = new ChildOfAnotherError("Child error of AnotherError")

// This will return true because someError is an instance of SomeError
console.log(someError instanceof SomeError)

// This will return false because anotherError is an instance of AnotherError, not SomeError
console.log(anotherError instanceof SomeError)

// This will return true, childOfAnotherError is an instance of ChildOfAnotherError
// and ChildOfAnotherError is inherited from AnotherError
console.log(childOfAnotherError instanceof AnotherError)

// This will return true, childOfAnotherError is an instance of ChildOfAnotherError
console.log(childOfAnotherError instanceof ChildOfAnotherError)
```
<span class="red-text-bold">Inherited errors need to be caught first</span>
```js
class ErrorGroup extends Error {};
class FirstChildOfErrorGroup extends ErrorGroup;
class SecondChildOfErrorGroup extends ErrorGroup;

try {
....
} catch( err) {
  // Separate handler for FirstChildOfAnotherError
  if (err instanceof FirstChildOfErrorGroup) { ... }

  // Handler for a group of errors
  if (err instanceof ErrorGroup) { ... }
}
```
