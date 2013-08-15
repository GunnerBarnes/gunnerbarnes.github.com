---
layout: post
title: "Setting up Github Pages blog with custom domain name"
description: ""
category: 
tags: []
---
After setting up a basic layout for this blog, I also wanted to set it up with my own domain. 
Setting up a Github Pages blog with a custom domain couldn't be easier. All you need to do is :
>- Create a CNAME file in your [root directory](https://github.com/GunnerBarnes/gunnerbarnes.github.com).
>- Update your DNS to use an `A` record pointing to `204.232.175.78`

I had purchased this domain a while ago when I first attempted to start blogging on Wordpress, so I had to first update my DNS settings to no longer point to my host's nameservers.

In my case, I bought my domain from [Namecheap.com](http://namecheap.com). However, because I had been using my host's nameservers, the Host Management option didn't appear in the sidebar. After updating my domain to use Namecheap's DNS, the Host Management option appeared right away. 

![Namecheap's Host Management settings](/assets/images/host_management.png)

Save these DNS changes, and that's it, though it may take up to a day for the changes to propagate.





