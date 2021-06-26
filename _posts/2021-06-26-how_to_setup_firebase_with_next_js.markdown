---
layout: post
title:      "How to Setup Firebase With Next.js"
date:       2021-06-26 19:55:45 +0000
permalink:  how_to_setup_firebase_with_next_js
---


To get started we need to install two dependencies with `npm install` or `yarn add`.

`firebase` && `firebase-admin`

Next we'll create the folder structure for our firebase files:

```
firebase/
	-initFirebase.js
```

For the next step you'll need to

1. Go to your firebase console
2. Select your project
3. Click the settings cog on the left panel
4. click project settings
5. scroll down towards the bottom and you will see "SDK setup and configuration"

Now that you have the necessary keys, create a `.env.local` file to store them in

```bash
//.env.local
NEXT_PUBLIC_FIREBASE_API_KEY=
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=
NEXT_PUBLIC_FIREBASE_PROJECT_ID=
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
NEXT_PUBLIC_FIREBASE_APP_ID=
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=


//for firebase-admin
FIREBASE_CLIENT_EMAIL=

FIREBASE_PRIVATE_KEY=
```

Now we can setup the firebase connection and export it for use in the app.

```js
//initFirebase.js
import firebase from 'firebase/app'

const clientCredentials = {
  apiKey: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
  authDomain: process.env.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.NEXT_PUBLIC_FIREBASE_PROJECT_ID,
  storageBucket: process.env.NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.NEXT_PUBLIC_FIREBASE_APP_ID,
  measurementId: process.env.NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID,
};

export default function initFirebase() {
  if(!firebase.apps.length){
    firebase.initializeapp(clientCredentials)
    
    if('measurementId' in clientCredentials){
      firbase.analytics()
      firebase.performance()
    }
  }
}
```

All thats left is to do is import and initialize it

```javascript
import firebase from '../firebase/initFirebase'

firebase()
```

to write data to the firestore we need to use some built in methods to run. Let's use an example component to do just that:

```js
import firebase from 'firebase/app';
import 'firebase/firestore';

export const writeData = () => {
  try {
     firebase
        .firestore() //connect
        .collection('myCollection') //specify collection
        .doc('my_document') //specify document
        .set({ name: 'username', email: 'dont@me.com' }) //document data
        .then(console.log('success')); // YAY!!
   } catch (error) {
      console.log(error); // -_-
   }
};
```

to read data from the firestore we apply a very similar process

```js
import firebase from 'firebase/app';
import 'firebase/firestore';

export const readData = () => {
    try {
      firebase
        .firestore() //connect
        .collection('myCollection') //specify collection
        .doc('my_document')	//specify document
        .onSnapshot((doc) => {	//retrieve data snapshot
        	if(!doc.exists()){ //handle errors
            throw new Error()
          }
          console.log(doc.data());
        });
    } catch (error) {
      console.log(error);
    }
};
```

And there you have it! you can now read and write to your firebase project. To learn more about firebase read their [docs](https://firebase.google.com/docs?authuser=0). Also checkout the [examples](https://github.com/vercel/next.js/tree/canary/examples) on the Next.js repo for more guidence.
