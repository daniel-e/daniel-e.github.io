---
layout: post
title: Parse HTML with BeautifulSoup in Python
tags: [python, programming]
---

Parsing HTML data is a common task which is also very error prone if you try to write your own parser or if you try to parse HTML  it via regular expressions. Parsing HTML data in Python with the Python package "[Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)" is quite easy and can be done with just a few lines of code.

Look at the example below. It shows you how easy it is with Beautiful Soup to extract all links which match a certain regular expression.

{% highlight python %}
from bs4 import BeautifulSoup
import re

html_page = open("file.html").read()
soup = BeautifulSoup(html_page, "lxml")
for e in soup.findAll('a', attrs = {'href': re.compile("gallery.*dir.*")}):
    print e.get('href')
{% endhighlight %}
