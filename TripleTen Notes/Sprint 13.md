## Intro to Authentication and Authorization:
#### Authentication vs Authorization: What's the Difference?
<span class="blue-text-bold">Identification</span> - Give someone or something a name, number, or symbol (username or email)
<span class="blue-text-bold">Authentication</span> - Checking that the person is actually who they say they are.
<span class="green-text">Identification is part of the authentication process</span>
<span class="blue-text-bold">Authorization</span> - The process where the system decides which permissions to give a user
#### JWTs: Keeping Users Logged In:
**Authorization flow:**
1. A user enters their account details, i.e. their username and password.
2. The server generates a token, which is a unique set of symbols, and sends it to the user.
3. The token is saved inside the user's browser.
4. Upon subsequent page visits, the user sends this token to the server instead of having to enter their details again.
5. The server checks if there's a token in the request and verifies that this token matches the one that was previously sent to the user. This is the authentication part of the process.
6. If the token check is successful, then the user is authorized. If not, an error message is displayed to the user.
7. When the user logs out the system, the browser will delete the token from memory. After this, the user will need to re-enter their email and password in order to log in.
**The Structure of a Token:**
- The `header` contains metadata about the token.
- The `payload` consists of the data that the token carries.
- The `signature` ensures that the information in the token cannot be changed.
<span class="red-text-bold">These three parts are separated by dots</span>
![[18. JWT WebToken.png]]
###### Header:
Contains 2 fields:
 - The creation algorithm: HMAC, SHA256, or RSA
 - The type of token, ("JWT")
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
// This will be encoded into a string using Base64URL algorithm
```
###### Payload:
Contains actual information that was encoded e.g. user data
```json
{
  "name": "Elise Bouer",
  "_id": "39dow8ak8402jf23u4do057s",
}
// This will also be encoded into a string
```
###### Signature:
<span class="red-text-bold">Signature guarantees that the contents of the header and payload have not been changed since the token was created</span>
#### Other Authentication and Authorization Methods:
<span class="blue-text-bold">Sessions</span> - An object that contains the user's login information. The client receives a session identifier instead of a token.
<span class="blue-text-bold">OAuth</span> - Authentication through social network accounts such as Facebook, Yahoo, or Google (Open Authorization) (<span class="red-text-bold">Passport.js</span>)
<span class="blue-text-bold">Multi-Factor Authentication</span> - SMS authentication, Authenticator apps, 2FA, 3FA
## Server-Side Authentication and Authorization:
#### User Creation (the /signup route):
**User model:**
```js
// models/user.js

const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true,
    minlength: 8
  }
});

module.exports = mongoose.model('user', userSchema);
```
<span class="red-text-bold">If you use the unique property, but your collection already contains duplicates, the new constraint won't work. Similar to required: true</span>
**Saving Users in the Database:**
```js
// controllers/users.js

const User = require('../models/user');

exports.createUser = (req, res) => User.create({
  email: req.body.email,
  password: req.body.password,
})
  .then((user) => res.send(user))
  .catch((err) => res.status(400).send(err)); // Will run if there's a user with the same email
```
**Hashing passwords:**
<span class="blue-text-bold">bcryptjs</span> - A module to hash passwords
```js
// controllers/users.js

const bcrypt = require('bcryptjs'); // importing bcrypt
const User = require('../models/user');

exports.createUser = (req, res) => {
  // hashing the password
  bcrypt.hash(req.body.password, 10)
    .then(hash => User.create({
      email: req.body.email,
      password: hash, // adding the hash to the database
    }))
    .then((user) => res.send(user))
    .catch((err) => res.status(400).send(err));
};
```
**Salts and Hash Tables:**
![[19. Hacker's DB.png]]
<span class="blue-text-bold">Salts</span> - Before hashing the password, we add a random symbol, and this symbol will completely change the final hash. 
<span class="red-text-bold">This is available to view on the DB. It's so if someone that has the same password and that password gets hashed, hackers get the password for multiple users</span>
<span class="blue-text-bold">Cost factor</span> - Each cost factor doubles the hashing time, and makes brute-force attacks exponentially harder
```powershell
alice: $2b$10$saltA$hashA
bob:   $2b$10$saltB$hashB
carol: $2b$10$saltC$hashC
```
Same password.
Different salts.
Different hashes.
#### Authentication (the /login route):
**Creating an Authentication Controller:**
```js
// controllers/users.js

