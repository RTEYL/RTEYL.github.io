---
layout: post
title:      "JavaScript Common Data Structures and When to Use Them"
date:       2021-03-01 00:14:24 +0000
permalink:  javascript_common_data_structures_and_when_to_use_them
---


TL;DR a study guide to improve knowledge of data structures



Queues:

- ​	common use cases:
  - maintain a ‘queue’ of processes or events
  - keeping order of asynchronous actions
  - messages
- ​	uses a first in first out method of inserting and removing data
- ​	can be implement as an array or a linked list

​	example structure [github ](https://github.com/thawkin3/js-data-structures-and-algorithms/blob/master/src/data-structures/queue/src/queue.js) `js`

Stack:

- common use cases:
  - similar use cases as queue but when actions are needed in reversed order
    - most recent process executes first
    - most recently accessed data
  - browser history
  - previous action
  - previous state
- uses last in first out method of inserting and removing data
- similar to a queue

​	example structure [github](https://github.com/thawkin3/js-data-structures-and-algorithms/blob/master/src/data-structures/stack/src/stack.js) `js`

Linked List:

- common use cases:
  - implementing stacks and queues
  - undo/redo, forward/back
  - image/video carousel
  - next/previous
- tracking previous and next actions

- fast insertion deletion as the size increases
- operations can be less expensive than arrays
- works well with stacks and queues
- fast to traverse

​	example structure [github](https://github.com/thawkin3/js-data-structures-and-algorithms/blob/master/src/data-structures/linked-list/src/linked-list.js) `js`

Binary Search Tree:

- ​	common use cases:
  - search
- fast comparisons of data
- fast to search for data
- fast to insert/remove data
- can be unbalanced

​	example structure [github](https://github.com/thawkin3/js-data-structures-and-algorithms/blob/master/src/data-structures/binary-search-tree/src/binary-search-tree.js) `js`





code examples from [Tyler Hawkins](https://github.com/thawkin3/js-data-structures-and-algorithms) `js-data-structures-and-algorithms`
