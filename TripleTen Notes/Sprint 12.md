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
#### Computer Networks:
