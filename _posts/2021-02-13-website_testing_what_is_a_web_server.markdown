---
layout: post
title:      "Website testing: What is a web server? "
date:       2021-02-14 01:05:16 +0000
permalink:  website_testing_what_is_a_web_server
---

​	localhost is a loopback addresses in which the IP point to your computer. A loopback address is a special IP address `reserved `for use in testing network cards that always looks like `127.x.x.x` and it needs a network interface to communicate with itself (via: LAN, WAN, etc.) [see the difference](https://network-support.com/what-is-the-difference-between-a-lan-and-wan-network/)

​	How is an address used for testing network cards applied to testing websites? With HTTP software, the loopback address connected to the internet becomes a web server capable of serving your HTTP responses and sending HTTP requests to other servers. On the other hand, with local server software, a  server can communicate across its local network without an internet connection.

​	The difference between local and web servers is predominantly the software a server is running. They both function is similar ways but the communication path varies.

​	For example,  `Express.js` creates an http server utilizing Node’s `http` core module and it then listens to the specified port for incoming requests and sends back your response. Express is built on top of Node’s `http` module and with that being said a similar configuration can be made with Node explicitly or the many other Express alternatives.



resources: 

https://www.hostinger.com/tutorials/what-is-localhost

https://stackoverflow.com/questions/52961775/what-is-the-difference-between-local-server-and-a-web-server#:~:text=Not%20much%20more.,that%20can%20serve%20web%20pages.&text=A%20local%20server%20is%20again,the%20local%20network%20or%20LAN.

https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server

https://evanhahn.com/understanding-express/
