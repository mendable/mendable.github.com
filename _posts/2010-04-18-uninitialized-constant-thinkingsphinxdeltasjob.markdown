--- 
layout: post
title: uninitialized constant ThinkingSphinx::Deltas::Job
wordpress_id: 284
wordpress_url: http://www.mendable.com/uninitialized-constant-thinkingsphinxdeltasjob/
---
If you are getting this error during a sphinx index, it is likely because you have a copy of the gem 'thinking-sphinx' installed, but you also have an older version of the gem from the old github repository also installed (eg, freelancing-god-thinking-sphinx). 

The solution is to remove the old github gem version.

Also this should be a reminder that github will stop hosting gems in October 2010, so you need to allocate time to switching all of your gem dependencies be running gems from github to their respective rubygems/gemcutter versions.
