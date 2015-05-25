---
layout:     post
title:      "Sending images with your Ruby / Rails API - Receive them in RubyMotion"
subtitle:   "Base64 encoding to translating images to binaries and back."
date:       2015-05-24 12:00:00
author:     "Damien Sutevski"
comments:   true
header-img: "img/post-bg-04.jpg"
published:  true
---

<h2 class="section-heading">Rails Side</h2>

For an iOS game I'll eventually release, I store images for each "story" on a Story model. `image_file` is one of the attributes and it's just a string of the image's file name. There's a before_save callback that stores a Base64 binary version of the image on that model's instance.

{% highlight ruby %}
before_save :generate_image_binaries

...

file_path = "#{Rails.root}/app/assets/images/#{image_file}"
file = File.read(file_path)
self.image = Base64.encode64(file).gsub(/\n/, "")
{% endhighlight %}

<h2 class="section-heading">RubyMotion Side</h2>

After receiving a "Story" object in RubyMotion through an API call, I initialize an NSData object with the Base64 encoded image. These are stored with NSDefaults. I'm using a globally variable here. Blah blah not usually recommended blah blah blah.

{% highlight ruby %}

$defaults["image-0"] = NSData.alloc.initWithBase64EncodedString(story[:image], options: 1)

{% endhighlight %}

This app uses an [RubyMotion Query (RMQ)](http://rubymotionquery.com/). This is an awesome library. Recently, RMQ and [ProMotion](https://github.com/clearsightstudio/ProMotion)(and more) combined as [RedPotion](https://github.com/infinitered/redpotion). Haven't tried that yet, but I'm excited to. Anyway, later when showing Story views, I set the image in RMQ. It shouldn't be too hard to adapt the code below if you aren't using RMQ. The part that matters is `UIImage.imageWithData`.

{% highlight ruby %}
def story_image(st)
  st.image = UIImage.imageWithData($defaults['image-0'])
  ratio = st.image.size.width / st.image.size.height
  st.view.contentMode = UIViewContentModeScaleAspectFit
  st.frame = {t: 0, l: 0, w: app_width, h: app_width/ratio}
  st.background_color = UIColor.blackColor
end

{% endhighlight %}