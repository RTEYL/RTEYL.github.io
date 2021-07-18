---
layout: post
title:      "Mocking functions with Jest"
date:       2021-07-18 23:00:51 +0000
permalink:  mocking_functions_with_jest
---


Mocking with jest is just as it sounds, a way to simulate the behavior of modules or functions. This is especially useful when testing external dependencies, API's, and asynchronism.

To mock functions we use the `jest.fn()` method and can then specify the implementation and the return value to perform assertions on.

Similarly, jest has an automocking feature to automatically mock all modules used in the tests and rather than specifying which one to mock we specify which ones to un-mock with  `jest.unmock()`. We can tell jest to automock by adding `automock: true` to the `jest.config.js` 

To mock a specified module we can use the `jest.mock()` method and can add mock implementations of that module.

Lastly, if we dont want to mock the implementation but still want make assertions for a module or function we can use `jest.spyOn()`

We can also enable and disable automocking for specific test cases for example, if

```js
// --> code snippet from the jest documentation

// jest.config.js 
{
	"automock": true
}

// utils.js
export default {
  authorize: () => {
    return 'token';
  },
};

// some.test.js
import utils from '../utils';

jest.disableAutomock();

test('original implementation', () => {
  // now we have the original implementation,
  // even if we set the automocking in a jest configuration
  expect(utils.authorize()).toBe('token');
});
```

With automocking enabled above we disable the automock feature with `jest.disableAutomock()` and we can have the inverse with `jest.enableAutomock()`

```js
// --> code snippet from the jest documentation
// utils.js
export default {
  authorize: () => {
    return 'token';
  },
  isAuthorized: secret => secret === 'wizard',
};

// some.test.js
jest.enableAutomock();

import utils from '../utils';

test('original implementation', () => {
  // now we have the mocked implementation,
  expect(utils.authorize._isMockFunction).toBeTruthy();
  expect(utils.isAuthorized._isMockFunction).toBeTruthy();
});
```





References:

https://jestjs.io/docs/jest-object
