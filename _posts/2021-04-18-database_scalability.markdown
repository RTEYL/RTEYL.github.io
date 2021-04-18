---
layout: post
title:      "Database Scalability"
date:       2021-04-18 20:20:19 +0000
permalink:  database_scalability
---


Database scalability is a big topic with a lot to cover so I’d like to some up some of the higher level concepts. It’s important to note that just because you can scale doesn’t mean you should. If you have ten users and ten products there's no reason to worry about scaling. Scaling to early can be a waste of resources and time instead, you should wait until the load on the server starts to be cumbersome.

By the time the database needs to be scaled you should have already been working on having good system design and architecture.

Reuse the data already stored in the app as much as possible to limit the amount of requests and queries to the database. Store the results of common queries to reduce repetition. Avoid complex operations and blocking code that can stop or drastically slow down the server.

Cache database queries by storing the results from the first request in memory and have subsequent requests pull from memory instead of making new requests. From there you can strategies updates to the cache to optimize performance.

Improve data retrieval speeds by adding more indexes to the database. Indexes are retrieved much faster than queries that need to search row by row, column by column, and table by table.

Use JWT’s, cookies, or session storage to store session data client-side to reduce the load from the database.

Horizontal scaling:

- add more servers (sharding)
- merge data into one database
- infinitely scalable

vertical scaling:

- improve capacity
- improve hardware
- is limited by current technology

SQL:

- schema
- relations
- distributed data
- horizontal scaling is difficult/impossible
- vertical scaling is still useful
- limitations for lots of queries per second

NoSQL:

- schema-less
- no or few relations
- merge or nest data
- horizontal and vertical scaling useful
- highly performant

References:

https://medium.com/swlh/5-database-scaling-solutions-you-need-to-know-e307570efb72

https://www.red-gate.com/simple-talk/sql/performance/designing-highly-scalable-database-architectures/

https://towardsdatascience.com/system-design-101-b8f15162ef7c
