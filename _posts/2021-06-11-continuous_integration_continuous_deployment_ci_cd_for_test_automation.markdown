---
layout: post
title:      "Continuous Integration/Continuous Deployment (CI/CD) for Test Automation"
date:       2021-06-11 20:10:49 +0000
permalink:  continuous_integration_continuous_deployment_ci_cd_for_test_automation
---


​	There’s a lot of things that can be done with various CI/CD tools but, in the scope of testing we really only need to focus on test automation. Since we’ll be using GitHub to host this project we can utilize GitHub Actions to automate our tests. GitHub Actions allow developers to automate the development life cycle.

​	In order for tests to be automated successfully, scripts should be created to be run during the workflow. For example, to automate tests we can set up a workflow to run checks and ensure that tests are passing before merging new changes to products. Some examples of necessary scripts, packages to be created are unit/integration tests Jest and e2e tests with Cypress. For automation Circle CI and GitHub Actions or a combination of both are very popular with JavaScript applications.

  The Jest testing framework is widely popular in JavaScript applications and for React or React frameworks is often paired with React Testing Library. Cypress allows you to create seamless end-to-end testing on a cloud server and can be added to your automation pipeline.

​	There are also tools to help with developer experience and and formatting. Eslint is a static code analysis tool to help developers identify, avoid, and prevent error prone JavaScript code. You can add Eslint to most text editors and run to automatically fix many issues. Add Eslint to an automation pipeline with your own rules to keep code clean and functional in the way you want it no matter how other developers have their own configured.

​	While Eslint is great for catching common errors, preventing bug, ...etc, it is often paired with a code formatter. Prettier is a code formatter that well, formats your code to look and style in a configurable way. Everyone has their own preferences in code formatting but when working with more developers having a single format across the application is very important.

  Include Prettier and Eslint in the automation of your project to ensure uniformity. Automating these processes with tests can greatly improve quality and developer experience. Running tests automatically adds confidence to your push and pull requests.
	
	Resources:

[Eslint](https://eslint.org/)

[Prettier](https://prettier.io/)

[CircleCI](https://circleci.com/)

[Jest](https://jestjs.io/)

[Cypress](https://www.cypress.io/)


