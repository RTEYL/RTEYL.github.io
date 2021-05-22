---
layout: post
title:      "Next.js Testing Best Practices"
date:       2021-05-22 18:46:47 -0400
permalink:  next_js_testing_best_practices
---


​	When I first started looking into Next.js and how they recommend developers test an application using it, I found very little information. Their website has no documentation on it and their GitHub readme doesn’t have any opinions on it either. If we take a look at other frameworks, Vue.js has a [guided section](https://vuejs.org/v2/guide/testing.html#Recommendations) on unit, integration, and end-to-end testing. Angular also has a [setup guide](https://angular.io/guide/testing) with example configurations. 

​	If Next.js has no documentation about testing where do we look? I scavenged the issues and discussions to find any conversation between the Next.js team and other developers. Ill put some links at the bottom to the issues/discussions where I found information on testing. There is also an extensive list of [examples](https://github.com/vercel/next.js/tree/canary/examples) on the Next.js GitHub page.

​	While there are a lot of packages and libraries to test JavaScript code, developers recommend a combination of [Facebook’s `jest`](https://jestjs.io/) testing framework, [CircleCI](https://circleci.com) and/or [GitHub](https://github.com) for automation and integration, and [Cypress](https://www.cypress.io/) for E2E (end-to-end) tests.

​	For file and folder structure, the convention is to place tests in a folder placed in the root directory named `__test__`. Inside the test folder add test configuration files and subdirectories to add separation for the different kinds of tests.

```
root/
..etc
--/__test__
---/unit
---/integration
---/..etc
..etc
```

Use jest mock functions as little as possible, if you feel like there need to be a lot of them try using an integration testing pattern instead of a unit testing pattern.

​	Next.js separates the frontend and backend into individual components and combines (builds) them at run time. Because of that, some developers believe that there should be a higher emphasis on integration testing to ensure the app is working properly when this happens. It’s also common to have lighter unit tests that are narrow in scope and emphasize the more crucial aspects of the application. With that information, it’s common to see some anti-patterns of test-driven development like the “testing cone”:

![The Evolution of the Testing Pyramid - James Willett](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSxDeeLWJGcG30AtLwggx00dBWQQnCj1DAHrQ&usqp=CAU)

Or the “testing trophy” pattern popularized by [Kent C. Dodds](https://kentcdodds.com/):

![Static vs Unit vs Integration vs E2E Testing for Frontend Apps](https://kentcdodds.com/static/c331ec0658e3d2927db08b6e4946e266/e3189/confidence-coefficient.png)

​	While this approach may seem counter intuitive to the principles of TDD we must understand that the best practices vary for every framework, library, language, ...etc. If the application being built requires a higher emphasis on integration over unit testing that doesn’t mean we can’t adhere to TDD principles. You should strive for clear, effective, and many tests with simple unit testing and high impact integration testing. 

​	It’s important to know how your application will be built to be able to test effectively. With Next.js, if you want to test backend logic from a fronted perspective (and vise versa) you need to make sure the appropriate steps and testing pattern are used to follow that flow.





Resources: 

[Vue - Testing Recommendations](https://vuejs.org/v2/guide/testing.html#Recommendations)

[Angular - Testing Recommendations](https://vuejs.org/v2/guide/testing.html#Recommendations)

[Next.js - issue #10967](https://github.com/vercel/next.js/issues/10967)

[Next.js - discussion](https://github.com/vercel/next.js/discussions/14141)

[Next.js - Examples](https://github.com/vercel/next.js/tree/canary/examples)

[Jest](https://jestjs.io/)

[Circle CI](https://circleci.com)

[GitHub](https://github.com)

[Circle CI - blog - Next Testing](https://circleci.com/blog/next-testing/)

[Cypress](https://www.cypress.io/)

[GitHub - Next.js - Jest Example](https://github.com/vercel/next.js/tree/canary/examples/with-jest)

[Kent C. Dodds - website](https://kentcdodds.com/)

[Testing JavaScript](https://testingjavascript.com/)

[Kent C. Dodds - blog - Unit VS Integration](https://kentcdodds.com/blog/unit-vs-integration-vs-e2e-tests)

[Log Rocket - blog - Testing](https://blog.logrocket.com/testing-and-error-handling-patterns-in-next-js/)


