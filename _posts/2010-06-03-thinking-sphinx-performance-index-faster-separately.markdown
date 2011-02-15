--- 
layout: post
title: Thinking Sphinx Performance - Split your re-indexing into seperate tasks
wordpress_id: 309
wordpress_url: http://www.mendable.com/?p=309
---

If you have multiple indexes, you need to consider if they all need to be re-indexed at the same time. 

For example, you might have a "users" index that changes frequently that you want to re-index from scratch or merge delta changes, and a "spare car parts" index that may only change once or twice a day. 

The accepted way to perform a re-index your data is of course to run "rake ts:index" which will re-create ALL of your indexes, but that assumes all indexes are equal and all need re-indexing at the same time - usually that is not true. When you have to re-index a *huge* index when all you really want is to update the smaller one... it's inefficient.

So, what does rake ts:index actually do?

The thinking sphinx index task works out what config file to use based on your rails environment and Rails.root directory, re-generates the config file, and then calls the sphinx indexer program with a link to the config file.

The indexer program has a whole range of options which you might like to get familiar with:
<pre>$ indexer
Sphinx 0.9.9-release (r2117)
Copyright (c) 2001-2009, Andrew Aksyonoff

Usage: indexer [OPTIONS] [indexname1 [indexname2 [...]]]

Options are:
--config <file>		read configuration from specified file
			(default is sphinx.conf)
--all			reindex all configured indexes
--quiet			be quiet, only print errors
--noprogress		do not display progress
			(automatically on if output is not to a tty)
--rotate		send SIGHUP to searchd when indexing is over
			to rotate updated indexes automatically
--buildstops <output.txt> <N>
			build top N stopwords and write them to given file
--buildfreqs		store words frequencies to output.txt
			(used with --buildstops only)
--merge <dst-index> <src-index>
			merge 'src-index' into 'dst-index'
			'dst-index' will receive merge result
			'src-index' will not be modified
--merge-dst-range <attr> <min> <max>
			filter 'dst-index' on merge, keep only those documents
			where 'attr' is between 'min' and 'max' (inclusive)
--merge-killlists			merge src and dst killlists instead of applying src killlist to dst
Examples:
indexer --quiet myidx1	reindex 'myidx1' defined in 'sphinx.conf'
indexer --all		reindex all indexes defined in 'sphinx.conf'</pre>

We are interested in indexing only specific indexes at specific times, following from the example above, the "spare car parts" index, instead of running rake ts:in from cron to re-index everything every hour (for example), you can split those indexes out to separate tasks like this:

<pre>indexer --config /var/www/your-website.com/current/config/production_slave.sphinx.conf spare_car_parts_core --rotate
indexer --config /var/www/your-website.com/current/config/production_slave.sphinx.conf users_core --rotate</pre>

Put those into cron with your desired time schedule, and you will have a happier database server. Important to note is that you must include the _core part, as this is how the ThinkingSphinx gem names the index in the sphinx configuration.

When you deploy remember to run "rake ts:config" with the correct Rails environment to generate your config.
