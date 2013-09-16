---
layout: post
title: "Jekyll Auto Reload"
description: ""
category: 
tags: [jekyll]
---

As I mentioned in [my first ever post]({% post_url 2013-08-14-hello-world%}) on this site, I'm blogging with Jekyll hosted on Github Pages. 
For my development site, I can serve Jekyll locally by running the command, `jekyll serve`, from the command line 
in the root directory of my site. The `jekyll serve` command can also take optional parameters. You can see these
optional parameters by appending `-h`, for "help" to the `jekyll serve` command. The `-w` parameter was of particular
interest to me, as it tells your jekyll server to watch for changes and automatically regenerate your local site. Without this option,
you would have to stop and restart the server anytime you make a change.