---
layout: post
title:      "Some Neat resources for JavaScript Data Structures"
date:       2021-02-21 22:21:40 +0000
permalink:  some_neat_resources_for_javascript_data_structures
---


​	I've been on a path to learning more about data structures this last week and decided to dedicate this blog to some cool things I found that others might not know about.

​	Notably, the first thing I thought of after watching so many people code the structures out for each project is 'shouldn't there just be a package for this?'. See for many reasons most languages don't have these complex data structures available out of the box [read me about that here](https://softwareengineering.stackexchange.com/questions/65139/should-data-structures-be-integrated-into-the-language-as-in-python-or-be-prov) and [here](https://softwareengineering.stackexchange.com/questions/191653/why-many-programming-languages-have-only-2-data-structures-arrays-and-hashes). I do understand that It's to teach people what they look like and how to build them.

  That being said, [this here](https://github.com/eyas-ranjous/datastructures-js) `datastructures-js` npm package works like any other with `import/require`:

```js
// FROM datastructures-js docs
const Trie = require('@datastructures-js/trie');
// OR
import Trie from '@datastructures-js/trie';
```

The documentation is clear, well written and easy to understand. plus, it shows the time complexity of each method.

​	I also found an amazing [storybook](http://tylerhawkins.info/js-data-structures-and-algorithms/storybook-dist/?path=/story/data-structures-doubly-linked-list--node-visualizer) that helps visualize a data structure and use common methods on it which allows you to see how the structure changes when changes are made to it. It also provides algorithms. The same person that made the story book also has a published [npm package](https://github.com/thawkin3/js-data-structures-and-algorithms) `js-data-structures-and-algorithms` in which you can `import/require` data structures an algorithms.

Resources:

[SO data structure integration(1)](https://softwareengineering.stackexchange.com/questions/65139/should-data-structures-be-integrated-into-the-language-as-in-python-or-be-prov)

[SO data structure integration(2)](https://softwareengineering.stackexchange.com/questions/191653/why-many-programming-languages-have-only-2-data-structures-arrays-and-hashes)

[Evas - datastructures-js - npm](https://github.com/eyas-ranjous/datastructures-js)

[Tyler Hawkins Storybook](http://tylerhawkins.info/js-data-structures-and-algorithms/storybook-dist/?path=/story/data-structures-doubly-linked-list--node-visualizer)

[Tyler Hawkins - js-data-structures - npm](https://github.com/thawkin3/js-data-structures-and-algorithms)

Other:

[FCC YT data structures and algoriths](https://www.youtube.com/watch?v=t2CEgPsws3U)

[Hackerrank](https://www.hackerrank.com/domains/data-structures)


