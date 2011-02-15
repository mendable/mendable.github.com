--- 
layout: post
title: Sometimes the best way to create is to remove
wordpress_id: 227
wordpress_url: http://www.mendable.com/?p=227
---
Over the life cycle of a project many features may be developed, sometimes just to account for one variation or tiny use case, get used once, and are then not used again for the next 5 years (if ever).

One of my personal pet peeves is overly complicated software with too many redundant, old, unused features. This sort of redundant complexity increases the learning curve for new developers coming into the software project and creates extra work in maintaining the software, 

I often think the best way to create and maintain software, is to periodically remove all the old crap not being used any more. Some people do not like the idea of losing a feature in their application, as in "OMG, you can't take that away, I was planning to use that one day", and maybe they were. But often, you have to look at what they say they are going to do, and compare that with what they actually do. 

I am sure this is some sort of psychological effect where the fear of losing a feature has more of an emotional impact on the end user / client than the prospect of faster software development and the addition of new features, as well as cheaper maintenance. 

If the feature works, can't it just stay?

Hypothetically yes, but sometimes the part the end user sees is that the old features only continue to work because when adding new features we are able to test we have not broken the old features, and in some cases that means a ton of work when a big structural change is occurring.

Also consider the development of a new software project, or a big addition to an existing project. The number of proposed features grows exponentially with the number of people involved. In reality, only 20% of the features will make 80% of the software, but you may end up coding 80% of the features to make up 90% of the functionality, just because people don't get agile, or the business structure around you does not allow for it to happen so easily (extensive budgeting approvals, waterfall methodology), or of the HiPPO effect (Highest paid person's opinion).

This post has been a bit of a rant, but the points are these:
* Don't develop crap you don't need
* Remove old crap you don't use any more

Do points 1 & 2 and you will get:
* Faster software development
* Software that is easier to maintain
* Easier to bring in new developers to a project
* Happier developers
* Reduced costs all round
