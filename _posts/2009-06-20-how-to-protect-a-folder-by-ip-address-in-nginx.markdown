--- 
layout: post
title: How to protect a folder by IP address in Nginx
wordpress_id: 113
wordpress_url: http://www.mendable.com/?p=113
---
To protect a folder by IP address in nginx is fairly simple, see the example below that demonstrates only allowing access to the /protected folder for people in the 192.168.0.* network.
{% highlight nginx %}
server {
  ....
  location /protected {
    allow 192.168.0.0/24;
    deny all;
  }
  ...
}
{% endhighlight %}
