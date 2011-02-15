--- 
layout: post
title: Correct Format Plugin Released
wordpress_id: 149
wordpress_url: http://www.mendable.com/?p=149
---
I have released a new Plugin for Rails, called <strong>Correct-Format</strong>.

Github URL: <a href="http://github.com/mendable/correct-format">github.com/mendable/correct-format</a>


This plugin allows you to automatically correct simple user input mistakes and format user-input without raising an ActiveRecord Error and without inserting inconsistently formatted data into your database. Using this plugin will enhance the usability and user-friendlyness of your application and increase your data integrity.

<b>You can automatically:</b>

* Make a field uppercase
* Make a field lowercase
* Capitalize the first letter of the first word (and lower case everything else)
* Capitalize the first letter of all words (and lower case everything else)
* Replace comma with period and downcase email addresses

<h2>Installation</h2>
{% highlight tcsh %}
$ git submodule add git://github.com/mendable/correct-format.git vendor/plugins/correct-format
$ git commit -am "add correct-format plugin"
{% endhighlight %}

Ensure that you have this in your config/deploy.rb:

{% highlight ruby %}
set :git_enable_submodules, true
{% endhighlight %}

so that the submodules are pulled down when you deploy your application.

<h2>Example Usage</h2>
{% highlight ruby %}
  class User < ActiveRecord::Base
    # Make usernames consistently lower case
    correct_format_downcase :username

    # Replace comma's with periods in email address, and make email address all lower case
    correct_format_email :email

    # Capitalize first letter of first word, and downcase the rest
    correct_format_capitalize :username

    # Capitalize all first letters of ALL WORDS in the string
    correct_format_capitalize_each :address_line_1, :address_line_2

    # UK Postcodes are upper case
    correct_format_upcase :postcode

    # apply a function to EVERY string field in a record
    correct_format_capitalize self.attributes.select{|k, v| self.column_for_attribute(k).type == :string }.map(&amp;:first)
  end
{% endhighlight %}

Feedback and patches welcome.
