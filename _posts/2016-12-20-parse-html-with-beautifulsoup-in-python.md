---
layout: post
title: Parse HTML with BeautifulSoup in Python
tags:
- python
---

Example

{% highlight python %}
from bs4 import BeautifulSoup
import re

html_page = open("file.html").read()
soup = BeautifulSoup(html_page, "lxml")
for e in soup.findAll('a', attrs = {'href': re.compile("gallery.*dir.*")}):
    print e.get('href')
{% endhighlight %}
