## Automated Testing:
#### Testing Frameworks and Methods:
<span class="blue-text-bold">jest</span> - A testing framework that allows users to use testcases
```bash
npm install --save-dev jest
```
```json
// package.json
"scripts": {
  "test": "jest",
}
```
**Where to write tests:**
The tests for a file called `helpers.js` would be placed in `helpers.test.js`
```js
// function.js
const sayHello = (firstName, lastName) => {
  return `Hello, ${firstName} ${lastName}!`;
};

module.exports = sayHello;
```
**How to write tests:**
`test()` or `it()` are used for creating tests. First parameter is the name of the test, the 2nd parameter is the function to be run
```js
// function.test.js
test('What the function under test should do', () => {
  // expected output depending on input
});
it('What the function under test should do', () => {
  // expected output depending on the input
});
```
<span class="blue-text-bold">expect()</span> - It takes the expected values as an argument and returns an object with methods for testing.
<span class="blue-text-bold">toBe()</span> - Compares its own argument to the one passed to the expect() function
```js
expect(1).toBe(1); // true
expect(1).toBe(2); // false
expect(1+2).toBe(3) // true
```
```js
// function.test.js
const sayHello = require("./function.js");
test('Creates a greeting', () => {
  expect(sayHello('Lera', 'Jackson')).toBe('Hello, Lera Jackson!');
});
```
We can now run `npm run test`
- toBe() tests primitive values and object references
- toEqual() is used to compare objects and arrays
- toStrictEqual() is used to compare objects and arrays but more "strictly"
- toBeUndefined() toBeDefined() compare the outputs with undefined toBeDefined will pass even with a value of null.
- toBeNull() checks if the output is null
- toMatch() checks whether a string matches a regular expression
- toContain checks the resulting array contains the value we specify
- toBeGreaterThan(), toBeGreaterThanOrEqual(), toBeLessThan(), toBeLessThanOrEqual()
#### Units and Test Methods:
Ideally all functions need to be tested. To do this, we need to break our program down into units.
<span class="blue-text-bold">Unit</span> - The smallest part of code that can be tested in isolation.:

```js
// Each of these functions is a unit so we can write a test for each of them
// check that the password contains a number
function checkNumber(pass) {
  if (typeof pass === 'string') {
    let regex = /\d/;
    return regex.test(pass); // the regex.test() method will return true if the passed string matches the regexp

    }
}

// check that the password contains a special character
function checkSymbol(pass) {
  if (typeof pass === 'string') {
    let regex = /[!@#$%^&*(),.?":{}|<>_]/;
    return regex.test(pass);
  }
}

// run both checks
function checkPass(pass) {
  return checkNumber(pass) && checkSymbol(pass);
```

