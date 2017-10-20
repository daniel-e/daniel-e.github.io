Visit my blog at http://daniel-e.github.io/

# Running the blog locally

Do these steps once:

```bash
git clone git@github.com:daniel-e/daniel-e.github.io.git
cd daniel-e.github.io/

apt-get install ruby ruby-dev
sudo gem install bundler
bundle install
```

Do this step every time changes have been made:

```
bundle exec jekyll serve
```

Now visit `http://localhost:4000` with the browser.

# Markup tips

**Include Python code with line numbers**

    {% highlight python linenos %}
    ...
    {% endhighlight %}

**Include links**

    [link text](https://www.example.com/)

**Include math**

Add this to `_includes/head.html`:

    <script type="text/javascript"
        src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
    </script>

Now you can add equations to a post:

    $$ a^2 + b^2 = c^2 $$

**Include images**

    ![alt text](../assets/image.png)

**Grep all used tags**

    grep "tags:" _posts/* | cut -d: -f3 | tr -d '[]' | tr , '\n' | sort -u

