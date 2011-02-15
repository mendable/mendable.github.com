--- 
layout: post
title: Compiling Power DNS on Snow Leopard
wordpress_id: 341
wordpress_url: http://www.mendable.com/?p=341
---
This is how to install PowerDNS on Mac OSX Snow Leopard 10.6.4.

1) Install 64-bit Mysql server from the MySQL dmg package available from the MySQL website

2) Install the boost c++ library by ports (at time of writing this is boost version 1.44) {% highlight tcsh %}sudo port install boost{% endhighlight %}

3) Download the authoritative server tar.gz file from [powerdns.com][], and unpack.

4) Build and Install. {% highlight tcsh %}CXXFLAGS=-DDARWIN CPPFLAGS=-I/opt/local/include ./configure \
 --with-mysql-includes=/usr/local/mysql/include/
make
sudo make install
{% endhighlight %}

5) Install a default config file, examples within the package. {% highlight tcsh %}sudo cp /usr/local/etc/pdns.conf-dist /usr/local/etc/pdns.conf{% endhighlight %}

6) Edit /usr/local/etc/pdns.conf to taste. To setup the MySQL backend, you will need to add the following to the file:{% highlight tcsh %}
----
launch=gmysql
gmysql-host=127.0.0.1
gmysql-user=root
gmysql-dbname=power_dns
----{% endhighlight %}

7) Create the database and database schema. You can find it <a href="http://doc.powerdns.com/generic-mypgsql-backends.html">here</a>.

8) Start the server.

There is an init script file in "pdns/pdns" in the tar.gz package after compiling that you can use to stop/start the server, you must have root privileges (ie, use sudo) to start/stop the server because the daemon listens on ports &lt; 1024.

You can find server log output in /var/log/system.log.
