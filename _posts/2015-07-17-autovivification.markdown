---
layout:     post
title:      "'initializing' hashes in ruby - autovivification"
subtitle:   "Counting, sums, and nested hashes right off the bat"
date:       2015-5-17 12:00:00
author:     "Damien Sutevski"
header-img: "img/autovivification.jpg"
comments:   true
published:  true
---

TL;DR: ```hash = Hash.new { |h, k| h[k] = 0 }```

I've often needed to count or sum over a set of data. Daily sales, weekly user sign ups, and in my most recent project, montly work units per employee. Here's an example of summing number of orders per day:

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
  hash[o.date][o.state] += 1
end
{% endhighlight %}

{% highlight console %}
NoMethodError: undefined method `[]' for nil:NilClass
{% endhighlight %}

Similarly, we can set the default value for keys to another hash, which lets us assign values for nested keys immediately.

{% highlight ruby %}
hash = Hash.new { |h, k| h[k] = {} }
{% endhighlight %}
