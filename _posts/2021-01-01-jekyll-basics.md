---
layout: post
title: "Jekyll Basics"
date: 2021-01-01 22:00:13 -0600
categories: jekyll
---
[*Refer to online documentation here.*](https://jekyllrb.com/)

Jekyll converts markup languages [<a href="#ref1">1</a>], such as HTML,
Markdown, or LaTex, into static websites. [*Static* websites](https://en.wikipedia.org/wiki/Static_web_page) are
displayed exactly as specified in source code (e.g., HTML), whereas
*dynamic* websites are generated when a user loads the page (e.g., PHP
script). Jekyll is a Ruby project and uses
[RubyGems](https://rubygems.org/) as the package manager for any
external plugins (e.g., `gem install ...`) [<a href="#ref2">2</a>]. A
common, and Jekyll-required, plugin is [`bundler`](https://bundler.io/),
which maintains dependencies.

## Command Line

The following workflow is commonly used:

```bash
# Check dependencies (GCC/Make not listed
ruby -v                     # Version of Ruby; should be greater than 2.4
which gem                   # Check for RubyGem as the package manager

# Initiate `jekyll` and `bundle` CLI commands
gem install jekyll bundler

# Create new Jekyll project
# Jekyll defaults a prebuilt theme (`minima`); to ignore this, you can pass `--blank`
jekyll new another-site-of-mine
cd another-site-of-mine/

# Create a new "Gemfile" to maintain project dependencies
bundle init
vim Gemfile                 # Append `gem "jekyll"` as a dependency

# Build the site and make available on local server
bundle exec jekyll serve    # http://localhost:4000
```

Running `bundle exec` ensures dependencies are set and versioned
correctly. Running `jekyll serve` builds the site and outputs the static
site into the `_site` directory (equivalent to `jekyll build`), as well
as rebuilding it any times changes are made with the site viewable on
the local server (http://localhost:4000) [<a href="#ref3">3</a>].

## Directory Layout

```
.
|-- _config.yml
|-- _data
|   |__ members.yml
|-- _includes
|   |-- footer.html
|   |__ header.html
|-- _layouts
|   |-- default.html
|   |__ post.html
|-- _posts
|   |__ YYYY-MM-DD-blog-post.md
|-- _sass
|   |-- _base.scss
|   |__ _layout.scss
|-- _site
|-- .jekyll-metadata
|__ index.html
```

Overview of the documents and their respective functions:

| File/Directory | Description |
| -------------- | ----------- |
| _config.yml    | Stores configuration data (rather than operating with parameters in command line) |
| _drafts        | Unpublished posts (formatted without a date) |
| _includes      | Partials that can be facilitated for reuse. Given liquid tag "include file.ext", the partial can be found in '_includes/file.ext' |
| _layouts       | Templates wrapping posts. Chosen on a post-by-post basis in YAML Front Matter. The liquid tag "content" injects content into the webpage |
| _posts         | Dynamic content. Naming convention is important, formatted: `YEAR-MONTH-DAY-title.MARKUP`. Permalinks can be customized for each post, but date and markup language are solely determined by file name. |
| _data          | Well-formatted data (e.g., .yml, .yaml, .json, .csv) and accessible via `site.data`. Then, if there's file '_data/members.yml', we can access the contents through `site.data.members`. |
| _sass          | Sass partials available to be imported into 'main.scss', which will be processed into single stylesheet 'main.css'. |
| _site          | The generated site will be placed here by default once Jekyll transforms all the text (typically add this to `.gitignore`). |
| .jekyll-metadata | Helps Jekyll keep track of files that have not been modified since last build (again, a `.gitignore` file). |
| index.html or index.md | Provided the file has a YAML Front Matter section, it will be transformed by Jekyll. |
| Other Files/Folders | Every other directory/file (such as css and images folders) will be copied verbatim to the generated site. |

## Configuration Notes

- YAML stands for "YAML Ain't Markup Language" with its primary purpose being "a human friendly data [serialization](https://en.wikipedia.org/wiki/Serialization) standard" [<a href="#ref4">4</a>].
- YAML supports Jekyll's configuration (`_config.yml`) and Front Matter content.
- [Front Matter](https://jekyllrb.com/docs/front-matter/) specifies common inputs for a post, such as layout, title, author, date, permalink, category, and tags.
- [Liquid](https://shopify.github.io/liquid/) is supported by Jekyll and can be used to fetch YAML variables. Liquid must be within HTML, which may require embedding HTML tags in markup files.

Below is an example for setting default configurations for the Front
Matter in the site.

```yml
# _config.yml
defaults:
  -
    scope:
      path: ""
      type: "pages"
    values:
      layout: "my-site"
      track_this_variable: "dog"
  -
    scope:
      path: "blog" # For files in the /blog folder
      type: "pages"
    values:
      layout: "blog-post" # Different layout for these posts
```

Here is an application of the Front Matter [<a href="#ref5">5</a>]:

{% raw %}
```md
---
layout: post
title: This is My Title
date: YYYY-MM-DD HH:MM:SS
new_variable: hotdog
---

The area above is **Front Matter**. The variables in this section can be referenced with **Liquid** as:

My favorite food is <p>{{ page.new_variable }}<p>.

Additionally, we can reference variables set in `_config.yml`:

My favorite animal is <p>{{ site.track_this_variable }}<p>.
```
{% endraw %}

## References

<p id="ref1">[1] <a href="https://en.wikipedia.org/wiki/Markup_language">According to Wikipedia</a>, <em>"a markup language is a system for annotating a document in a way that is syntactically distinguishable from the text, meaning when the document is processed for display, the markup language is not shown, and is only used to format the text."</em></p>

<p id="ref2">[2] Jekyll has a <a href="https://jekyllrb.com/docs/ruby-101/">Ruby 101</a> resource in their documentation, but main takeaways are trapped above.</p>

<p id="ref3">[3] <em>"For any Jekyll site, a build session consists of discrete phases in the following order - setting up plugins, reading source files, running generators, rendering templates, and finally writing files to disk."</em> - <a href="https://jekyllrb.com/docs/rendering-process/">Jekyll Rendering Process</a></p>

<p id="ref4">[4] Documentation for the lastest release of YAML is available <a href="https://yaml.org/spec/1.2/spec.html#Introduction">here</a>. Another big takeaway from this documentation is, <em>"YAML was specifically created to work well for common use cases such as: configuration files, log files, interprocess messaging, cross-language data sharing, object persistence, and debugging of complex data structures."</em></p>

<p id="ref5">[5] Note to show raw code for Liquid script, you must wrap the code with `(% raw %) ... (% endraw %)`. Silly enough, we need to use parantheses in place of curly brackets here, since the `endraw` in my note would prematurely break the `raw ... endraw` wrapping this paragraph.</p>
