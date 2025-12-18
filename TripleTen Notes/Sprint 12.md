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
  console.log(req.url); // /hello
  console.log(req.method); // GET
  console.log(req.headers); // the request headers will be here
  console.log(req.body); // and the request body will be here. But the GET request doesn't have it
  res.statusCode = 200; // the response's status code 
  res.statusMessage = 'OK'; // the content of the message 
  res.setHeader('Content-Type', 'text/plain'); // add a header to the response 
  res.write('Hello, '); 
  // send the first part of the message - the "Hello, " string 
  res.write('world!'); 
  // send the second part of the message - the "world!" string 
  res.end(); 
  // end the response
});

server.listen(3000);
```
