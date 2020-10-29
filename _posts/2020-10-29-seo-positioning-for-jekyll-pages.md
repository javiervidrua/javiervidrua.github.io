---
layout: post
title:  "SEO positioning for jekyll pages"
date:   2020-10-29 00:00:00 +0200
categories: jekyll update
---
If you want to make your Jekyll webpage easier to index for search engines and social networks, follow these steps:

## Step 1
Add the following to your site's `Gemfile`:

```ruby
gem 'jekyll-seo-tag'
```

## Step 2
Add the following to your site's `_config.yml`:

```yml
plugins:
- jekyll-seo-tag
```

If you are using a Jekyll version less than `3.5.0`, use the `gems` key instead of `plugins`.

And with that, search engines will index your Jekyll webpage much easier, and as consequence, it will appear higher in the list when people search for it.