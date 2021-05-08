---
layout: post
title:      "Sessions and Cookies With Node and Express"
date:       2021-05-08 22:16:51 +0000
permalink:  sessions_and_cookies_with_node_and_express
---




​	Cookies are sent via [response headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) and store data on the clients browser. The most common examples of this are for adds, tracking, shopping carts, and authentication. In Node, you can use the response object to set cookies in the header directly with:

```js
res.setHeader('Set-Cookie', 'someKey=someValue');
```

The browser will recognize the `Set-Cookie` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#cookies) then, store the key-value pair on the clients browser. While the cookie is still valid, the browser will send the data back to the server with every request. Be careful though, cookies can be easily read or manipulated because of that, sensitive data should not be stored in cookies or on the browser.

​	Sessions in Node can also can be set in a response object but sensitive info is stored and validated on the backend. The values sent to the browser should be encrypted to protect the user. We only want sensitive info to be shared across requests by the same user so other users cant use others sensitive data. To keep sessions secure and keep track of the user, the session id is encrypted and set in a cookie to be stored on the clients browser. The session id will be encrypted and is now protected against manipulation and cross site attacks. The server will be in charge of validating the session and only the specific server that created the session will be able to decrypt and manipulate the session.

​	To start with session in Node.js we can use a [package](https://expressjs.com/en/resources/middleware/session.html#compatible-session-stores) from Express.js called `express-sesssion`. The session can be configured in `app.js` by telling express to use the session middleware. 

```js
// app.js
const express = require('express');
const session = require('express-session');
const app = express();


app.use(
  session({ // config session options
    secret: 'some super secure secret',
    resave: false,
    saveUninitialized: false,
    cookie: {}
  })
);
```

Then in any route, you can manipulate the session by setting a key-value pair on the request object like so:

```js
// routes.js

exports.getRoute = (req, res, next) => {
	req.session.someKey = 'some value'
}

// controller.js

exports.doSomething = (req, res, next) => {
	if(req.session.someKey === 'some value'){
		res.render('/', {
			someKey: true
		})
  }else{
		res.render('/err', {
			someKey: false
		})
	}
}
```

Storing sessions in memory can grow quickly in size and is not reccomended by Express outside of development. With larger applications it’s a better practice to store the sessions in the database. Additionally, storing the session in the database is much more secure. We can do that using MongoDB with a package called `connect-mongodb-session`.

```js
// app.js
const express = require('express');
const session = require('express-session');
const app = express();
const MongoDBStore = require('connect-mongodb-session')(session); // require and construct with 'express-session'

const store = new MongoDBStore({ // create new instace of store
  uri: process.env.MONGODB_URI,
  collection: 'sessions',
});

app.use(
  session({
    secret: 'some super secure secret',
    resave: false,
    saveUninitialized: false,
    store, // add store to session middleware
  })
);
```

<sub>See other compatible databases [here](https://expressjs.com/en/resources/middleware/session.html#compatible-session-stores).</sub>


​	Sessions should be used when you want to keep track of data across multiple requests that belongs to a single role or user and should not be accessible or readable by any other means. Cookies are great for non-sensitive data and, of course, the popular uses cases listed in the first section of this blog. If you’re interested another alternative for sessions consider using [JSON Web Tokens (JWT)](https://jwt.io/introduction). A popular Express package from Auth0 is `express-jwt`, and you can read the docs [here](https://github.com/auth0/express-jwt#readme).



Resources:

[express-session - Express.js](https://expressjs.com/en/resources/middleware/session.html)

[Compatible Session Stores - Express.js](https://expressjs.com/en/resources/middleware/session.html#compatible-session-stores)

[HTTP Headers - Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

[express-jwt - Auth0](https://github.com/auth0/express-jwt#readme)

[JWT - jwt.io](https://jwt.io/introduction)


