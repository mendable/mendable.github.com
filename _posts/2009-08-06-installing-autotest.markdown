--- 
layout: post
title: Installing AutoTest
wordpress_id: 205
wordpress_url: http://www.mendable.com/?p=205
---
I think autotest is one of the best things about developing in Rails. If you don't know, autotest runs in the background watching all of your application files. When you edit any of them, it automatically runs the sections of your test suite that apply to the modified file, and can then give you feedback as to if your tests are passing or failing. This means more time developing, less time switching windows to run rake test!

I thought I would post a note about how to correctly install autotest, as some of the other documentation is out of date.

{% highlight tcsh %}
sudo gem uninstall ZenTest
sudo gem install ZenTest
sudo gem install autotest-rails
{% endhighlight %}

Something that is missing in a lot of other online documentation is the fact that you have to install the autotest-rails gem as well, if you don't do this, you will get an error any time you try to run autotest:
{% highlight tcsh %}Autotest style autotest/rails doesn't seem to exist. Aborting.{% endhighlight %}

Another note, if you want to get desktop notifications working, <a href="http://ph7spot.com/articles/getting_started_with_autotest">this page</a> has details of how to achieve that using Mac OSX, KDE, Gnome, etc.
