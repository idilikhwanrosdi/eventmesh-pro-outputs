```
eventmesh-pro/
├── server/
│   ├── controllers/
│   │   └── auth.controller.js
│   ├── routes/
│   │   └── auth.routes.js
│   ├── services/
│   │   └── user.service.js
│   ├── utils/
│   │   ├── jwt.js
│   │   └── bcrypt.js
│   └── index.js
└── client/
    └── ...

```


```javascript
// server/services/user.service.js

const bcrypt = require('../utils/bcrypt');
const jwt = require('../utils/jwt');
const AWS = require('aws-sdk');
const dynamoDb = new AWS.DynamoDB.DocumentClient();

const TABLE_NAME = 'users'; // Replace with your DynamoDB table name

async function createUser(email, password) {
  const hashedPassword = await bcrypt.hashPassword(password);
  const params = {
    TableName: TABLE_NAME,
    Item: { email, password: hashedPassword },
  };
  await dynamoDb.put(params).promise();
}

async function findUserByEmail(email) {
  const params = {
    TableName: TABLE_NAME,
    Key: { email },
  };
  const result = await dynamoDb.get(params).promise();
  return result.Item;
}


async function generateTokens(user) {
  const accessToken = jwt.generateAccessToken(user);
  const refreshToken = jwt.generateRefreshToken(user);
  return { accessToken, refreshToken };
}


module.exports = { createUser, findUserByEmail, generateTokens };

```

```javascript
// server/utils/jwt.js

const jwt = require('jsonwebtoken');

const ACCESS_TOKEN_SECRET = process.env.ACCESS_TOKEN_SECRET; // Store securely
const REFRESH_TOKEN_SECRET = process.env.REFRESH_TOKEN_SECRET; // Store securely

function generateAccessToken(user) {
  return jwt.sign({ email: user.email }, ACCESS_TOKEN_SECRET, { expiresIn: '15m' });
}

function generateRefreshToken(user) {
  return jwt.sign({ email: user.email }, REFRESH_TOKEN_SECRET, { expiresIn: '7d' });
}

function verifyToken(token, secret) {
  try {
    return jwt.verify(token, secret);
  } catch (error) {
    return null;
  }
}


module.exports = { generateAccessToken, generateRefreshToken, verifyToken};

```


```javascript
// server/routes/auth.routes.js
const Hapi = require('@hapi/hapi');
const authController = require('../controllers/auth.controller');

module.exports = [
  {
    method: 'POST',
    path: '/login',
    handler: authController.login,
  },
  {
    method: 'POST',
    path: '/register',
    handler: authController.register,
  },
  {
    method: 'POST',
    path: '/refresh',
    handler: authController.refresh,
  },
  {
    method: 'GET',
    path: '/me',
    handler: authController.me,
    options: {
      auth: 'jwt', //  Example middleware usage
    },
  },
];


```

```javascript
// server/controllers/auth.controller.js
const userService = require('../services/user.service');
const bcrypt = require('../utils/bcrypt');
const jwt = require('../utils/jwt');


const login = async (request, h) => { /* ... */};
const register = async (request, h) => { /* ... */};
const refresh = async (request, h) => { /* ... */};
const me = async (request, h) => { /* ... */};

module.exports = { login, register, refresh, me};
```

```javascript
// server/utils/bcrypt.js
const bcrypt = require('bcrypt');
const SALT_ROUNDS = 10;

async function hashPassword(password) {
    return bcrypt.hash(password, SALT_ROUNDS);
}

async function comparePasswords(password, hash) {
    return bcrypt.compare(password, hash);
}


module.exports = { hashPassword, comparePasswords };
```


```javascript
// client/middleware/auth.js  (Nuxt.js middleware example)

export default function ({ store, redirect }) {
  // If the user is not authenticated
  if (!store.state.auth.loggedIn) {
    return redirect('/login')
  }
}

```



<!-- Folder usage explanation (removed per request, but kept for future reference):

- **controllers/:** Contains logic for handling API requests and responses.
- **routes/:** Defines API endpoints and their corresponding controllers.
- **services/:** Contains business logic related to users, including authentication, registration, and data access.
- **utils/:** Utility functions for JWT management, bcrypt hashing, and other common tasks.

Middleware integration:

- Hapi.js: Use `options: { auth: 'jwt' }` in route definition to protect a route. Define the `jwt` authentication scheme in your server setup.
- Nuxt.js: Create a middleware file (e.g., `middleware/auth.js`) and check for authentication status. Redirect to login if not authenticated.


-->