module.exports.login = (req, res) => {
  const { email, password } = req.body;
  // ...
};
```
1. Search the database for a user using the submitted email; if the user is found, hash the submitted password and compare it to the hash inside the base.
2. Hash the submitted password and check if there is a user in the database with the submitted hash and password.
```js
// controllers/users.js

module.exports.login = (req, res) => {
  const { email, password } = req.body;

  User.findOne({ email })
    .then((user) => {
      if (!user) {
        // user not found
        // fire the catch block with an error
        return Promise.reject(new Error('Incorrect password or email'));
      }
    // user found
    })
    .catch((err) => {
      // return an authentication error
      res
        .status(401)
        .send({ message: err.message });
    });
};
```
**Checking passwords:**
```js
// controllers/users.js

module.exports.login = (req, res) => {
  const { email, password } = req.body;

  User.findOne({ email })
    .then((user) => {
      if (!user) {
        return Promise.reject(new Error('Incorrect password or email'));
      }
      return bcrypt.compare(password, user.password);
    })
    .then((matched) => {
      if (!matched) {
        // the hashes didn't match, rejecting the promise
        return Promise.reject(new Error('Incorrect password or email'));
      }
      // successful authentication
      res.send({ message: 'Everything good!' });
    })
    .catch((err) => {
      res
        .status(401)
        .send({ message: err.message });
    });
};
```
#### Custom Methods for Mongoose Models:
- Our custom method has 2 parameters, email and password
- Returns either a user object or an error
- We can set it on the statics property on our desired schema
```js
// models/user.js

const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true,
    minlength: 8
  }
});
// we're adding the findUserByCredentials methods to the User schema 
// it will have two parameters, email and password
userSchema.statics.findUserByCredentials = function findUserByCredentials (email, password) {
  // trying to find the user by email
  return this.findOne({ email }) // this — the User model
    .then((user) => {
      // not found - rejecting the promise
      if (!user) {
        return Promise.reject(new Error('Incorrect email or password'));
      }

      // found - comparing hashes
      return bcrypt.compare(password, user.password)
        .then((matched) => {
          if (!matched) {
            return Promise.reject(new Error('Incorrect email or password'));
            }
          return user;
        });
    });
};

module.exports = mongoose.model('user', userSchema);
```
We can use it like so
```js
module.exports.login = (req, res) => {
  User.findUserByCredentials('elisebouer@tripleten.com', 'EliseBouer1989')
    .then(user => {
      // we get the user object if the email and password match
      // Authentication successful
    })
    .catch(err => {
      // otherwise, we get an error
      // Authentication error
      res.status(401).send({ message: err.message });
    });
};
```
#### JSON Web Tokens:
<span class="blue-text-bold">jsonwebtoken</span> - A package to import to use JWT
```js
// controllers/users.js

const jwt = require('jsonwebtoken'); 

module.exports.login = (req, res) => {
  const { email, password } = req.body;

  return User.findUserByCredentials(email, password)
    .then((user) => {
      // we're creating a token
      const token = jwt.sign(
        { _id: user._id }, 
        'some-secret-key', 
        { expiresIn: 3600 });
      // First parameter is the payload
      // Second parameter is secret key
      // Ooptional third parameter as an options object

      // we return the token
      res.send({ token });
    })
    .catch((err) => {
      res
        .status(401)
        .send({ message: err.message });
    });
};
```
#### Authorization Middleware:
**Getting the Token form the Header:**
<span class="green-text">If a valid token is presented, The request will continue on for further processing otherwise, the request will go to a controller which will return an error message to the client</span>
```js
// middleware/auth.js

const jwt = require('jsonwebtoken');