```js
test('Check that the password contains a number', () => {
    expect(checkNumber('some_not_so_strong_pass')).toBe(false);
    expect(checkNumber('stronger_pass_123')).toBe(true);
});

test('Check that the password contains a special character', () => {
    expect(checkSymbol('somePass')).toBe(false);
    expect(checkSymbol('another_pass')).toBe(true);
});

test('Check password', () => {
    expect(checkPass('somePas$')).toBe(false);
    expect(checkPass('another_pass_123')).toBe(true);
});
```
#### Test Suites:
<span class="blue-text-bold">Test Suite</span> - Testing a group of units functions making up the overall function
<span class="blue-text-bold">Unit</span> - A singular function to be tested
So if you have a function that creates a registration it will have a validation method, calculate password hash and writes data to data base method. You can make a test suite for all 3 of these
```js
describe('Request handler tests', () => { // Test suite
  // Unit test
  test('should validate the data', () => {/* test code */});
  test('should calculate the password hash', () => {/* test code */});
  test('should write the data to the database', () => {/* test code */});
});
```
#### Testing HTTP Requests:
If engineers change rules for accessing their API we need to account for that with the <span class="blue-text-bold">SuperTest</span> Library
start testing -> connect to server -> run tests -> disconnects from server -> finish tests
<span class="red-text-bold">When running tests, process.env.NODE_ENV is automatically set to "test" by the Jest runner</span>
```js
if (process.env.NODE_ENV !== "test") {
  app.listen(PORT, () => {
    console.log(`App listening on port ${PORT}`);
  })
}
```
**Installing SuperTest:**
```bash
npm install --save-dev supertest
```
```js
// endpoint.test.js
const supertest = require('supertest');
const app = require('./app.js');

const request = supertest(app);
```
**Checking Request Sending:**
```js
describe('Endpoints respond to requests', () => {
  it('Returns data and status 200 on request to "/"', () => {
    return request.get('/').then((response) => { // get post delete put patch
      expect(response.status).toBe(200);
      expect(response.text).toBe('Hello, world!');
    })
  })
})
```
**Configuring Requests:**
- set() sets the attributes
- send() sets the req body
- query() configure a get request that doesn't have a body only a url
- attach() attach a file to the request
#### Database Testing:
We can't use real data in tests because if there's a problem it can damage user data
Engineers add random meaningless data then delete it
database -> test -> delete test data
<span class="blue-text-bold">beforeAll()</span> - Will run before running all tests in a file
<span class="blue-text-bold">afterAll()</span> - Will run after running all test in a file
<span class="blue-text-bold">beforeEach()</span> - Will run before each test.
<span class="blue-text-bold">afterEach()</span> - Will run after each test
```js
const mongoose = require('mongoose');
const User = require('../models/user');
const fixtures = require('../fixtures');

const MONGO_URL = 'mongodb://localhost:27017/aroundtheus';
  
beforeAll(() => {
  return mongoose.connect(MONGO_URL, {
    useNewUrlParser: true,
    useCreateIndex: true,
    useFindAndModify: false,
    useUnifiedTopology: true,
  });
});

afterAll(() => {
  return mongoose.disconnect();
});

describe('Database tests', () => {
  beforeEach(() => {
    const { name, about, avatar, email, password } = fixtures.user;
    return User.create({
      name,
      about,
      avatar,
      email,
      password,
    });
  });
  afterEach(() => User.deleteOne({ email: fixtures.user.email }));
  it('The user must be complete', () => {
    return User.findOne({ email: fixtures.user.email })
      .then((user) => {
        expect(user).toBeDefined();
        expect(user.email).toBe(fixtures.user.email);
        expect(user.name).toBe(fixtures.user.name);
      });
  });
});
```
## Advanced Middleware:
#### Centralized Error Handling:
```js
// example from middlewares/auth.js

try {
  payload = jwt.verify(token, 'some-secret-key');
} catch (err) {
  // always log those errors
  console.error(err);

  // if an error occurs we immediately set the status and send it
  return res
    .status(401)
    .send({ message: 'Authorization required' });
}
```
```js
// app.js

// all the app.js code

// other app.use() statements

// we handle all errors here, by logging the error to the console
// and sending a response with an appropriate status code and message
app.use((err, req, res, next) => {
  console.error(err);
  return res.status(500).send({ message: 'An error occurred on the server' });
});

app.listen(PORT);
```
Using the new middleware
```js
try {
  payload = jwt.verify(token, 'some-secret-key');
} catch (e) {
  const err = new Error('Authorization required'); 
  err.statusCode = 401;

  next(err);
}
```
```js
app.use((err, req, res, next) => {
  console.error(err);
  res.send({ message: err.message });
});

// { "message": "Authorization error" }
```
**Custom error constructors:**
```js
// errors/not-found-err.js

class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.statusCode = 404;
  }
}

module.exports = NotFoundError;
```

