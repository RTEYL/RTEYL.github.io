---
layout: post
title:      "Technologies You Need For a Monorepo"
date:       2021-06-19 15:58:12 -0400
permalink:  technologies_you_need_for_a_monorepo
---


​	There are advantages and disadvantages of monorepos and many who use them have their own reason for why it works best for them. When I first started looking into monorepos I found my self wondering, what kind of technologies are used to manage vast quantities of code and collaboration? To answer this question Ill go over some of these technologies, what they are, and how they work for monorepos.

​	As one of the most used JavaScript packages, regardless of the type of repository, ESLint still plays a major role here. ESLint is a static code analyses tool for JavaScript to quickly find problems. Many of the problems ESLint finds can be fixed automatically removing the need for developers to go in and fix issues by hand. ESLint is fully customizable to be able to pre-process code, use custom parsers, and make your own rules to work exactly the way you want to for each individual project.

You can add ESLint to your project with

`yarn add eslint --dev`

then initialize it with `yarn run eslint --init`

After initialization, a `.eslintrc` file will be added to the project and here is where you can completely customize ESLint. It's designed to be flexible and customizable in that you can turn on/off any rule and set the severity, or how it will behave when issues are found, of it. Some of the main configurables, other than rules, are the environments, global variables, and plug-ins. ESLint’s documentation is quite extensive and easy to follow along with. You can find detailed descriptions of all the rules on the [ESLint website](https://eslint.org/). You can also choose how ESLint formats the appearance of linting results to best suit your stylistic/programmatic needs.

​	Husky Is another package that you might not see in every monorepo but, It can still be extremely helpful. Husky is a tool to help developers manage git hooks and allows us to run scripts in different stages in the git life-cycle. Having the ability to run scripts on different git commands can help simplify automations with GitHub actions and have script run for a developer in a local environment before code is pushed to GitHub.

​	Another major player in monorepos and is made specifically for them is Lerna. Lerna is a library tool to help manage JavaScript monorepos. It can be used with either NPM or Yarn package managers but, in this case, I’ll continue with the Yarn package manager and take advantage of Yarn Workspaces. Lerna assists in hoisting dependencies from the individual packages into the root level. Allows code and package sharing across multiple packages with a single node modules folder at the root level. Utilizing Lerna with Yarn workspaces you may for example export components from one app to be used in another which of course extends to the entire package. Lerna also allows us to be able to run scripts across multiple packages simultaneously or individually. This flexibility can also allow us to run scripts selectively on specific packages. For example we can run testing, CI/CD, linters, ...etc. only on packages that have code changes.

Add Lerna with 

`npm i -D lerna` or `yarn add lerna -D`

Initialize Lerna with `npx lerna init`

After initialization a `lerna.json` will be added with some defaults to the root of the monorepo.

​	Similar, and often paired together with ESLint, Prettier is also one of the most used NPM packages. Prettier is an opinionated code formatter that supports many different languages and text editors. It helps maintain a single style across entire projects and the many developers that may use it. Most developers will have their own opinions and style when it comes to how they code and how they want their code to look or be formatted but by adding scripts and configurations to your project you can enforce certain styles. Prettier is often combined with a linter to help enforce those rules and automatically format/fix code. One of the most popular combinations is to combine Prettier with ESLint. This combinations allows ESLint to help developers not only fix code but also formats it to conform with the projects Prettier configuration.



resources:

[Lerna](https://lerna.js.org/eslint)

[ESLint](https://eslint.org/)

[Husky](https://typicode.github.io/husky/#/)

[Prettier](https://prettier.io/)
