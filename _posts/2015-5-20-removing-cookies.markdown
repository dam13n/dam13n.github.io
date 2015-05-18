---
layout:     post
title:      "Removing cookies when building APIs more mobile apps"
date:       2015-5-17 12:00:00
author:     "Damien Sutevski"
header-img: "img/numbers.jpg"
published:  false
comments:   true
---

I've run into sign in and sign out issues on iOS apps when using session cookies. This solved it for me. I placed it in an ApiController class that the rest of my API inherited from.

{% highlight ruby %}
def remove_cookies
  request.session_options[:skip] = true
end
{% endhighlight %}

It's strange to me that this was needed and fixed the issue... If there's something else at play that I'm missing, let me know!
