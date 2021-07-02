---
layout: post
title:      "Next.js + Firebase + Auth"
date:       2021-07-02 20:40:53 +0000
permalink:  next_js_firebase_auth
---


There are a couple of firebase auth packages that help with authentication. One is Next.js specific and auth specific called `next-firebase-auth` and the other provides functionality for firebase as a whole. Designed for react, `reacfire` has methods for lazy loading, retrieving docs, ...etc. `reacfire` also has methods to sign in and logout (not sure about 0Auth though) may want to create a custom package?!?! May want to use firebase directly then develop custom package?!?!

> NOTE:
>
> Firebase will store unlimited authenticated users via web auth
>
> Firebase offers phone auth free for the first 10k users authenticated via mobile auth
>
> We can test auth via web unlimited times HOWEVER there is still storage, read, write, and delete limitations. 1 Gibibyte (1.074 GB)/month firestore storage, 20k/day reads, 50k/day writes, and 20k/day deletes included in the free (Spark) plan and also the starting points for the pay-as-you-go (Blaze) plan.

To sign up a user with basic email and password firebase has a `CreateUserWithEmailAndPassword` method that returns the sign in data along with the user.For sign in, firebase has a `signInWithEmailAndPassword` method that also returns the sign in data along with the user. There is also an authentication observer (`onAuthSatateChanged` ) that allows you to add side effects when the auth state changes.

<sub>*code snippets are from the firebase docs</sub>

```js
//sign up

firebase.auth().createUserWithEmailAndPassword(email, password)
  .then((userCredential) => {
    // Signed in 
    var user = userCredential.user;
    // ...
  })
  .catch((error) => {
    var errorCode = error.code;
    var errorMessage = error.message;
    // ..
  });

// sign in

firebase.auth().signInWithEmailAndPassword(email, password)
  .then((userCredential) => {
    // Signed in
    var user = userCredential.user;
    // ...
  })
  .catch((error) => {
    var errorCode = error.code;
    var errorMessage = error.message;
  });

// auth observer

firebase.auth().onAuthStateChanged((user) => {
  if (user) {
    // User is signed in, see docs for a list of available properties
    // https://firebase.google.com/docs/reference/js/firebase.User
    var uid = user.uid;
    // ...
  } else {
    // User is signed out
    // ...
  }
});
```

You can also sign in and create users with auth providers like Google and Facebook.



There are two ways to access the currently signed in user, first is with the `onAuthStateChanged` observer and store the user or some user info with state management or local storage. The second and most likely preferred way, is the `currentUser` property

```js
const user = firebase.auth().currentUser;

if (user) {
  // User is signed in, see docs for a list of available properties
  // https://firebase.google.com/docs/reference/js/firebase.User
  // ...
} else {
  // No user is signed in.
}
```

You can then access the properties of the user as an instance of `User` such as, profile data, auth and provider data. You can also update a users profile information and email. Firebase also provides an easy to use email verification method `sendEmailVerification` which can be customized in the firebase console. Some other easy to use methods include `updatePassword`, `sendPasswordResetEmail`, and account deletion with `user.delete()`

```js
// update profile

const user = firebase.auth().currentUser;

user.updateProfile({
  displayName: "Jane Q. User",
  photoURL: "https://example.com/jane-q-user/profile.jpg"
}).then(() => {
  // Update successful
  // ...
}).catch((error) => {
  // An error occurred
  // ...
});  

// update email

const user = firebase.auth().currentUser;

// TODO(you): prompt the user to re-provide their sign-in credentials
const credential = promptForCredentials();

user.reauthenticateWithCredential(credential).then(() => {
  // User re-authenticated.
}).catch((error) => {
  // An error ocurred
  // ...
});

user.updateEmail("user@example.com").then(() => {
  // Update successful
  // ...
}).catch((error) => {
  // An error occurred
  // ...
});

// verify email

firebase.auth().currentUser.sendEmailVerification()
  .then(() => {
    // Email verification sent!
    // ...
  });
  
  // set password
  
  const user = firebase.auth().currentUser;
const newPassword = getASecureRandomPassword();

user.updatePassword(newPassword).then(() => {
  // Update successful.
}).catch((error) => {
  // An error ocurred
  // ...
});

// password reset email

firebase.auth().sendPasswordResetEmail(email)
  .then(() => {
    // Password reset email sent!
    // ..
  })
  .catch((error) => {
    var errorCode = error.code;
    var errorMessage = error.message;
    // ..
  });
  
// delete user
  
const user = firebase.auth().currentUser;

user.delete().then(() => {
  // User deleted.
}).catch((error) => {
  // An error ocurred
  // ...
});
```

You can also prompt for re-authentication when accessing sensitive or destructive user data to add an additional level of security.

```js
const user = firebase.auth().currentUser;

// TODO(you): prompt the user to re-provide their sign-in credentials
const credential = promptForCredentials();

user.reauthenticateWithCredential(credential).then(() => {
  // User re-authenticated.
}).catch((error) => {
  // An error ocurred
  // ...
});
```

Firebase also supports reCAPTCHA verification with a built in `RecaptchaVerifier` method. This IS required when using phone auth and helps to prevent abuse. Firebase automatically handles the validation, keys, and secrets of the reCAPTCHA provided by them. the `RecapthaVerifier` supports both a reCAPTCHA widget and invisible reCAPTCHA which does not require any user action for verification.

Similar to a 'Guest' account, firebase supports anonymous sign in to securely store user data without requiring a user to sign up. This is extremely useful to keep track of a users data while they browse the site and can easily be upgraded to a user account without data loss.

```js
firebase.auth().signInAnonymously()
  .then(() => {
    // Signed in..
  })
  .catch((error) => {
    var errorCode = error.code;
    var errorMessage = error.message;
    // ...
  });
```

To upgrade we just need to add a providers credentials to the anonymous user with the `user.linkWithCredential()`  method.

```js
var credential = firebase.auth.EmailAuthProvider.credential(email, password);

auth.currentUser.linkWithCredential(credential)
  .then((usercred) => {
    var user = usercred.user;
    console.log("Anonymous account successfully upgraded", user);
  }).catch((error) => {
    console.log("Error upgrading anonymous account", error);
  });
```

It is also possible to link a user to multiple auth providers and sign in using any of the linked providers.

```js
var googleProvider = new firebase.auth.GoogleAuthProvider();
var facebookProvider = new firebase.auth.FacebookAuthProvider();
var twitterProvider = new firebase.auth.TwitterAuthProvider();
var githubProvider = new firebase.auth.GithubAuthProvider();

auth.currentUser.linkWithPopup(provider).then((result) => {
  // Accounts successfully linked.
  var credential = result.credential;
  var user = result.user;
  // ...
}).catch((error) => {
  // Handle Errors here.
  // ...
});
```

If a user decides to unlink an auth provider, we can with the `unlink` method.

```js
user.unlink(providerId).then(() => {
  // Auth provider unlinked from account
  // ...
}).catch((error) => {
  // An error happened
  // ...
});
```


