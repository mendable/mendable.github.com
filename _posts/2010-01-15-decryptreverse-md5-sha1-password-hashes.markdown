--- 
layout: post
title: Decrypt/Reverse MD5 / SHA1 Password Hashes
wordpress_id: 223
wordpress_url: http://www.mendable.com/?p=223
---
There is a nice little tool over on <a href="http://tmto.org">tmto.org</a> that allows you to decrypt (or should I say, reverse) MD5 or SHA1 hashes. Essentially, someone has forward hash'ed all a-z letters, put them in a database, then matches your hash against what is in the DB to find what the original string was. This is somewhat limited, it only currently does a-z by the looks of things, but a very nice idea. Should also re-enforce the benefits of double-salting your passwords with a per-size and per-user based salt as well.
