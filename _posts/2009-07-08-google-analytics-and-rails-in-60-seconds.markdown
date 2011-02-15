--- 
layout: post
title: Google Analytics and Rails in 60 Seconds
wordpress_id: 167
wordpress_url: http://www.mendable.com/?p=167
---
This quick post will show you how to install Google Analytics into your Rails powered website in less than 60 seconds.

Installing Google Analytics into your Ruby on Rails website is very easy. First of all, ensure you are signed up for <a href="http://www.google.com/analytics">Google Analytics</a> if you are not already.

Once you have your Analytics account setup and you are logged in, you should be able to see the website for which you are tracking statistics, and the tracking ID (will be in the format of "UA-nnnnnn-n". You will need this now.

{% highlight tcsh %}
$ mkdir app/views/common
{% endhighlight %}

Add this code to <b>app/views/common/_google_analytics.html.erb</b>:

{% highlight erb %}
<script type="text/javascript">

  var _gaq = _gaq || [];
  _gaq.push(['_setAccount', 'UA-XXXXX-X']);
  _gaq.push(['_trackPageview']);

  (function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();

</script>
{% endhighlight %}

(the latest code can be obtained from <a href="http://code.google.com/apis/analytics/docs/tracking/asyncTracking.html">here</a>)

You need to replace the "UA-XXXXX-X" in that code with your own tracking ID. Save and close.

Rails allows you to easily keep your website design easy to manage, by allowing you to use standardized site layout templates for your entire website (application.html.erb) or for specific controllers or even actions. These files are stored in the <b>app/views/layouts/</b> directory. One at a time, open each of the files in this directory, and add the following to the end of the file, just before the closing "<>/body>" HTML tag:
{% highlight erb %}
<%= render :partial => 'common/google_analytics' %>
{% endhighlight %}

All done!

If you want an alternative method of inserting your Google Analytics code into your application, there are <a href="http://www.juixe.com/techknow/index.php/2007/02/04/rails-google-analytics-plugin/">a</a> <a href="http://www.google.com/search?q=google+analytics+rails">few</a> existing plugins that will do the work (what work?) for you. Personally, for such a trivial task, why bother with a plugin? Using plugins all of the time is not the right approach, I would even consider it an anti-pattern of sorts. It creates a whole new set of maintainability problems (what if rails changes the way it works, and those plugins no longer work after upgrade? What if the Google Analytics code needs to be updated, but the plugin is no longer being maintained, but you had no idea that was the case? In every case, the decision to use plugins or not can only be decided after weighing up the pro's and con's of each choice. My general rule (some exceptions obviously) is, if I can do it in 5 minutes or less, a plugin is probably not a good idea.
