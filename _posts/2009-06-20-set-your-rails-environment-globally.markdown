--- 
layout: post
title: Set your Rails Environment Globally
wordpress_id: 92
wordpress_url: http://www.mendable.com/?p=92
---
In much of the rails documentation it is suggested that when you want to run a task in a specific environment, you should set the RAILS_ENV environment variable on the command line where you are running the task. 

For example, assume you want to run <b>rake db:migrate</b> on the production system (should be done automatically by Capistrano, but anyway..), because it is the production system, you would have to declare your rails environment in the command line, so you would actually have to run <b>RAILS_ENV=production rake db:migrate</b>.

Well, this becomes tedious sometimes, and open to human error as well. Luckily there is a way around this.

The solution is simple.

Add it to your <b>/etc/environment</b> file:
<pre>RAILS_ENV=production</pre>

You may have to log out and log in again of your shell for it to take effect. Put this on your production server, and any command that runs on that production server will automatically default to the production environment. This includes cron jobs, and other users logging into the same system.

Handy!