module.exports = (req, res, next) => {
  const { authorization } = req.headers;

  if (!authorization || !authorization.startsWith('Bearer ')) {
    return res
      .status(401)
      .send({ message: 'Authorization Required' });
  }

  const token = authorization.replace('Bearer ', '');
  let payload;
  
  try {
    payload = jwt.verify(token, 'some-secret-key');
  } catch (err) {
    return res
      .status(401)
      .send({ message: 'Authorization Required' });
  }

  req.user = payload; // assigning the payload to the request object

  next(); // sending the request to the next middleware
};
```
```js
// app.js

const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');

const { createUser, login } = require('./controllers/auth');
const auth = require('./middleware/auth');

const app = express();

// some routes don't require auth
// for example, register and login
app.post('/signup', createUser);
app.post('/signin', login);

// authorization
app.use(auth);

// Or we can add middlware to a separate route
app.use('/cards', auth, createCard));
```
## Regular Expressions:
#### JS Regular Expressions:

```js
// Regexes are enclosed in slashes
const regex = /expressing regularly/;
console.log(regex); // /expressing regularly/

const userList = 'Emma, Sean, Kate, Owen, Jane, Bartholomew, Luke';
const regex = /Bartholomew/;

userList.match(regex);  // [ 'Bartholomew' ]
```
**Using Regex:**
This is an alternative to the .includes array method
```js
const comments = [
  'I have an iPhone 6s — it\'s incredibly fast.',
  'I have the latest Samsung, everything is fine.',
  'Why pay more if I can get a Xiaomi cheap?',
  'The Nokia 3310 is the best phone ever. Once I dropped it into the Grand Canyon and it still works.',
  'The new iPhones are the best because they have improved water resistance.'
];
const regex = /iPhone/;
const iPhoneComments = comments.filter((item) => regex.test(item));

/* 
the test() method checks if the specified string exists and 
returns true or false. we will study this method later in depth.
if the test method returns true, we push the comment to a new array using
the filter method
*/

console.log(iPhoneComments);

/* [
  'I have an iPhone 6s — it\'s incredibly fast.',
  'The new iPhones are the best because they have improved water resistance.'
] */
```
#### Methods and Flags:
There are 3 entities that influence a search:
- <span class="blue-text-bold">Special characters</span> - Allows us to refine our search such as ignoring caps in a string
- <span class="blue-text-bold">Flags</span> - Customize the way methods work. e.g. find all matches instead of one
- <span class="blue-text-bold">Methods</span> - Define what should be done when the string is found. e.g. replace it show the location within the text
**Regex methods:**
```js
// String.match()
 const regex = /d/;
 const word = 'knowledge';

 word.match(regex); // [ 'd' ] — the method found the character in the string
 // Returns null if no match
 
 // regex.test()
 const regex = /c/;
 const word = 'schedule';

 regex.test(word); 
 // true — the method confirmed that there is a match in the string
 // false - if there's no match
```
<span class="red-text-bold">regex.test() will reassign the last index!</span>
**Flags:**
<span class="blue-text-bold">Flags</span> - Characters we place at the end of our regular expressions after the final slash. Customize our search settings
- The <span class="blue-text-bold">g</span> flag - You'll search for every match in your text
```js
const str = 'tro-lo-lo';
const result = str.match(/lo/);

result[0]; // 'lo'
result.index; // 4
result.input; // 'tro-lo-lo'

const globalResult = str.match(/lol/g);
result; // ['lo', 'lo']
```
- The <span class="blue-text-bold">i</span> flag - Disables case sensitivity
```js
const str = 'Wilhelm Conrad Roentgen was awarded the Nobel Prize in 1901.'

const regex = /roentgen/;
const regexIgnore = /roentgen/i;

str.match(regex); // null
str.match(regexIgnore); // [ 'Roentgen' ]
```
- The <span class="blue-text-bold">m</span> flag - Multiple line searches and required special characters
- The <span class="blue-text-bold">u</span> flag - Unicode searches
```js
const str = '& is an ampersand. Its Unicode value is 26';
const regex = /\u{26}/; // u flag is not set here
const regexUnicode = /\u{26}/u; // but here it is set

