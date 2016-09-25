---
title:  "url linking"
---
Immediately after setting up sane config defaults for [different environments]({% post_url 2016-09-25-environment-based-config %}), I was faced with another mind boggling quirks of Jekyll: **Crazy url linking and relative paths**. I was beginning to believe that the system has a mind of its own and just breaks no matter what changes I do in my `_config.yml`. So the problem was in these two lines:

```
baseurl: "/vivek_jekyll" # the subpath of your site, e.g. /blog
url: "https://vivek1729.github.io/vivek_jekyll" # the base hostname & protocol for your site
```

`url` points to my Github Repo where I have set up this project. And `baseurl` as per the documentation should point to the folder where my blog lives to take care of nesting etc. Sweet. Makes sense till now.

Now in `index.html` where I loop through posts and generate links for them, this goes on :

```
{{ "{{ post.url | prepend: site.baseurl " }}}}
```

And with recent changes of Jekyll which I obviously had no clue about, all theme based stuff has been moved to independent gems. So, all the documentation I read prepared me to expect a folder structure like:

![Folder structure for Jekyll](http://i.imgur.com/QMCGM9O.png)

I did not see any folders like `_includes`,`_layouts` etc, so it took me a while to figure out:

1. Where to find these files which are referenced as just `layout: default` in `index.html`.
2. How to fix the url linking. I could see that the css files which were generated in `head` tag formed by appending the `site.url` and `site.baseurl` params and the posts miraculously enough always had my github base url that is **without** the repo name. So just "https://vivek1729.github.io/"

Diving into the README for [Minima](https://github.com/jekyll/minima), I saw this line comfortably tucked in at the bottom of the page:

>You can choose to override the `_includes/head.html` file to specify a custom style path.

So, I decided to over ride that file, create the necessary folders in my blog source and finally modified my config file to:

```
baseurl: "/vivek_jekyll" # the subpath of your site, e.g. /blog
url: "https://vivek1729.github.io/vivek_jekyll" # the base hostname & protocol for your site
```

and consequently the `_config.dev.yml` file to:

```
baseurl: "" # the subpath of your site, e.g. /blog
url: "http://localhost:4000" # the base hostname & protocol for your site
```

And finally, after all the shenanigans the paths seem to make sense for me. Checkout this [github issue](https://github.com/jekyll/jekyll/issues/332) if you have an hour to spare and just empathize the pain others also went through figuring out similar stuff.

Now that I think about it, I didn't have to modify the `head.html` at all. I could just roll with:

```
baseurl: "/vivek_jekyll" # ensure a slash at the beginning
url: "https://vivek1729.github.io" # yes, no trailing slash
```

So, finally I have reverted everything to normal and just changed my `_config.yml` to ^. Hope this works. (fingers crossed!)