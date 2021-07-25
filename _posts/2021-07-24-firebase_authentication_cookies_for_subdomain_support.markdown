---
layout: post
title:      "Firebase Authentication Cookies for Subdomain Support"
date:       2021-07-25 01:18:18 +0000
permalink:  firebase_authentication_cookies_for_subdomain_support
---


I've spent this last week trying to find a way to add CSRF protection on my Next.js application that uses Firebase Authentication session cookies. Now, before you ask, I plan on sharing the session across wild card sub domains all based on the same application (similar to how Medium gives you a custom URL: username.medium.com). Firebase does not support this out of the box and I found adding api middleware to every request or wrapping each component with cookie middleware tedious and a bit frustrating.... Not as frustrating as it was to figure this out though -_- . 



First we have the firebase setup, your probably past this already though.

```js
// admin init
import admin from 'firebase-admin';

const adminCredentials = {
  project_id: process.env.NEXT_PUBLIC_FIREBASE_PROJECT_ID,
  private_key: process.env.FIREBASE_PRIVATE_KEY,
  client_email: process.env.FIREBASE_CLIENT_EMAIL,
};
const databaseURL = process.env.NEXT_PUBLIC_FIREBASE_DATABASE_URL;

if (!admin.apps.length) {
  admin.initializeApp({
    credential: admin.credential.cert(adminCredentials),
    databaseURL,
  });
}

export default admin;
```

```js
// client init
import firebase from 'firebase/app';
import 'firebase/auth';
import 'firebase/storage';
import 'firebase/firestore';
// import 'firebase/analytics';
// import 'firebase/performance';

const clientCredentials = {
  apiKey: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
  authDomain: process.env.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN,
  databaseURL: process.env.NEXT_PUBLIC_FIREBASE_DATABASE_URL,
  projectId: process.env.NEXT_PUBLIC_FIREBASE_PROJECT_ID,
  storageBucket: process.env.NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.NEXT_PUBLIC_FIREBASE_APP_ID,
  measurementId: process.env.NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID,
};

if (!firebase.apps.length) {
  firebase.initializeApp(clientCredentials);
  if (process.env.NODE_ENV === 'production') {
    if ('measurementId' in clientCredentials && typeof window !== 'undefined') {
      // firebase.analytics();
      // firebase.performance();
    }
  }
}

export default firebase;
```

For demonstration purposes I'll stick to auth and firebase not necessarily every POST req. I have made a sign in method closely following [this example](https://github.com/firebase/quickstart-nodejs/tree/master/auth-sessions).

```js
// signin -> get user token and csrf cookie
// getCookie comes directly from the example above
export const getCookie = (name) => {
  const cookie = document.cookie.match('(^|;) ?' + name + '=([^;]*)(;|$)');
  return cookie ? cookie[2] : null;
};

export const signInWithEmailAndPassword = async ({ email, password }) => {
  try {
    if (!email || !password) {
      throw 'Email and password are required.';
    }

    const { user } = await firebase
      .auth()
      .signInWithEmailAndPassword(email, password);

    if (!user) throw 'User sign in failed';
  
    if (!user.emailVerified) throw 'Email is not verified.';

    const idToken = await user.getIdToken(true);

    const csrfToken = getCookie('csrfToken');

    return fetch('/api/session', {
      method: 'POST',
      body: JSON.stringify({ idToken, csrfToken }),
      headers: {
        contentType: 'application/json',
      },
    });
  } catch (error) {
    throw error;
  }
};
```

The example linked above uses node/express back end making it difficult to translate to Next.js which doesn't have a good way to add middleware to pages and api, of course you can always create a custom server in Next.js but I wanted to try and keep the already optimized Next.js server.



Now onto the api

```js
// post session -> verify token and csrf -> set session cookie
export default async function handler(req, res) {
  const { method } = req;
    
    if(method === 'POST') {

      const body = JSON.parse(req.body);
      const idToken = body.idToken || '';
      const csrfToken = body.csrfToken || '';
		// validate server csrf and client csrf
      if (csrfToken !== req.cookies.csrfToken) {
        res.status(401).send('Unauthorized request');
        return;
      }

      const expiresIn = 60 * 60 * 24 * 7 * 1000;
        // verify auth token to create a new session cookie
       return admin
         .auth()
         .verifyIdToken(idToken)
         .then((decodedClaims) => {
         if (new Date().getTime() / 1000 - decodedClaims.auth_time < 5 * 60) {
          return admin
            .auth()
            .createSessionCookie(idToken, { expiresIn: expiresIn });
            }
            throw new Error('Session expired');
          })
          .then((sessionCookie) => {
           
            const isProd = process.env.NODE_ENV === 'production';
            const options = {
              maxAge: expiresIn,
              httpOnly: true,
              secure: isProd,
              sameSite: 'strict',
           // change domain for production when prod domain is set up
          // domain: isProd ? 'example.com' : 'localhost',
              domain: 'localhost',
              path: '/',
            };

          res.setHeader(
            'Set-Cookie',
            cookie.serialize('_session', sessionCookie, options)
          );

         res.status(200).end(JSON.stringify({ status: 'success' }));
        })
        
       .catch((error) => console.log(error));
    }
}
```

If you had already followed along with the example heres where the real solution begins.

```js
import '../styles/globals.css';
import { useEffect } from 'react';
/* 
setter/getter for js readable cookies -> renamed on my end to JSCookie, docs will just have "import Cookie from 'js-cooke' ", I don't like having the package names look so simuliar
 */
import JSCookie from 'js-cookie';
// for cyptographically random strings
import { v4 as uuidv4 } from 'uuid';
// to parse cookies
import cookie from 'cookie';

function MyApp({ Component, pageProps }) {
    // when the app is built check for existing csrf
  useEffect(() => {
    const csrf = cookie.parse(document.cookie).csrfToken;
    if (!csrf) {
        // set a new csrf
      JSCookie.set('csrfToken', uuidv4(), {
        path: '/',
        expires: new Date(Date.now() + 5 * 60000),
        sameSite: 'strict',
        // ...opts
      });
    }
    return () => {
        // app cleanup
      JSCookie.remove('csrfToken', { path: '/' });
  }, []);

  return <Component {...pageProps} />;
}

export default MyApp;

```

At first I tried setting [custom headers](https://nextjs.org/docs/api-reference/next.config.js/headers) (take a look, it may solve other issues), but I found that the same cookies would be persisted across tabs and browsers, yes even if you have one of each chrome, firefox, ie, brave, ...etc (i tried -_-), because customizations in `next.config.js` are set at build time and would be the same no matter how many different tabs or browsers you have open.

With this set up the cookie will stay valid within the same session until it expires. Things to add later however would be things like when the user logs out reset the csrf, or for added security you can refresh the csrf on each req by: 

```js
useEffect(() => {
    // remove the validation and every render will set a new cookie
    // const csrf = cookie.parse(document.cookie).csrfToken;
    // if (!csrf) {
      JSCookie.set('csrfToken', uuidv4(), {
        path: '/',
        expires: new Date(Date.now() + 5 * 60000),
        sameSite: 'strict',
      // });
    }
    return () => {}
        JSCookie.remove('csrfToken', { path: '/' });
    };
```

however its a usually better to refresh for each user session and/or each form request/submission.
