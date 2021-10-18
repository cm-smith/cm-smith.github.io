---
layout: post
title: "Site Editing Cheatsheet"
date: 2021-01-01 16:00:00 -0600
categories: site
---

*This post is continuously updated while learning on the fly.*

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

- CDNs used to fetch centrally located javascript files
    - More info about "Content Delivery Networks" can be found [here](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/).
    - **Major limitation** is not being able to develop offline without broken functionality.
- Post-specific styling or scripting
    - You can add `<style>` and `<script>` components to the top of the markdown file to embed CSS and Javascript functionality.
- Linking to other pages in the site
    - [Link to pages](https://jekyllrb.com/docs/liquid/tags/#link): {% raw %}`{% link dir/page.html %}`{% endraw %}
    - [Link to posts](https://jekyllrb.com/docs/liquid/tags/#linking-to-posts): {% raw %}`{% post_url yyyy-MM-dd-post-name %}`{% endraw %}
- Creating drop-down components
    - You can use the [`<details>` HTML tag](https://gist.github.com/citrusui/07978f14b11adada364ff901e27c7f61).
    - Be sure to keep an empty line between the summary tag and the main content; adding new lines (`<br>`) is helpful in formatting
    - If you want to enable Markdown syntax in the main content, you can specify so in the tag: `<details markdown="1">`.
- Math syntax
    - Use MathJax CDN, which maps pretty closely to LaTeX. To append the CDN to a specific post (and account for in-line LaTeX syntax), here is available [code from StackOverflow](https://stackoverflow.com/a/46765337).