```js
const NotFoundError = require('./errors/not-found-err');

module.exports.getProfile = (req, res, next) => User
  .findOne({ _id: req.params.userId })
  .then((user) => {
    if (!user) {
      // if there is no such user,
      // throw an exception
      throw new NotFoundError('No user with matching ID found');
    }

    res.send(user);
  })
  // ..
```
**Handling errors that have already been thrown:**
```js
const NotFoundError = require('./errors/not-found-err');
const BadRequestError = require('./errors/bad-request-err');

module.exports.getProfile = (req, res, next) => User
  .findOne({ _id: req.params.userId })
  .then((user) => {
    if (!user) {
      throw new NotFoundError('No user with matching ID found');
    }

    res.send(user);
  })
  .catch((err) => {
    if (err.name === "CastError") {
      next(new BadRequestError("The id string is in an invalid format");
    } else {
      next(err);    
    }
  ); 
```
**Handling unknown errors:**
```js
app.use((err, req, res, next) => {
  console.error(err);
  // if an error has no status, set it to 500
  const { statusCode = 500, message } = err;
  res
    .status(statusCode)
    .send({
      // check the status and display a message based on it
      message: statusCode === 500
        ? 'An error occurred on the server'
        : message
    });
});
```
**Rules for centralized error handling**

1. **Always log the errors to the console with `console.error`.**
    
    This rule isn’t actually new, but it bears repeating: you should always include a `console.error` statement to log your errors to the console. This is your most important source of information when trying to debug.
    
2. **Always terminate promise chains with a `catch()` block.**
    
    Pass the `next()` function to the `catch()` block and add an error handler somewhere at the end of `app.js`. If a promise chain is not terminated with a `catch()`, it will result in an unhandled promise rejection. However, in future versions of Node.js, the application will simply crash.
    
3. **Don’t use `throw` in terminating `catch()` blocks.**
    
    `throw` redirects the code handling to the next `catch()` block. If you use `throw` in the last `catch()` block, it will have nowhere to go and this will lead to an unhandled promise rejection.
    
4. **If the handler receives an error without a status, return a server error.**
    
    In cases where the error is unknown, you should return a server error with a `500` status code.
#### Validating Inbound Server Data:
<span class="red-text-bold">We use Joi and Celebrate as middleware so our input gets validated before it hits the controller. Which can help with DDOS attacks because they can sending many request that can overload the processor such as password hashing</span> 
**Joi and Celebrate:**
<span class="blue-text-bold">Joi</span> - A popular Node.js library for data validation. Allows you to describe data in an intuitive way
<span class="blue-text-bold">celebrate</span> - Npm package that includes joi to validate incoming request data

```json
{
  body: Joi.object().keys({
    email: Joi.string().required().email(),
    password: Joi.string().required().min(8),
    name: Joi.string().required().min(2).max(30),
    age: Joi.number().integer().required().min(18),
    about: Joi.string().min(2).max(30),
  })
}
```
- `email` — a string, required field, must match the email pattern.
- `password` — a string, required field, at least 8 characters.
- `name` — a string, required field, from 2 to 30 characters.
- `age` — a number, integer, required field, the minimum possible value is 18.
- `about` — a string from 2 to 30 characters.
```js
const { celebrate, Joi } = require('celebrate');

router.post('/posts', celebrate({
  body: Joi.object().keys({
    title: Joi.string().required().min(2).max(30),
    text: Joi.string().required().min(2),
  }),
}), createPost);
```
Also allows you to validate parameters and headers and queries
```js
const { celebrate, Joi } = require('celebrate');

router.delete('/:postId', celebrate({
  // validate parameters
  params: Joi.object().keys({
    postId: Joi.string().alphanum().length(24),
  }),
  headers: Joi.object().keys({
    // validate headers
  }),
  query: Joi.object().keys({
    // validate query
  }),
}), deletePost);
```
<span class="red-text-bold">By default, Joi doesn’t allow fields that are not listed in the validation object. To change this behavior, after calling the `keys()` method, call the `unknown()` method with `true` as an argument</span>
```js
const { celebrate, Joi } = require('celebrate');

router.delete('/:postId', celebrate({
  headers: Joi.object().keys({
    "Content-Type": "application/json",
    // If an unknown headers is passed here it will still pass validation
  }).unknown(true),
}), deletePost);
```
**Errors:**
If the request doesn't pass the described validation, celebrate will pass it onto it's own error handler not our centralized handler
```js
// app.js

const { errors } = require('celebrate');

// ...

// error handlers
app.use(errors()); // celebrate error handler

// our centralized handler
app.use((err, req, res, next) => {
  // ...
});
```
Any validation errors will be code 400
```js
"child \"name\" fails because [\"name\" is required]",{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "child \"name\" fails because [\"name\" is required]",
    "validation": {
        "source": "body",
        "keys": [
            "name"
        ]
    }
}
```
```bash
npm install celebrate validator
```
#### Logging:
<span class="blue-text-bold">Winston</span> - A logging library. Helps you record messages about what your app is doing
<span class="blue-text-bold">expressWinston</span> - middleware for Express that automatically logs HTTP requests and responses
```js
// middlewares/logger.js

const winston = require('winston');
const expressWinston = require('express-winston');

// The winston.format function allows us to customize how our logs 
// are formatted. In this case, we are using a built-in timestamp
// format, as well as Winston's generic printf method.
const messageFormat = winston.format.combine(
  winston.format.timestamp(),
  winston.format.printf(
    ({ level, message, meta, timestamp }) =>
      `${timestamp} ${level}: ${meta.error?.stack || message}`
  )
);

// The request logger, with two different "transports". One transport
// logs to a file, the other logs to the console.
const requestLogger = expressWinston.logger({
  transports: [
    new winston.transports.Console({
      // For console logs we use our relatively concise messageFormat
      format: messageFormat,
    }),
    new winston.transports.File({
      filename: "request.log",
      // For file logs we use the more verbose json format
      format: winston.format.json(),
    }),
  ],
});
```
**Error logger:**
```js
// middlewares/logger.js

//...

// error logger
const errorLogger = expressWinston.errorLogger({
  transports: [
    new winston.transports.Console({
      format: messageFormat,
    }),
    new winston.transports.File({
      filename: "error.log",
      format: winston.format.json(),
    }),
  ],
});
```
```bash
npm install winston express-winston
```

