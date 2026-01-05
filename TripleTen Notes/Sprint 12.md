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
**Use URL Params When**:
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