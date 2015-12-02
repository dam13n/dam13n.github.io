---
layout:     post
title:      "Some thoughts on the Hyperloop concept"
subtitle:   "a wing-based variation"
date:       2015-6-23 12:00:00
author:     "Damien Sutevski"
header-img: "img/autovivification.jpg"
comments:   true
published:  true
---

The recent announcement from SpaceX about hosting a *Hyperloop* competition got me thinking about the concept again. I read Elon Musk's white paper when it was released and some engineering challenges came to mind. I perused through it this week, and here are some things that seem problematic:

* How the heck would a capsule switch tracks (or tubes) such as when at a station? Would a "one-in, one-out" be forced?
* How is banking controlled when navigating through curves or maybe encountering fluctuations in air density in the tube? Sounds like a fun bobsled ride.
* Is it unnecessarily inefficient to only have 1 track per tube?
* How do you stop these things in emergencies?
*

While a default design of cyndrical tubes and a clyndrical-ish capsule made sense from a simplicity and manufacturing cost and easy point of view, I wanted to take a step back and wonder if other geometries could be suitable.

I imagined <a href=""></a>



TL;DR: ```hash = Hash.new { |h, k| h[k] = 0 }```

I've often needed to count or sum over a set of data. Daily sales, weekly user sign ups, and in my most recent project, monthly work units per employee. Here's an example of summing number of orders per day:

{% highlight ruby %}
hash = {}

orders.each do |o|
  hash[o.date] += 1
end
{% endhighlight %}

{% highlight console %}
NoMethodError: undefined method `+' for nil:NilClass
{% endhighlight %}

This sucks because it's such a frequently needed action. I don't want to write in extraneous code to set the expected key values to 0. And I finally found a solution.

{% highlight ruby %}
hash = Hash.new { |h, k| h[k] = 0 }

orders.each do |o|
  hash[o.date] += 1
end
{% endhighlight %}

{% highlight console %}
{"2015-5-14"=>10, "2015-5-15"=>2, "2015-5-16"=>7}
{% endhighlight %}

This is called [autovivification](http://en.wikipedia.org/wiki/Autovivification), which apparently comes from Perl. And it's awesome. Any key's value can have a default value set. Here's another example that comes up a lot for me:

{% highlight ruby %}
hash = {}

orders.each do |o|
  hash[o.date][:state] = o.state
end
{% endhighlight %}

{% highlight console %}
NoMethodError: undefined method `[]' for nil:NilClass
{% endhighlight %}

Similarly, we can set the default value for keys to another hash, which lets us assign values for nested keys immediately.

{% highlight ruby %}
hash = Hash.new { |h, k| h[k] = {} }
{% endhighlight %}
