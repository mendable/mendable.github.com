--- 
layout: post
title: Thinking Sphinx Performance - How to Index from Slave MySQL Database
wordpress_id: 302
wordpress_url: http://www.mendable.com/?p=302
---

For one of the sites I work with, we are running a load balanced web server setup with separate MySQL database servers in master-slave configuration on the back end. Each of the web servers run their own Sphinx searchd daemon (to reduce latency of client connection/queries) using a non-distributed sphinx index.

While looking at our Munin graphs I was concerned to see the amount of bandwidth and disk IO on the master MySQL database server caused by periodic re-indexing, and that load increases in line with the number of front-end web servers currently in use.

So that got me thinking... these are huge read-only queries, a small time lag is acceptable, they *should* be happening on the Slave MySQL database.

ThinkingSphinx does not have any official documentation for how to configure this, so here is the secret to setup the Sphinx indexer to use your slave MySQL database...

<b>1/. Create a Read Only MySQL User</b>
On your Master MySQL server create a read-only MySQL user if you do not have one already. It will be replicated automatically onto the slave servers. This is just good practice, you do NOT want any changes accidentally made on your slave databases or you run the risk of breaking MySQL replication.

<b>2/. Create new production_slave environment</b>
We will use a separate Rails environment to hold our slave database and sphinx configuration, but you need to get that environment working. To do this in your rails project, copy config/environments/production.rb to config/environments/production_slave.rb. 

<b>3/. Configure database.yml</b>
In your database yml, create a new entry for your production_slave environment, pointing to your slave MySQL database, and use your read-only MySQL user.

<b>4/. Edit your config/sphinx.yml file</b>
Take a copy of your production section and duplicate it under production_slave. It is important that you use the same port number and settings of the production environment.

<b>5/. Commit your changes and deploy live</b>
Push our your code to the servers, so your servers now have access to your new production_slave environment. Edit your yml configs on the servers if your deploy process does not mange this for you.

<b>6/. Setup Sphinx Indexer for new Environment</b>
The sphinx indexer should now use the RAILS_ENV=production_slave. As this is the first time you should now run "RAILS_ENV=production_slave rake ts:in" which will automatically generate a new config/production_slave.sphinx.conf file for you that searchd and indexer can use, and generate your first set of indexes in db/sphinx/production_slave/.

<b>7/. Restart Sphinx</b>
Stop your production sphinx instance, and restart sphinx with the production_slave environment. It needs to read the same config file as your indexer in case of future schema changes etc.

<b>8/. Update RAILS_ENV for sphinx elsewhere</b>
This will largely be dependent on your setup, but you need to change your cron-jobs, init.d scripts, deploy scripts, process monitoring etc to use the production_slave environment for your sphinx daemon and sphinx indexing tasks.

That's it!

The reason this works it because of step #4, where we use the same host & port for our sphinx server for both the production and production_slave environments. Although your app continues to run as production, your sphinx server runs as production slave on the port expected in the production environment so you are now querying the sphinx daemon that is indexed from the slave MySQL database.
