---
layout: post
title:  'ActiveRecord callback “gotcha”'
date:   2012-05-24 12:00:00
categories: ruby rails activerecord
---

Just now I’ve ran into very interesting behavior with ActiveRecord callbacks. Took me almost an hour to understand what is happening. So this post might save you some time.

Let’s look at the following code example:

    class Something < ActiveRecord::Base
      before_validation :format_url
      ...
      before_validation :grab_url_from_rss, :format_url
      ...

Basically here we’ve gor your regular model with some actions to be performed before validations. But notice that format_url is called two times. This is made intentionally to ensure that resulting URL if formatted, even if it was fetched from the RSS feed of that page. To break things down, here’s what is happening there:

1. URL gets properly formatted to avoid any errors while fetching content of the page
2. We fetch the RSS address from the HTML document of that URL, then we change the URL, to the one that was fetched from the RSS, which usually points to the root of the site
3. We format the new URL one more time, to ensure that the new URL is also properly formatted

Nothing too complicated, really.

But what I didin’t expect is that the first :format_url hasn’t been executing, which sometimes resulted in an error. It appears, that if you put the same method into a callback more than once, only the last occurrence is taken in account. All the previous mentions are ignored.

To solve this I just moved the format_url call to the end of `grab_url_from_rss` method. Simple as that, but took some time to figure the problem.

#### TL;DR

Multiple occurrences of the same method in the same type of callbacks are ignored and only the last one is taken into account. That’s it.
