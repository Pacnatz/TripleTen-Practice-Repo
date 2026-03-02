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