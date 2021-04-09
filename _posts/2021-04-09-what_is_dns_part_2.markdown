---
layout: post
title:      "What is DNS (part 2)"
date:       2021-04-09 22:09:37 +0000
permalink:  what_is_dns_part_2
---

This is the seccond part of my explanation on DNS [see here](https://blog.devtylerjones.com/what_is_dns_domain_name_system) for part 1. This will detail less about what DNS is and more about zones and security.

Domain controllers are a logical group of computers, users, printers that share the same database. If a Domain contains child domains or sub-domains Active Directory considers the structure a forest because they share the same Active Directory schema

Active Directory zone replication is used when DNS records are updated the changes are automatically replicated. Active Directory can quickly compress and replicate data between sites securely. Zones are replicated to all domain controllers providing redundancy and security which allows traffic to flow in the event a server goes down. Additional security  to ensure only authorized clients can update records in the DNS zone.

DNS forwarding will forward queries for external DNS names to servers outside of its network. DNS conditional forwarding is similar to DNS forwarding however it will only forward queries from specific domains. Conditional forwarding improves name resolution for forests or domains that would otherwise not be connected. Not recommended if the configuration is likely to change.

DNS delegation allows you to delegate admin authority and has various performance improvements.

Some notable DNS security techniques:

- DANE(DNS Authentication of Named Entities)

  Tells the client who to expect a certificate from and helps to prevent man-in-the-middle attacks.

- DNS Cache Locking

  Sets the TTL(Time to Live) amount so the cache wont be updatable within that time frame to mitigate DNS cache poisoning

- Response Rate Limiting

  Prevents (Distributed Denial of Service)DDOS attacks by limiting the number of similar requests by not responding to potentially malicious attacks.

- DNS Socket Pool

  Uses a random port in a large pool of available ports to avoid cache tampering.

- DNS Load Balancing

  Round Robin is a popular method of load balancing to balance the network load. Netmask Ordering enhances the Round Robin method by providing further divisions. Note, this is not a better alternative to hardware load balancing but to be used in parallel

- DNS security

  Domain Name System Security Extensions (DNSSEC) add security to the DNS protocol by validating DNS server responses. DNS without security extensions provides no security or authentication.

- DNS policies

  Allow you to assign policies to different zones for filtering, splitting, selective recursion, traffic management, and more.


