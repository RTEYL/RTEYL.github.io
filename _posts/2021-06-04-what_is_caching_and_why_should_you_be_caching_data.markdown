---
layout: post
title:      "​	What is caching and why should you be caching data?"
date:       2021-06-05 01:33:03 +0000
permalink:  what_is_caching_and_why_should_you_be_caching_data
---


​	A cache is a storage  method for subsets of data that can be read at high speeds so that future requests for data can be received much faster. We see and use cached data everyday, It’s one of the reasons our favorite websites can deliver content as quickly as it does. When a request is made to Facebook.com for example, much of the static data, or data that doesn’t change often, is loaded quickly from cache while other data is retrieved from a server.

​	Caching data can significantly reduce loading times and improve the User Experience (UX) of the application. Another example would be like driving out of the way to a farm and buying fresh produce vs going to a farmers market or a grocery store just down the street. You won’t only find caching in user-facing pages however, caching is also found in [Domain Name Systems](https://blog.devtylerjones.com/what_is_dns_domain_name_system) (DNS), [Content Delivery Networks](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) (CDN), backend servers, and databases.

​	To reap the benefits of data caching, we can cache some of the data for especially intensive requests and selectively decide when it’s necessary to update the cache. Set expiration limits ([TTL](https://en.wikipedia.org/wiki/Time_to_live)) to invalidate older data and tell the browser to request new data from the server as needed. We can also take a look at Lazy Loading from cache to load as little as possible more often to quickly retrieve small sets of data, think of how Facebook loads only a small number of posts at first then as you scroll down more data is loaded.

​	Caching will also greatly reduce the load of your servers. By allowing the browser to cache data it will need fewer requests to your servers which in turn save you money and bandwidth. This can be optimized even further by using a CDN to not only help cache data but, to help balance the number of requests coming from places that may be far away from your hosted server (see [Load Balancing](https://www.citrix.com/solutions/app-delivery-and-security/load-balancing/what-is-load-balancing.html) for more info).  Many hosting platforms have some form of this out of the box already and you may not have noticed it.





Resources:

[Cloud Flare - What is a CDN](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/)

[Tyler J - What is DNS](https://blog.devtylerjones.com/what_is_dns_domain_name_system)

[CS Wiki - Caching](https://computersciencewiki.org/index.php/Cache_memory)

[Wiki - TTL](https://en.wikipedia.org/wiki/Time_to_live)

[AWS - Caching](https://aws.amazon.com/caching/)

[Brock Reece - Frontend Caching](https://medium.com/@brockreece/frontend-caching-strategies-38c57f59e254)

[Rishabh Gupta - Caching](https://dev.to/zeerorg/caching-data-in-frontend-3973)

[Citrix - Load Balancing](https://www.citrix.com/solutions/app-delivery-and-security/load-balancing/what-is-load-balancing.html)




