---
layout: post
title:      "URL Queries With Express"
date:       2021-03-07 22:50:49 +0000
permalink:  url_queries_with_express
---


`req.query.<key>`

`req.query` contains any URL queries from the request in a key value pair format

in this example: 

`http://localhost:3000/products/:params?category=home&priceLimit=100`

<-	     					pathname		     	  ->  <-Params->  <-        	      				Query			     	      		->  



The start of a query is always `?` and is supported by all browsers, then is followed by key/value pairs like in the about example:

```js
// http://localhost:3000/products/:params?category=home&priceLimit=100

req.query.catagory // 'home'
req.query.priceLimit // '100'

// as an object
req {
  query{
    category: 'home'
    priceLimit: '100'
  }
  ...req
}
```

Since query is an object you can also have arrays, objects and nesting.

for example:

```js
// http://localhost:3000/products/:params?category[]=home&category[]=garden&priceLimit=100

req.query.catagory // ['home', 'garden']
req.query.priceLimit // 100

// as an object
req {
  query{
    category: ['home', 'garden']
    priceLimit: '100'
  }
  ...req
}
```

Notice the syntax  category[]= to denote the use of an array, you can also leave out the brackets `[]` to have the same effect but may have unexpected results [see this](https://evanhahn.com/gotchas-with-express-query-parsing-and-how-to-avoid-them/).

with additional nesting you can achieve a good object structure to easily and efficiently access long queries:

```js
// http://localhost:3000/products/:params?product[category]=home&product[category]=garden&product[priceLimit]=100&search[productName]=planter

req.query.product.catagory // ['home', 'garden']
req.query.product.priceLimit // '100'
req.query.search.productName // 'planter'

// as an object
req {
  query{
    product {
      category: ['home', 'garden']
      priceLimit: '100'
    },
    search{
      productName: 'planter'
    }
  }
  ...req
}
```



The last thing to note here is security and while HTTPS provides SSL encryption for all requests there are still vulnerabilities and sensitive data should never be used in the url and some will suggest to only send sensitive data as params with `POST`, `PUT`, `DELETE` requests because sensitive information may be logged by server logs or browser history.



resources:

[REST basics](https://saipraveenblog.wordpress.com/2014/09/29/rest-api-best-practices/)

[SO query-string security](https://stackoverflow.com/questions/2629222/are-querystring-parameters-secure-in-https-http-ssl)

[Evan Hahn’s express gotchas](https://evanhahn.com/gotchas-with-express-query-parsing-and-how-to-avoid-them/)

[Express docs](https://expressjs.com/en/api.html)