## Deploying and Hosting in the Cloud
#### Creating a Remote Server
<span class="red-text-bold">Google Cloud requires your comment on your public ssh key to be your username</span>
```bash
# Creates a new ssh key -C is the comment -f is the filename
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/somerandomname
cat ~/.ssh/id_ed25519.pub # Outputs public ssh key for google cloud 
eval `ssh-agent -s` # Starts an ssh agent in memory. It will be lost if you restart your pc
ssh-add ~/.ssh/id_ed25519 # Adds private ssh key to the bash instance to be used to verify with the public one
```
#### Making the Server Ready to Use
**Installing Node.js:**
```bash
# Run these on the VM

# First command
sudo apt-get update

# Second command
sudo apt-get install -y ca-certificates curl gnupg

# Third command
sudo mkdir -p /etc/apt/keyrings

# Fourth command
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
```

```bash
# First command
NODE_MAJOR=20 # Replace this with the major node version on your local machine with node -v

# Second command
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
```

```bash
# Run these on the VM

# First command
sudo apt-get update

# Second command
sudo apt-get install nodejs -y
```

```bash
# Run these on the VM

node -v
```
**Installing MongoDB:**
```bash
# Run these on the VM

# First command
curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor

# Second command
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list

# Third command
sudo apt-get update

# Fourth command
sudo apt-get install -y mongodb-org
```

```bash
# Run this on the VM
# run the mongod service
sudo systemctl start mongod
```

```bash
# Run this on the VM
# Verify if its running
sudo systemctl status mongod
```

```bash
# Run this on the VM
# Launch automatically even after server remote machine restarts
sudo systemctl enable mongod
```

```bash
# Run this on the VM
# Starting mongosh. Similar to MongoDB Compass
mongosh
```
**Installing Git:**
```bash
# Run these on the VM

# 1.
sudo apt update

# 2.
sudo apt install git
```
**Launching the server:**
```bash
# Run this on the VM
cd ~
```
Use HTTPS git option as it allows for the repo to be readonly
```bash
# Run this on the VM.
# Make sure to replace REPO_NAME with your repo's name 
# and YOUR_USERNAME with your username.
git clone https://github.com/YOUR_USERNAME/REPO_NAME.git
```
1. `cd` into the cloned repo with `cd REPO_NAME`.
2. Install the dependencies.
```bash
# Run this on the VM
npm install
```
3. Run the server
```bash
# Run this on the VM
npm run start
```
#### How to keep an application continuously running with PM2
**Installing PM2 and launch the server:**
```bash
# Run this on the VM
sudo npm install pm2 -g
```

