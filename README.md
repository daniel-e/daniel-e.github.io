Visit my blog at http://daniel-e.github.io/

# Running the blog locally

```bash
git clone git@github.com:daniel-e/daniel-e.github.io.git
cd daniel-e.github.io/
apt-get install ruby ruby-dev
sudo gem install bundler
sudo bundle install
bundle exec jekyll serve
```

Now visit `http://localhost:4000` with the browser.

# Markup tips

Include Python code with line numbers:

    {% highlight python linenos %}
    ...
    {% endhighlight %}

Include links:

    [link text](https://www.example.com/)


