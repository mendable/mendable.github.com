--- 
layout: post
title: Where are your users from? - (By SQL Email Domain)
wordpress_id: 4
wordpress_url: http://www.mendable.com/?p=4
---
Ever wondered who your users use as their email provider? Who are the biggest email providers of the users on your website? Luckily SQL has the answer in 1 easy step using the <a href="http://dev.mysql.com/doc/refman/5.1/en/string-functions.html#function_substring-index">SUBSTRING_INDEX</a> string function.<br />

{% highlight sql %}
mysql> SELECT COUNT(*) AS Total, 
  SUBSTRING_INDEX(email, '@', -1) AS Domain
  FROM users 
  GROUP BY SUBSTRING_INDEX(email, '@', -1)
  ORDER BY COUNT(*) DESC
  LIMIT 10;
+-------+------------------+
| Total | Domain           |
+-------+------------------+
|  1000 | hotmail.com      |
|   999 | yahoo.co.uk      |
|   888 | aol.com          |
|   777 | btinternet.com   |
|   666 | yahoo.com        |
|   555 | hotmail.co.uk    |
|   444 | gmail.com        |
|   333 | ntlworld.com     |
|   222 | tiscali.co.uk    |
|   111 | eircom.net       |
+-------+------------------+
10 rows in set (0.00 sec)

{% endhighlight %}