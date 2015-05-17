---
layout:     post
title:      "Using Comman Separation on the Other Side"
subtitle:   "We should use commas to easily read values smaller than 1."
date:       2007-12-10 16:00:00
author:     "Damien Sutevski"
header-img: "img/numbers.jpg"
published:  true
comments:   true
---

Try reading this:

```
0.000009
```

It has 5 zeroes. Counting these is tedious, especially with even smaller numbers. That is a millionth though. Or a micro- something. Try a number over 1, like:

```
900000.0
```

So that'd be inefficient too. But we fix this with comma separation:

```
90,000
```

Ninety thousand is discerned much faster like this. The separation of the 5 zeroes into a set of 2 and 3 allows this. So why not apply this to any number n, where `1 > |n| > 0 ?` Using my previous example:

```
0.00,000,9
```

This makes it easy to see that the value is a millionth. Note that the first comma separates only the 10th and 100th places. W/o doing this, the value wouldn't reflect the 'larger than 1' corresponding value. I find it faster to use this technique over having to mentally rearrange the last comma each time.

Don't let the asymmetry bother you. The one's place is really the 'center', and the decimal that represents values less than 1 is arbitrary.
