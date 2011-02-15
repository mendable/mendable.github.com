--- 
layout: post
title: Mass Find/replace for Rails Tests
wordpress_id: 18
wordpress_url: http://mendable.wordpress.com/?p=18
---
After upgrading an old project to Rails 2.3.2,my tests were failing to run, bombing out with an error like:
<pre>./test/unit/my_model.rb:4: undefined method `fixtures' for MyModelTest:Class (NoMethodError)</pre>
This was because in Rails 2.3.2, tests have to derive from ActiveSupport::TestCase, and not the old Test::Unit::TestCase.

A quick find and replace soon has us up and running again:
<code>perl -pi -w -e 's/Test::Unit::TestCase/ActiveSupport::TestCase/g;' test/unit/*.rb</code>

Effectively, this one-liner searches through all the files in the test/unit directory, and replaces all instances of "Test::Unit::TestCase" with "ActiveSupport::TestCase"
