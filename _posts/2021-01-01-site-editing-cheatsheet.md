---
layout: post
title: "Site Editing Cheatsheet"
date: 2021-01-01 16:00:00 -0600
categories: site
---

To reduce the complexity in this Github Pages site, the default theme [`minima`](https://github.com/jekyll/minima) is used. Not all files from the theme are copied and saved in this repo. Instead, we have added `gem "minima"` to the `Gemfile` for this Jekyll site. Whenever we serve the site (e.g., `jekyll serve`), the theme templates are imported upon build. We can copy and edit any files from their Github page to personalize the `minima` theme.

# Table of contents

- [Header](#header)
- [Footer](#footer)
- [Main content](#main-content)

## Header

By default, any markdown files in the main directory will appear in the header. To specify only a subset of files, as well as the order, you can edit the `_config.yml` file:

```
# _config.yml
# If you want to link only specific pages in your header, uncomment
# this and add the path to the pages in order as they should show up
#header_pages:
# - about.md
```

## Footer

## Main content

- [Link to pages](https://jekyllrb.com/docs/liquid/tags/#link): {% raw %}`{% link dir/page.html %}`{% endraw %}
- [Link to posts](https://jekyllrb.com/docs/liquid/tags/#linking-to-posts): {% raw %}`{% post_url yyyy-MM-dd-post-name %}`{% endraw %}
