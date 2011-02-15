--- 
layout: post
title: Git Submodules and Capistrano
wordpress_id: 7
wordpress_url: http://mendable.wordpress.com/2009/04/22/git-submodules-and-capistrano/
---
Small point of reference, if you use git submodules in your rails application, then remember to add:
<blockquote>set :git_enable_submodules, true</blockquote>
to your deploy recipe. This is not very well documented as a lot of documentation on the web for capistrano was written before <a href="http://dev.rubyonrails.org/changeset/8755">this feature was included</a>, and before git was widely used.
