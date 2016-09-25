---
title:  "Environment based configurations"
---
Now that we have a [Todo]({% post_url 2016-09-25-todo %}) List of sorts. Let's start with having *sane* defaults for different environments namely development and production. From [this article](http://www.csinaction.com/2015/02/07/environments-in-jekyll-aka-jekyll_env/), it seems that the environment variable is exposed as **`JEKYLL_ENV`** which can be set to different values in layouts or includes. An example from the same source:

{% highlight html %}
<!DOCTYPE html>
<html>
  <head>
    ...
  </head>
  <body>
    ...
   {{ "{{ content " }}}}
    ...
    {{ "{% if jekyll.environment == 'production'" }}%}
      <p>
        // Do something...
      </p>
    {{ "{% endif " }} %}
  </body>
</html>
{% endhighlight %}

**PS:** I spent more time embedding the snippet above then solving the problem I set out to solve in the first place. The "environment" and "content" variables are reserved [Filters and tags](https://jekyllrb.com/docs/templates/) specified in Liquid templating language. While markdown makes it very easy to write posts and I am enjoying every bit of it, this was a Massive pain in the rear. Those variables actually got compiled and I was seeing entire html content of my posts embedded in the snippet. The solution was to [escape liquid tempalte tags](http://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags) and it is possible without external plugins!

I'll get into layouts and stuff later on. Getting back to setting environments!

This [SO post](http://stackoverflow.com/a/27400343/1083314) came to my rescue. The idea is to 'cascade' different configs when building or serving. I made a `_config.dev.yml` file with `url: "http://localhost:4000" # the base hostname & protocol for your site`.

Now, I serve my blog with the following command:

`bundle exec jekyll serve --config _config.yml,_config.dev.yml`

Notice the order of config files in the command `config1,config2,config3,..` and so on. The parameters will get over-riden as we move to the right which is why I just specify `url` in `_config.dev.yml`.