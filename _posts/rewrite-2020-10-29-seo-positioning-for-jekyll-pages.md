---
layout: post
title:  "SEO positioning for jekyll pages"
date:   2020-10-29 00:00:00 +0200
categories: jekyll update
---
If you want to make your Jekyll webpage easier to index for search engines and social networks, what you have to do is **improve the SEO of your webpage**.

If you have built your website using Jekyll, you can follow these steps:

## 1 - Add the SEO plugin
Add the following to your site's `Gemfile`:

```ruby
gem 'jekyll-seo-tag'
```

## 2 - Update the config file
Add the following to your site's `_config.yml`:

```yml
plugins:
- jekyll-seo-tag
```

If you are using a Jekyll version less than `3.5.0`, use the `gems` key instead of `plugins`.

## 3 - Test if it worked

You'll have to wait some time to see the changes when you make a search, as search engines will have to update their database.

Now search engines will index your Jekyll webpage much easier, and as consequence, it will appear higher in the list when people search for it.

In my case, **this is the result**:

---

![SEO](/images/seo-duckduckgo.png "SEO")

---