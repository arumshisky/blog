---
layout: single
title:  "Welcome to Jekyll!"
date:   2019-06-19 18:41:47 -0400
categories: jekyll update
collections: test
tags: tag 
mathjax: true
toc: true
toc_label: "My Table of Contents"
toc_icon: "cog"
excerpt: "A unique line of text to describe this post that will display in an archive listing and meta description with SEO benefits."
classes: wide
---

# Subtitle

Just testing everything including $$x$$ math

* list
* list
* list

$$ f(a) = \frac{1}{2\pi i} \oint_\gamma \frac{f(z)}{z-a} dz $$

## Just testing

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

# Testing kramdown

# Welcome!

This is an online [Markdown](http://en.wikipedia.org/wiki/Markdown)
editor. Whatever Markdown text you write here gets transformed into
HTML that gets displayed on the right.

# My Funky Heading 1
{: #funky }

The funky text is [described below](#funky)

If you enable this in _config.yml:

~~~
kramdown:
    auto_ids: true
~~~

Then this will also work:

### My Funky Heading

The funky text is [described below](#my-funky-heading)

Hlines
------

* * *

---

  _  _  _  _

---------------

This is some text.[^1]. Other text.[^footnote].

[^1]: Some *crazy* footnote definition.

[^footnote]:
        this is now a code block (8 spaces indentation)

This is a Ruby code fragment `x = Class.new`{:.language-python}

~~~
def func():
    pass
~~~
{: .language-python}

Definition lists
----------------

kramdown
: A Markdown-superset converter

{::comment}
This text is completely ignored by kramdown - a comment in the text.
{:/comment}

Do you see {::comment}this text{:/comment}?
{::comment}some other comment{:/}

|---
| Default aligned | Left aligned | Center aligned | Right aligned
|-|:-|:-:|-:
| First body part | Second cell | Third cell | fourth cell
| Second line |foo | **strong** | baz
| Third line |quux | baz | bar
|---
| Second body
| 2 line
|===
| Footer row

|-----------------+------------+-----------------+----------------|
| Default aligned |Left aligned| Center aligned  | Right aligned  |
|-----------------|:-----------|:---------------:|---------------:|
| First body part |Second cell | Third cell      | fourth cell    |
| Second line     |foo         | **strong**      | baz            |
| Third line      |quux        | baz             | bar            |
|-----------------+------------+-----------------+----------------|
| Second body     |            |                 |                |
| 2 line          |            |                 |                |
|=================+============+=================+================|
| Footer row      |            |                 |                |
|-----------------+------------+-----------------+----------------|


Table [#tab-sample] shows an example table figure.

~ TableFigure { #tab-sample; \
    caption: "Modelle mit unterschiedlich geschätztem baseline hazard"; }
|                 | $c(t)$                                         ||||
|                 |---------|---------|---------|---------------------|
|                 | (A0)    | (A1)    | (A2)    | (A3)                |
|                 | ohne    | Log     | Polynom | Stückweise konstant |
|:----------------|:-------:|:-------:|:-------:|:-------------------:|
| Log likelihood  | -6.798  | -6.733  | -6.715  | -6.686              |
| Pseudeo $R^{2}$ | 0,048   | 0,057   | 0,059   | -                   |
| AIC             | 13.615  | 13.489  | 13.456  | 13.483              |
| BIC             | 13.711  | 13.594  | 13.580  | 14.009              |
| N               | 105.484 | 105.484 | 105.484 | 105.484             |
|-----------------|---------|---------|---------|---------------------|
{ .booktable }
~
Table 1 shows an example table figure.
