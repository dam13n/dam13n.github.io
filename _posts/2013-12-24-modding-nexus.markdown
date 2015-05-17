---
layout:     post
title:      "modding my Nexus 4 with Cyanogenmod and enabling LTE"
date:       2013-12-24 12:00:00
author:     "Damien Sutevski"
header-img: "img/cyanogen.jpg"
published:  true
comments:   true
---

<h6>Disclaimer: You probably won't do any permanent damage to your phone (but don't take my word for it) if you do anything wrong here, but you can waste a lot of time and get very frustrated. I've modded 4 different androids many times over the last 5 years or so, and I've never bricked a phone.
</h6>
I modded my Nexus 4 ('Mako' is the codename for it) today, switching to Cyanogen 10.2 and enabling LTE.

Speedtest.net results:

{% highlight console %}
LTE:
17.95 Mbps download   19.52 Mbps upload

CDMA:
4.21  Mbps download    2.09  Mbps upload

{% endhighlight %}

I want to get the steps down in writing to help others do the same and future me when I need to reformat my phone. There no single solution to getting this done, so I'll describe the process I took. I did this using Windows 7, but most of the steps should be similar regardless of the OS.

1) Tools that are included in the latest [Android SDK (adt bundle)](http://developer.android.com/sdk/index.html), called adb and fastboot, are necessary to access the phone. Unzip wherever.

2) If you haven't enabled Developer Mode on your phone, you'll have to in order to enable USB debugging. Go to Settings and click "Build number" 7 times. You'll see a visual confirmation. Now enable USB debugging.

3) Even getting your computer to recognize your Nexus 4 can be a pain sometimes. If you have issues with Windows, uninstall the current drivers through the device manager, and install this driver. If that link breaks, [here is the forum post](http://forum.xda-developers.com/showthread.php?t=1992345).


Make sure to add the whatever Cyanogenmod (we'll call it CM hereon), Google apps, radio zips to your phone's `/sdcard/` directory (whether a real sdcard or on the internal memory). Plug your phone by USB into your computer and either drag and drop files or use the adb "push" commands (in the 'platform-tools' directory of the Android SDK you installed). Sample command:

{% highlight console %}
adb push cm-10.2.zip /sdcard/
{% endhighlight %}

4a) If you want CM with LTE enabled, [go here](http://d-h.st/users/negroplasty/?fld_id=22218#files) (10.2 is latest stable version as of this post).

And [get a radio](http://forum.xda-developers.com/showthread.php?t=2267890).

Mmultiple choices here under "Resources", but I used the .33-.84 Hybrid radio, and it's working fine so far. Others have reported the other radios working too.

4b) OR for the standard CM, [get it here](http://download.cyanogenmod.org/?device=mako&type=).

And [get a radio in case Wifi/Bluetooth](http://forum.xda-developers.com/showpost.php?p=44719035&postcount=3621) don't work (mine didn't by default).

5) Get [Google apps](http://wiki.cyanogenmod.org/w/Google_Apps) (make sure to match to appropriate CM version):


6) Also get the [ClockwordMod Recovery image](http://download2.clockworkmod.com/recoveries/recovery-clockwork-6.0.4.3-mako.img).


I made sure to take whatever I wanted to off my phone before doing this, so make sure you back up whatever you want. I have no recommendations for how to do this, but there are tools out there.

7) Now with your phone connected by USB to your computer, run this command from the terminal:

{% highlight console %}
adb reboot bootloader
{% endhighlight %}

8) This will boot the phone into the bootloader screen. FYI, in general, volume up and down control direction and the power button selects.
Now in your terminal, you'll want to unlock the phone (visually confirmed). I believe this step wipes the phone:

{% highlight console %}
fastboot oem unlock
{% endhighlight %}

8) Restart the phone if it didn't already. You may need to re-enable USB debugging.
Now get to the bootloader screen again. Make sure the ClockwordMod Recovery image is in the same directory as the adb and fastboot (makes it easier) and run these two commands:

{% highlight console %}
fastboot flash recovery recovery-clockwork-6.0.4.3-mako.img

fastboot boot recovery-clockwork-6.0.4.3-mako.img
{% endhighlight %}


9) Now you should be in the CWM Recovery screen.
Choose to factory wipe / reset your phone.
Now choose to install a zip and navigate to the sdcard and to the `/0/` directory or wherever you put your zips.

10) Install the CM zip first. Then the Google apps zip. Then the radio zip.

Now restart your phone! If you chose the LTE enabled CM, there are just a few more steps - otherwise, you're done!

11) Don't worry if your signal bars look different now or have a weaker signal. Go to the phone dialer and enter:

{% highlight console %}
*#*#4636#*#*
{% endhighlight %}

Go to Phone Information, and select "LTE/GSM/CDMA auto" from the drop-down menu.

12) Lastly, navigate to Settings > More > Mobile Networks > Access Point Names, and choose this network configuration:

{% highlight console %}
APN   fast.t-mobile.com
MMSC  http://mms.msg.eng.t-mobile.com/mms/wapenc
MCC   3109
MNC   260
APN   type default, supl, mms
APN   protocol IPv4/IPv6
{% endhighlight %}

You can create a new one or modify the IPv6 version to include the IPv4/IPv6 option. Remember to hit save.

You should be good to go!

I JUST did this process myself, so I have no feedback about bugs or if this helped or hurt my Nexus 4 experience. Will try to remember to update. Let me know about your experiences! : )









