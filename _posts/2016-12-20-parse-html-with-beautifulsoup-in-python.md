---
layout: post
title: Parse HTML with BeautifulSoup in Python
---

{% highlight python %}
#!/usr/bin/env python

from bs4 import BeautifulSoup
import re, urllib

html_page = open("gallery.html").read()
soup = BeautifulSoup(html_page, "lxml")
for link in soup.findAll('a', attrs = {'href': re.compile("gallery.*dir.*")}):
    print link.get('href')
{% endhighlight %}
