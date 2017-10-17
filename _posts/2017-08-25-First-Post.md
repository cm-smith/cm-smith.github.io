---
layout: single
author_profile: true
categories: [Coding]
tags: [Jekyll]
---

# Understanding the Minimal Mistakes Theme

These posts will be hidden later, but for now used as a board to remember how
to personalize the site.

Use the [quick-setup
guide](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
for further documentation.

## Adding Links to the Header

The header is composed of two items:

1. A link to the main page
	- To adjust, go to `_config.yml` file in root directory and change
	  'title' attribute
2. Links that can become hidden by a dropdown menu
	- To adjust these, go to the `_data/navigation.yml` file

## Creating New Directories

Instead of creating an 'About' directory (in which you have a `index.html`
file), Michael Rose recommends just using a `_pages` directory that contains
pages such as `404.md`, `about.md`, `contact.md`, etc.

General coding practices to integrate these new directories:
- Include the new directory in the `_config.yml` file in the 'include:' section
- Assign permalink overrides in the YAML Front Matter for each new page
```md
---
layout: single
permalink: /about/
---
```

## YAML Front Matters

Use the `_pages/about.md` file as an example of overlaying an image at the top
of a post.

```md
---
layout: single
title: ""
excerpt: "Improves SEO and archive listings."
author_profile: true
tags: [stuff, something else]
categories: [Machine Learning, Code]
---
```

## Embedding Content

Let's see how to embed a video from youtube. Consider you want to use the
following link: "https://www.youtube.com/watch?v=XsxDH4HcOWA"

{% include video id="XsxDH4HcOWA" provider="youtube" %}

To add an image with a caption:

{% include figure image_path="/assets/images/bio-pic.jpg" alt="scroll-over
text" caption="This adds a caption with *markdown* allowed." %}

Tuesday Feb 16, 2016: Project Launch
- Create stuff
- other stuff

Friday Feb 19, 2016: Build Milestone
- stuff
- and more stuff

