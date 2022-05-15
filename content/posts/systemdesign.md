---
title: "System Design"
date: 2022-05-12T21:40:01+07:00
draft: false
author:
    - Khang Pham
tags: ["Programming"]
ShowToc: true
ToCOpen: false
---

# Problem

Nowadays, blogs are becoming more and more popular. Anyone can write blogs. There are two simple steps to make a blog/homepage for yourself or your company:

- It seems obvious that firstly you need to write out what you want your blog to display. There are some light frameworks available that can help you do that (e.g Hugo, Wordpress ...)
- After making your own interesting blog/homepage which entice a massive number of people around the world, you've got to ensure that your website is able to withstand that number of people

In this post, I am going to propose to you a system design structure (on a surface level) that can solve the problem of 10k tps.

# Solution (horizontal scaling)

In this specific case in which you want to serve a lot of people (for example 10000 people are supposed to read your post), all of the requests are kind of equal in terms of weight. I suggest having a load balancer using round-robin method to direct DNS requests to DNS servers number 1 2 3 ... n alternately (DNS servers take a DNS request and returns an IP address of a server).

![Load Balancer](/img/systemdesign/loadbalancer.jpg)

For example, let's say you have 3 servers (with 3 DNS servers), user1 sends a DNS request to the load balancer and then it directs it to DNS server1 which in turn returns an IP address of server1. Simultaneously, user2 also sends a DNS request to the load balancer. User2 gets a response of an IP address of server2. So on and so forth. By doing this, your website is able to handle multiples requests at the same time.

Going a bit further than that, if you do not modify your posts very often, then you can store something like html file of a particular post in reader's cache memory in order to reduce the latency.

If your homepage is a static web (which this case is), then I think you're already good to go. Otherwise, you need to dive deeper into session issues, data issues or something like that to appropriately design a system for serving dynamic websites.

***Note: You need to modify a bit if applying to other services such as e-commerce websites. Because they have sessions, another issue will arise.**
