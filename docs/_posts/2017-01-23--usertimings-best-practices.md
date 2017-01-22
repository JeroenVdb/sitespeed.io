---
layout: default
title: User timers Best Practices and New Whitelisting
description: User timings can now be filtered using a user defined whitelist
authorimage: /img/robot-head.png
intro: Recently Google decided it would be a good idea to make use of User Timings to track some of their ads implemented on various sites. While generally I would agree with that sometimes that can also have various reprocussions for doing it in a way that could cause issues for other tools. 
keywords: sitespeed.io, speed, index, speed, webperf, performance, web, wpo
nav: blog
---

#User timers Best Practices and New Whitelisting

Recently Google decided it would be a good idea to make use of User Timings to track some of their ads implemented on various sites. While generally I would agree with that sometimes that can also have various reprocussions for doing it in a way that could cause issues for other tools. 

So what's the big deal? Well they decided to implement their user timings to use a unique id per ad which in combination with a site capturing those timings can cause serious issues with storage in performance tracking tools that use the timings of a page for trending analysis. The biggest impact we have seen is the integration with a TSDB (time series database). If every user timer has a unique id it means you nolonger have control of the size of your TSDB and it is now at the mercy of these user timings and will grow unbounded ... indefinetly. That's no good! :'( 

A few weeks ago after we noticed these rogue usertimings filling our poor servers we decided to blacklist google's user timings until we could implement a more robust solution. Luckly one best practice that google followed was namespacing their user timings with `goog_`. That allows us to easily remove these timings since they are not really relevant for our performance tracking. 

With the latest release we probably will continue to permenately blacklist `goog_`, but with that we will have a whitelist in which a user will be able to supply a regex that will restrict what user timings are reported on and sent to our currently supported TSDB of graphite and InfluxDB. You should be able to do this by passing `--browsertime.userTimingWhitelist <some regex>`


With that all said and the whitelist implemented I think the following are generally best practice for use of the User Timings that are supported in modern browsers

- namespace your timers with `<site>_`
- Don't use unique ids unless absolutely necessary
- If you do need to use unique ids make sure to use namespaces ;-)

With all this said Happy New year everyone!

/Peter, Tobias, and Jonathan