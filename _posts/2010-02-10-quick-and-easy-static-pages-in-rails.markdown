--- 
layout: post
title: Quick and easy static pages in Rails
wordpress_id: 235
wordpress_url: http://www.mendable.com/?p=235
---
Sometimes you have a Rails app, the core functionality is not a CMS, and you need to add some static pages to your app that don't get changed often, eg, an about us page, privacy policy, etc.

This post shows you a quick and easy way to do that in about 60 seconds, without using a database model or having to write any sort of CMS.

1) First off, add some routes for your static pages. Here I want an FAQ page, About page, Privacy page, and Terms and Conditions pages.

{% highlight ruby %}
map.with_options :controller => 'static_pages', :action => 'show' do |static_page|
  static_page.faq 'faq', :id => 'faq'
  static_page.about 'about', :id => 'about'
  static_page.privacy 'privacy', :id => 'privacy'
  static_page.terms_and_conditions 'terms_and_conditions', :id => 'terms_and_conditions'
end
{% endhighlight %}
 
 
2) Add a functional test so we know it works now, and we can check we don't retrospectively break it later. Create a new file in test/functionals/static_pages_controller_test.rb:

{% highlight ruby %}
require File.dirname(__FILE__) + '/../test_helper'

class StaticPagesControllerTest < ActionController::TestCase

  should_route :get, '/privacy', :action => 'show', :id => 'privacy'
  should_route :get, '/terms_and_conditions', :action => 'show', :id => 'terms_and_conditions'
  should_route :get, '/faq', :action => 'show', :id => 'faq'
  should_route :get, '/about', :action => 'show', :id => 'about'

  %w(privacy about terms_and_conditions faq).each do |static_page_template|
    context "Accessing the #{static_page_template} page" do
      setup do
        get :show, :id => static_page_template
      end

      should_respond_with :success
      should_render_template static_page_template
    end
  end
end
{% endhighlight %}

This is fairly simple but covers everything we need it to. It just checks your routing is setup and the pages render from the static template files correctly.

3) Create your controller, app/controllers/static_pages_controller.rb:
{% highlight ruby %}
class StaticPagesController < ApplicationController
  
  def show
    page = params[:id]
    render :template => "static_pages/#{params[:id]}"
  end

end
{% endhighlight %}

Again, really simple. Use a RESTful 'show' method to select a template to render, then render it.


4) from a shell, make a directory for your pages to live in, and create the empty files:
{% highlight tcsh %}
$ mkdir app/views/static_pages
$ touch app/views/static_pages/about.html.erb
$ touch app/views/static_pages/faq.html.erb
$ touch app/views/static_pages/privacy.html.erb
$ touch app/views/static_pages/terms_and_conditions.html.erb
{% endhighlight %}

5) Check it works:
{% highlight tcsh %}
$ ruby test/functional/static_pages_controller_test.rb 
Loaded suite test/functional/static_pages_controller_test
Started
............
Finished in 0.156958 seconds.

12 tests, 24 assertions, 0 failures, 0 errors
{% endhighlight %}

Done! Now put the content in your pages, test, check in and deploy.