str.match(regex); // null
str.match(regexUnicode); // ['&']
```
- The <span class="blue-text-bold">y</span> flag - Allows us to start at a particular place in a text. Starts at lastIndex property
```js
const fuelUp = 'fuel up and go';
const regex = /go/y;

regex.lastIndex = 0; 
fuelUp.match(regex); 
// null. The first word is 'fuel', not 'go'

regex.lastIndex = 12;
fuelUp.match(regex); // [ 'go' ]

/* interestingly enough, different browsers display the info returned by string methods differently: some show the index only, while others show the original string and capturing groups */
```
- The <span class="blue-text-bold">s</span> flag - Enables dotall mode which allows . to match newline characters. Rarely used
<span class="red-text-bold">In a regular expression . can match any characters besides newlines</span>
```js
const str = `
  I can't remember what his name is:
  it's either Sortini or Sordini.
  Maybe even Surdini spelled with "u".
`;

const regex = /S.r.ini/g; // a dot in a regular expression stands for any character.

str.match(regex); // [ 'Sortini', 'Sordini', 'Surdini' ]
```
#### Special Characters and Negated Character Classes:
<span class="blue-text-bold">Special Characters</span> - Allows us to search for groups of characters, rather than a single specific one
```js
// Escaping characters
const str1 = 'yandex.com/maps/';
const regex1 = /\.com/; // escaping a dot, it's a period now
const regex2 = /\/maps/; // escaping a slash before the word maps

str1.match(regex1); // [ '.com' ]
str1.match(regex2); // [ '/maps' ]

// you need to escape a backslash in order to find it

const str2 = 'C:\\';
const regex3 = /\\/; // escaping a backslash

str2.match(regex3); // [ '\' ]
```
- <span class="blue-text-bold">\w</span> - Search for any digit, Latin letter (A-Z), or underscore
```js
const str = 'Sorry, I sent you the old copy of my paper bachelor_thesis_final_copy_3.docx. Please, do not open it. I am sending you a new one called bachelor_thesis_final_copy_4.docx';

const regex = /bachelor\wthesis\wfinal\wcopy\w\w.docx/g;

str.match(regex);

// [ 'bachelor_thesis_final_copy_3.docx', 'bachelor_thesis_final_copy_4.docx' ]
```
- <span class="blue-text-bold">\W</span> - Negation class! Searches anything besides a digit, Latin letter, or underscore
```js
const str = `
  IT companies' founding dates:
  Yandex: 09.23.1997
  Apple: 04/01/1976
  IBM: 06-16-1911
`;

const regex = /\w\w\W\w\w\W\w\w\w\w/g;

/* the digits are denoted by lowercase \w and the delimiters are uppercase \W. a delimiter is NOT a number, NOT a letter, and NOT an underscore. */

str.match(regex); // [ "09.23.1997", "01/04/1976", "16-06-1911" ]
```
- <span class="blue-text-bold">\d</span> - A special character that matches any digit:
```js
const str = '99 Balloons';
const regex = /\d\d/g;

str.match(regex); // [ '99' ]
```
- <span class="blue-text-bold">\s</span> - Searches for voids in text. Spaces that break lines don't break them and tabs
```js
const str = 'The smell\n' +
                    '            of vegetables' +
                    '                        on a cold day' +
          'performs faithfully an act of reality';

const regex = /\s/g;

str.match(regex).length; // 47 — Brautigan loved spaces
```
- <span class="blue-text-bold">\b</span> - boundaries. Before the first character, After the last character
```js
const string = "sadness";

string.match(/\bs/).index; // 0 — i.e. the first letter s
// the special character points at the border to its left, i.e. the beginning

string.match(/s\b/).index; // 6 - i.e. the last letter s
// the special character points at the border to its right, i.e. the end
```
#### Sets and Ranges:
<span class="blue-text-bold">Sets</span> - Define of group of characters to search for (Defines it for a single character)
```js
const regex = /rec[ie][ie]ve/g;
'receive or recieve'.match(regex); // ['receive', 'recieve']
```
<span class="blue-text-bold">Ranges</span> - Match characters within a selected range of values
```js
const regex = /[a-z0-9_!]/g; // Matches all latin, digits, underscore, and !
```
**Negating Character Sets and Ranges:**
```js
const str = 'Midterm grades: D C C A D B B C A';
const regex = /[^C-F]/g;

