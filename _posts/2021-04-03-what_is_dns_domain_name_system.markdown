---
layout: post
title:      "What is DNS (Domain Name System)?"
date:       2021-04-03 20:46:57 +0000
permalink:  what_is_dns_domain_name_system
---


The internet is made up of computers all over the world that communicate with each other using IP addresses. DNS translates host names to IP addresses, when you type in a website address and you’re computer doesn't already have the IP address in cache it will query a DNS server to see if it has the IP address.

There are four main types of DNS servers:

- Resolving Name Server queries other DNS servers if the record isn’t cached
- Root Name Server contains records on TLD Name Servers
- Top Level Domain (TLD) Name Servers contains records of Authoritative Name Servers within its TLD. examples (.com, .net, .org, ...etc)
- Authoritative Name Server contains DNS records mapping domain names to IP Addresses

Resources Records provide DNS based data about computers on a network:

- (SOA) Start of Authority is a record that contains information about the DNS server responsible for the particular zone. 
- (NS) Name Server indicates the zones authoritative DNS servers. 
- (A) Record that maps a fully qualified domain name (FQDN) to an IP address.
- (PTR) Pointer does the opposite of an A record in witch it maps an IP address to a FQDN.
- (CNAME) Canonical Name is an alias for a FQDN that can point traffic headed towards a.website.com to b.website.com instead.
- (MX) Mail Exchange specifies email servers for the zone
- (SRV) Service Record specifies servers for a particular service or protocol

DNS zones contain collections of DNS resource records:

- Forward Lookup Zone converts domains to IP Addresses
- Reverse Lookup Zone converts IP Addresses
- Primary Zone is the primary source of information
- Secondary Zone is a read-only copy of the primary zone, hosted on another server to use if the primary zone is unable to process queries.
- Stub Zone is also a copy of the primary zone however, it only contains records of authoritative name servers and is much less resource intensive.

There are two different types of queries, recursive and iterative, that define how a DNS server resolves queries. In a recursive query the DNS server takes responsibility for the full name resolution of a query. In an iterative query the DNS server responds with a referral to another server from its cache or zone files. Typically your computer will send a recursive query to your internet service provider’s (ISP) DNS Resolver and If your ISP doesn’t have the record in cache it sends iterative queries until resolution.

1.  Local computer -> ISP DNS Resolver
2.  ISP -> Root Name Server
3.  ISP -> TLD Name Server
4.  ISP -> Authoritative Name Server
    1. ISP sores record in cache
5.  ISP -> Local computer
    1. Local computer stores record in cache

 The DNS servers send referrals without attempting to query other servers itself. When the query is resolved by your ISP the record is cached, sent to your computer to be cached, and finally the browser can communicate with the IP address of the website.
