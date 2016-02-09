---
layout: post
title:  "I'll try jekyll for now"
date:   2015-08-14
tags:
- jekyll 
---

First official post of the blog. Not sure how effective jekyll is compared to wordpress but I'll try it for now.
The main problems I've encountered with it so far are as follows...

* Couldn't even get it to work with github pages at first due to what i'm guessing was intalling the latest version of jekyll and the version mismatch caused build errors

* Trying to find a solution to being able to easily create new posts without having to manually enter in the date and title with the "YYYY-MM-DD-post-title" template for posts which is pretty annoying. As far as I can tell i'm supposed to use a rake test/script for this since custom plugins for jekyll just aren't compatible with github-pages due to security reasons.

For now I'll just try and use a rake script for creating new files since it seems to be the easiest solution. From there I just need to paginate the blog posts, create a post directory, customize the site layout, and learn markdown and I should be fine.
