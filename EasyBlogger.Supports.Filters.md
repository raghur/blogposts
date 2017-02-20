<!--
PostId: 9183468336173429775
Title    : EasyBlogger now supports pandoc filters
Labels   : easyblogger
Format	 : markdown
Published: true
filters: c:/users/raghuramanr/AppData/Roaming/npm/mermaid-filter.cmd
-->
I just pushed version 1.1.0 of [EasyBlogger](http://github.com/raghur/easyblogger) to pypi. Notable changes are that
it now exposes ability to set [pandoc filters](https://github.com/jgm/pandoc/wiki/Pandoc-Filters).

Pandoc filters basically let you write plugins that can affect the document conversion. For example, my filter -
[`mermaid-filter`](http://github.com/raghur/mermaid-filter) will let you embed diagrams in text using
[mermaid](http://github.com/knsv/mermaid) syntax and then when you convert a markdown document, generate a .png file on
the fly, upload it to imgur and embed the link in the generated HTML file - so that your final HTML shows the image and not
a fenced code block!


A quick example - with markdown like this

```markdown
~~~mermaid
graph TD
    A --> B
~~~
```

note that you must specify that it's a mermaid syntax with ` ~~~mermaid ` as the start of your code block

You get an image like this in your blog:

~~~mermaid
graph LR
    A --> B
~~~

In fact, even this blog post was generated from mermaid source itself :)

Goes without saying that this isn't limited to mermaid-filter - you can use any of the other pandoc filters that are
available to generate/filter your blog markup!

Hope you like it as much as I do!