str.match(regex).join(''); // 'Midterm grades:      A  B B  A'

// it looks better now, but it will look suspicious if you leave
// blank spots on your report card like this!
```
It's good to escape these characters
- dot `.`
- plus `+`
- parentheses `()`
- caret (or hat) `^`
- opening square bracket `[`
- hyphen `-`
#### Quantifiers:
<span class="blue-text-bold">The + Quantifier</span> - The engine will look for all words in which this character occurs one or more times (At least once)
```js
const str = 'The correct spelling of the word "millennium" is with two Ls';
const regex = /mil+ennium/;

// this regular expression will find both variants: with one "L" and with two "L"s

str.match(regex); // [ 'millennium' ]
```
<span class="blue-text-bold">The * Quantifier</span> - Can find a match even if that character isn't present (Zero to Infinity)
```js
const exc = 'artist';
const esc = 'artiste';
const regex = /artiste*/; // the letter "e" may or may not occur
exc.match(regex); // [ 'artist' ]
esc.match(regex); // [ 'artiste' ]
```
<span class="blue-text-bold">The ? Quantifier</span> - Will only match if there's zero occurrences or 1 occurrence. (Zero to One)
```js
/* makes the letter u optional and matches 
both spelling variants: favourite and favorite. */

const regex = /favou?rite/g;
const    str = 'favourite or favorite';

str.match(regex); // ['favourite', 'favorite']
```

<span class="blue-text-bold">The | Quantifier</span> - Allows "forks" of our characters, `a | b` means `a` or `b` is fine
```js
const someSymbol = /cent(er|re)/g
const    str = 'center or centre';

console.log(str.match(someSymbol)); // ['center', 'centre']
```
<span class="blue-text-bold">The {} Quantifier</span> - Search for a group of repeated characters
```js
const regionCode = /\d{3}/;
const    phoneNumber = 'My phone number: +1(555)324-41-5';

phoneNumber.match(regionCode); // [ '555' ]
```
Indicating the exact number of repetitions:
```js
const str = 'this much, thiiis much, thiiiiiiis much';
const regex = /thi{2,5}s/;

str.match(regex); // [ 'thiiis' ]

// in the word "this" the letter "i" occurs only once
// and in the word "thiiiiiiis" the letter "i" occurs more than 5 times
```
You can omit the max number of repetitions:
```js
const someSymbol = /a{1,}/g;
const    str = 'alohaa';

console.log(str.match(someSymbol)); // ['a', 'aa']
```
**Lazy and Greedy Quantifiers:**
<span class="blue-text-bold">The Greedy Quantifier</span> - Engine chooses the longer match over the shorter one (default of the {} quantifier)
```js
const    str = 'Everyone else knows that book, you know';
const someSymbols = /e.{2,11}e/gi;

console.log(str.match(someSymbols)); // ['Everyone else'] Instead of 'Everyone'
```
<span class="blue-text-bold">The Lazy Quantifier</span> - Engine chooses the shorter match over the long one use `{}?`
```js
const someSymbols = /e.{2,11}?e/gi;
const    str = 'Everyone else knows that book, you know';

console.log(str.match(someSymbols)); // ['Everyone', 'else']

/* the lazy quantifier has found the shortest matches this time */ 
```
<span class="blue-text-bold">You can make the + operator lazy by adding ? in the same way </span>
#### The Beginning and End of a String - The m Flag:
<span class="blue-text-bold">The ^ special character</span> - Match the following character only if it is found at the beginning of the string
```js
const regex = /^\d+/g;
const newReg = /\d+/g;
const    str = '2001: A Space Odyssey premiered in 1968';

str.match(regex); // [ '2001' ];
str.match(newReg); // [ '2001', '1968' ];
```
<span class="blue-text-bold">The $ special character</span> - It matches the specified characters if they're found at the end of the string
```js
const regex = /\d+$/;
const str = 'https://tripleten.com/learn/web/courses/37/sprints/17/topics/20/lessons/12';

console.log(str.match(regex)); // ( [ '12' ] )
```
**Multiline Texts:**
- The <span class="blue-text-bold">m</span> flag - The engine will consider each line break as the end of one line and the beginning of another
```js
const str = `Nature’s first green is gold,
  Her hardest hue to hold.
  Her early leaf’s a flower;
  But only so an hour.
  Then leaf subsides to leaf.
  So Eden sank to grief,
  So dawn goes down to day.
  Nothing gold can stay.`;
const regex = /[A-Z]+.?$/gim;

str.match(regex); // ['gold,', 'hold.', 'flower;', 'hour.', 'leaf.', 'grief,', 'day.', 'stay.']
```
**Difficulties with Dots:**
```js
const regex = /.*$/;
const regexMultiline = /.*$/m;
const    str = 'I got, I got, I got, I got\n' +
  'Loyalty, got royalty\n' +
  'Inside my DNA\n' +
  'Coconut quarter piece, got war and peace\n' +
  'Inside my DNA\n' + 
  'I got power, poison, pain and joy\n' +
  'Inside my DNA\n' +
  'I got hustle, though, ambition, flow\n' +
  'Inside my DNA';

str.match(regexMultiline); // [ "I got, I got, I got, I got" ]

/* The m flag is on, so the engine has found 
as many characters as it could 
starting from the end of the first line */ 

str.match(regex); // [ "Inside my DNA" ]

/* Here, the m flag is disabled, making the engine start searching
characters from the end of the string. The dot in the pattern will match
any character except a line break. That's why it returned
only the last line */
```
#### Methods of Regular Expressions:
<span class="blue-text-bold">The String.search() method</span> - Takes a regex and returns the index at which it starts. If not found returns -1 (Does not work with the global flag, just shows the first one)
```js
const regex = /\d{3,}/i;
const string = '12! equals 479001600';

string.search(regex); // 11
```
<span class="blue-text-bold">The String.split() method</span> - Returns an array between the delimiter of your rege
```js
const regex = /\n/im;
const text = `I made myself a snowball
As perfect as could be.
I thought I'd keep it as a pet
And let it sleep with me.
I made it some pajamas
And a pillow for its head.
Then last night it ran away,
But first it wet the bed.`

const newText = text.split(regex);
console.log(newText); 

/* [
  'I made myself a snowball',
  'As perfect as could be.',
  'I thought I'd keep it as a pet',
  'And let it sleep with me.',
  'I made it some pajamas',
  'And a pillow for its head.',
  'Then last night it ran away,',
  'But first it wet the bed.'
] */
```
<span class="red-text-bold">If it starts or ends with the delimiter it will result in an empty string</span>
<span class="blue-text-bold">The RegExp.exec() method</span> - If there's a g flag, exec will return the first match and it will also add the matching character's index to the lastIndex property
```js
const str = `I write, erase, rewrite
Erase again, and then
A poppy blooms.`;
let regex = /.+/g;

regex.exec(str); // ['I write, erase, rewrite']
regex.exec(str); // ['Erase again, and then']
regex.exec(str); // ['A poppy blooms.']
regex.exec(str); // null

str.match(regex) // Returns all at once
// [ 'I write, erase, rewrite', 'Erase again, and then', 'A poppy blooms.' ]
```
<span class="blue-text-bold">The RegExp.test() method</span> - Returns a boolean and remembers the lastIndex property like exec
```js
const regex = /\w+@\w+\.\w+/g;
const str = 'Elise Bouer: elisebouer@gmail.com';

regex.test(str); // true
regex.lastIndex; // 32

// Call the RegExp.test method again:
regex.test(str); // false
regex.lastIndex; // 0

/* the new search returned no results,
so the lastIndex property is reset to zero */
```
<span class="blue-text-bold">The String.replace() method</span> - Replace anything in a text by returning a new string
```js
const strObj = 'The space goes after the comma ,not before the comma.';
const regex = /\s,/g;

strObj.replace(regex, ', '); // 'The space goes after the comma, not before the comma.'
```
<span class="red-text-bold">Pattern attributes for input elements take a regular expression but it doesn't allow you to use flags, you need to use ranges</span>
```html
		<input type="text" class="form__input" id="name-input" required pattern="[A-Za-z -]{1,}">
```
## Complexity Analysis of Algorithms:
#### Asymptotic Analysis:
<span class="red-text-bold">Algorithms are evaluated based upon 2 parameters: execution time and memory consumption (temporal and spatial)</span>
<span class="blue-text-bold">Temporal complexity</span> - How long a program takes to run
```js
function sum(numbers) {
    let sum = 0;

  for (let i = 0; i < numbers.length; i++) {
        sum += numbers[i]
  }

  return sum;
  // 4 operations in the loop
}
```
- `let sum = 0` — initializing a variable
- `let i = 0` — initializing a variable
- `i < numbers.length` — checking against the condition
- `i += 1` — incrementing the variable's value by one
- `numbers[i]` — accessing an element in the array
- `sum += numbers[i]` — incrementing the value of the `sum` variable
You'll get `2 + 4 * numbers.length`
2 + O(4n)

| n       | 2 + 4n  | Δ      |
| ------- | ------- | ------ |
| 1       | 6       |        |
| 10      | 42      | 7      |
| 100     | 402     | 9.5714 |
| 1000    | 4002    | 9.9552 |
| 10000   | 40002   | 9.9955 |
| 100000  | 400002  | 9.9995 |
| 1000000 | 4000002 | 9.9999 |
42/6 = 7

<span class="red-text-bold">As n gets bigger, the 4n part dominates</span>
Function won't grow faster than some constant times n
10n+50 = O(n)
2^n+n^3 = O(2^n)
n + n^2 / 1000 = O(n^2)
<span class="blue-text-bold">Spatial complexity</span> - Measures the amount of memory an algorithm will consume
- `let sum = 0` — a variable for storing the sum
- `let i = 0` — a variable for storing the index
Spatial complexity of this algorithm is O(1). 
No matter how big your input gets, the algorithm always uses the same amount of memory
<span class="red-text-bold">If sum was an array and you append numbers it will be a growing spatial complexity O(n)</span>
#### Complex Operations:
<span class="blue-text-bold">Complex Operations</span> - Calling a function: map() join() filter() have a complexity of O(n)
```js
function printNames(people) {
  const names = people.map(p => p.name); // О(n), counts as n operations
  return names.join(', '); // О(n), counts as n operations
}
// O(n) + O(n) = O(2n) omit the 2 get O(n)
```
```js
function printNames(people) {
  const names = people.map(p => {
        if (p.fullName) {
            return p.fullName;
        } else {
            return [p.firstName, p.middleName, p.lastName].join(' ');
        }
    });

  return names.join(', ');
}
// Complexity is k*O(n) = O(n)
```
```js
function swap(arr, i, j) {
    const tmp = arr[i];
    
    arr[i] = arr[j];
    arr[j] = tmp;
}
// Complexity is O(1)
```
<span class="red-text-bold">Loops are O(n)</span>
<span class="red-text-bold">Nested loops are O(n^2)</span>
#### Complexity Functions:
Binary searches a O(log(n)) because it cuts the array in half
```js
function binarySearch(sortedNumbers, n) {
    // defining the start and end points of the search
  let start = 0;
  let end = sortedNumbers.length;
    
  while (start < end) {
        // searching for the element in the middle of the array
    const middle = Math.floor((start + end) / 2);
    const value = sortedNumbers[middle];
    
        // comparing the argument with the value in the middle of the array
    if (n == value) {
      return middle;
    }

        // if the argument is less than the median of the array, split the array in half
        // Now the end of the array is its former median
    if (n < value) {
      end = middle;
        // otherwise, the beginning of the array becomes the element that comes right after the "median"
    } else {
      start = middle + 1;
    }
  }
  
    // if the target number is not found, return -1
  return -1;
}
```