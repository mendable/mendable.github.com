--- 
layout: post
title: Howto Display a GitHub Changelog in your application
wordpress_id: 117
wordpress_url: http://www.mendable.com/?p=117
---
In this post I will show you a simple way to display a list of your GitHub changes inside your application.

<img src="/images/posts/github_changes_screenshot.png" alt="github_changes_screenshot" title="github_changes_screenshot" width="522" height="271" style="display:block;clear:both;" />

This can be useful for so many reasons, not least helping to create a better relationship between your users and your developers. This obviously would not be applicable in all circumstances, but in many cases, having a Changelog inside your application (especially where the end users already work closely with the application developers) can enable them to see what work has been done, and what new features are available to them. This is also fantastically useful for allowing Project managers to keep track of development changes.

The code below will pull out the last 20 GitHub commit comments from your GitHub project, and display them in a table, and with a bit of CSS styling will look something like the screenshot above, or better!

{% highlight erb %}
<table border="0" width="100%">
<tr>
<th>When</th>
<th>Description</th>
</tr>
<% recent_website_changes = `git log -20 --abbrev-commit --pretty=format:"%H %cr - %s"` rescue ""   
recent_website_changes.split(/\n/).each do |ln|     
    commit_code = ln.split(/ /)[0]     
    without_commit = ln.gsub("#{commit_code} ", "")     
    commit_time = without_commit.split(/ - /)[0]     
    message = without_commit.gsub("#{commit_time} - ", "") %>
    <tr>
    <td><%= commit_time %></td>
    <td><%= message %></td>
    <td><%= link_to('view', "https://github.com/your-github-username/your-project-name/commit/#{commit_code}") %></td>
    </tr>
<% end %>
</table>
{% endhighlight %}

Another advantage of displaying your commit log publicly to other non-developers is that it can incentivize your developers to think carefully about writing better commit messages, knowing they will have to be read by others.