```bash
# Run this on the VM, in the back-end folder
pm2 start app.js
```
**Make the application more stable:**
```bash
pm2 startup

[PM2] Init System found: systemd 
[PM2] To setup the Startup Script, copy/paste the following command:
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u YOUR_USERNAME --hp /home/YOUR_USERNAME

pm2 save
```
**How to check PM2's status and logs:**
```bash
# Tells you if the process failed to start
pm2 list
# Logs the files last 50 lines
pm2 logs --lines 50
# Stops all processes and the PM2 daemon
pm2 kill
# Restart the application
pm2 start app.js
```
#### Registering a Subdomain:
**Create a subdomain for the frontend:**
- FreeDNS has a list of free domains
Type: A
Subdomain: ttfs
Domain: zinergy.net
Destination: 34.24.34.68

**Create `www` and `api` subdomains:**
Repeat the same for www. prefix and api. prefix
<span class="red-text-bold">The www. prefix is to ensure that your web application works regardless if the user puts in www. or without it </span>

GET www.ttfs.zinergy.net:3001/items returns 200 OK
#### Configuring Ports with nginx:
<span class="green-text">By default requests are sent to port 80</span>
<span class="blue-text-bold">nginx</span> - Pronounced "Engine X" is an HTTP server that can quickly redirect requests and serve static files
- If we need to send a static file or redirect a request to another URL we should use nginx
- If we want to implement some kind of logic, we should use Node.js
We want to make nginx to listen to requests on port 80 and redirect them to port 3001
**Install nginx:**
```bash
sudo apt update # updates the list of packages
sudo apt install -y nginx # installs nginx, -y approves all the prompts automatically
```
<span class="blue-text-bold">firewall</span> - A layer of security between a server and the outer world. Decide which requests should pass through and which should be blocked
```bash
sudo apt install ufw
sudo ufw allow 'Nginx Full' # Opens ports 80 and 443
sudo ufw allow OpenSSH
sudo ufw enable # Turn on firewall
sudo systemctl enable --now nginx # Luanch nginx and start it automatically
```
**Configure nginx:**
```bash
sudo nano /etc/nginx/sites-available/default 
# sudo allows us to execute a command as a super user 
# nano is a text editor 
# /etc/nginx/sites-enabled/default is a path to the nginx configuration file 
```
Delete the contents and replace it with this configuration:
```json
server {
  listen 80;

  server_name domainname.example.com www.domainname.example.com api.domainname.example.com;

  location / {
    proxy_pass http://localhost:3001;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
}
```
```bash
sudo nginx -t # Restart nginx
sudo systemctl reload nginx
```
#### Encrypting Data with HTTPS, SSL, and CertBot:
<span class="blue-text-bold">SSL</span> - Secure Socket Layer. It encrypts data that's transferred between the client and server
**Install Certbot:**
1. SSH into the server
2. Install Certbot
```bash
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/local/bin/certbot
```
**Connect the certificate:**
```bash
sudo certbot --nginx
# Will read the domain names in the NGINX config and issue certificates to use with those domains
# Answer the following prompts
sudo systemctl restart nginx # Restart nginx
# Now all traffic on port 443 (HTTPS port) will be encrypted
```
<span class="red-text-bold">The issued certificate must be renewed at least once every 3 months. Certbot renews the certificate automatically before it expires you don't need to run Certbot again unless you change your configuration</span>

