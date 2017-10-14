---
layout: post
title: Thread safe database with FMDB (SQLite) as alternative to Core Data
categories:
- Blog
tags:
- Core Data
- CoreData
- fast
- FMDB
- iOS
- iPhone
- Multithreading
- performance
- speed
- SQLite
- Thread
- Threads
- Threadsafe
status: publish
type: post
published: true
meta:
  structured_content: '{"oembed":{},"overlay":true}'
  _thumbnail_id: '79'
---

![](/squarespace_images/static_545299aae4b0e9514fe30c95_54529a29e4b025a90f45cc50_54529a2ee4b025a90f45ce4c_1414699607588_sqlite-logo.png_)
  


The apps I developed recently required saving and loading thousands of objects within a very short time on multiple threads, mostly for reasons of caching and offline mode support. I started implementing this behavior in 
Core Data, which caused a few problems related to thread safety and performance.

I tried using 
[FMDB](https://github.com/ccgus/fmdb) as an alternative to 
Core Data, which works surprisingly great. There is not much information about multithreading and thread safety available, that's why I'm writing this blog post.

To apply the MVC principal, I assume you create a class just for reading/writing into/from the database. You should never access the database directly from outside of it.

After trying different approaches of multithreading, this turns out to be the best solution for my projects.

First of all you need a database queue (provided by 
[FMDB](https://github.com/ccgus/fmdb)), an NSOperationQueue to queue your queries and a lock for the database.

If you use a singleton for your database, set up your static variables.

Basically there are 2 use cases now: Read from database and Write to database

To 
**read**
, all you have to is:

To**write**
into the database, you have to use the writeQueue:

That's all you need to really easily save/read data using 
[FMDB](https://github.com/ccgus/fmdb). I use a little wrapper that matches properties with the according columns inside the database.