#### Updating Back-End Code on the Server:
To update your server you need to pull from origin and if there are new node modules you must run npm install again. After that you need to pm2 restart the app.
```bash
cd ~/se_project_express
git pull origin main
npm install
pm2 restart app
```
#### Keeping Secrets with Environmental Variables:
**Generating Cryptographically Strong Pseudorandom Data:**
- <span class="blue-text-bold">cryptographically strong</span> - Data being generated by an algorithm that can't be effectively analyzed
- <span class="blue-text-bold">Pseudorandom</span> - random characters that aren't actually random and are generated by a mathematical algorithm
```js
const crypto = require('crypto'); // importing the crypto module

const randomString = crypto
  .randomBytes(16) // generating a random sequence of 16 bytes (128 bits) 
  .toString('hex'); // converting it into a string

console.log(randomString); // 5cdd183194489560b0e6bfaf8a81541e
```

```bash
# Input this into any terminal and copy the key
node -e "console.log(require('crypto').randomBytes(32).toString('hex'));"
```
**Updating your backend to read environmental variables:**
```bash
npm install dotenv
```

```js
// app.js
require('dotenv').config();
```

```js
// config.js
const { JWT_SECRET = "super-strong-secret" } = process.env;

module.exports = {
  JWT_SECRET,
};
```
**Creating a .env file on the server:**
Pull the repo and do npm install
```bash
nano .env
```
```
NODE_ENV=production
JWT_SECRET=the-secret-key-from-step-1-goes-here
```
**Restart pm2:**
```bash
pm2 restart app.js
```
#### Deploying your frontend to the server
**Update your frontend repo:**
```js
const baseUrl = process.env.NODE_ENV === "production" 
  ? "https://api.ttfs.zinergy.net" // Must use api subdomain
  : "http://localhost:3001";
```
**Update the homepage field in your package.json:**
```json
"homepage": "https://ttfs.zinergy.net"
```
**Upload the frontend code to the server:**
- Create the frontend build locally
- Copy the dist folder to the remote server
1. Create a folder on the server `mkdir ~/frontend`
2. Build the React project on your local machine `npm run build`
3. Add ssh key to the ssh agent on local machine
```bash
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519
```
4. Run scp on local machine
```bash
# Run this locally from the root of your frontend repository
scp -r ./dist/* nhatchu0508@ttfs.zinergy.net:/home/nhatchu0508/frontend
```
You can add it as a script for deployment in package.json
```json
"deploy": "npm run build && scp -r ./dist/* nhatchu0508@ttfs.zinergy.net:/home/nhatchu0508/frontend
```
**Setup nginx to serve the frontend:**
Open up nginx config file with
```bash
sudo nano /etc/nginx/sites-available/default
```
<span class="red-text-bold">Update the 1st server block to the api url and create a 2nd server block to include the www. and base url remember to update the url cert on the 2nd server block</span>
```json
server {
  server_name api.ttfs.zinergy.net;
  
  location / {
    proxy_pass http://localhost:3001;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/ttfs.zinergy.net/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/ttfs.zinergy.net/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
  server_name ttfs.zinergy.net www.ttfs.zinergy.net;

  root /home/nhatchu0508/frontend;

  location / {
    try_files $uri $uri/ /index.html =404;
  }

  listen 443 ssl; # managed by Certbot
  ssl_certificate /etc/letsencrypt/live/ttfs.zinergy.net/fullchain.pem; # managed by Certbot
  ssl_certificate_key /etc/letsencrypt/live/ttfs.zinergy.net/privkey.pem; # managed by Certbot
  include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
    if ($host = www.ttfs.zinergy.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = api.ttfs.zinergy.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = ttfs.zinergy.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


  listen 80;

  server_name ttfs.zinergy.net www.ttfs.zinergy.net api.ttfs.zinergy.net;
    return 404; # managed by Certbot
}
```

```bash
sudo nginx -t
sudo systemctl restart nginx
```

```bash
# Fix parent folder permissions safely
sudo chmod 711 /home
sudo chmod 711 /home/nhatchu0508
sudo chown -R www-data:www-data /home/nhatchu0508/frontend
sudo chmod -R 755 /home/nhatchu0508/frontend

# If you want to edit your build you must have the user own that folder with
sudo chown -R nhatchu0508:www-data /home/nhatchu0508/frontend
# Then you can switch back to www-data to own the folder to clear the 500 server error
sudo chown -R www-data:www-data /home/nhatchu0508/frontend

```

