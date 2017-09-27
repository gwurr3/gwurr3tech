# gwurr3tech
hugo theme for tech-related blog/site.
<br>
it's a work in progress...
<br>
<br>

The basic file skeleton, templates, misc features, and a good chunk of this README.md file  were shamelessly stolen from [After Dark](https://github.com/comfusion/after-dark)
<br>
However, no CSS or anything visual was.
<br>


example `config.toml` entries:
```
baseurl = "https://chamge.me.please/" # Controls base URL
languageCode = "en-US" # Controls site language
title = "Example Blog" # Homepage title and page title suffix
paginate = 20 # Number of posts to show before paginating

theme = "gwurr3tech" # Set default theme

enableRobotsTXT = true # Suggested, enable robots.txt file
googleAnalytics = "" # Optional, add tracking Id for analytics
disqusShortname = "" # Optional, add Disqus shortname for comments
SectionPagesMenu = "main" # Enable menu system for lazy bloggers
footnoteReturnLinkContents = "↩" # Provides a nicer footnote return link

[params]
	description = "" # Suggested, controls default description meta
	author = "" # Optional, controls author name display on posts
	hide_author = false # Optional, set true to hide author name on posts
	show_menu = true # Optional, set true to enable section menu
	powered_by = true # Optional, set false to disable credits
	images = [
		"https://source.unsplash.com/category/technology/2000x1322"
	] # Suggested, controls default Open Graph images
	theme_variant = "" # Optional, for use to overriding default theme
```
<br>
creating first post: `hugo new post/starry-night.md`


## Customizing

### Section Menu

gwurr3tech uses Hugo's [Section Menu for Lazy Bloggers](https://gohugo.io/extras/menus/#section-menu-for-the-lazy-blogger) to produce global site navigation if enabled.

To customize the menu, update the settings in `config.toml` like:

```toml
[[menu.main]]
  name = "Home"
  weight = 1
  identifier = "home"
  url = "/"
[[menu.main]]
  name = "Posts"
  weight = 2
  identifier = "post"
  url = "/post/"
```

Or update the menu using front matter from your pages:

```toml
menu = "main"
weight = 3
```

### Intelligent Lazy Loading

Lazy loading prioritizes when and how images and more are downloaded, improving perceived performance and reducing page load times. When activated, lazy loading will start working automatically. No JavaScript configuration is necessary.

**What makes it _Intelligent_?** If no lazy loaded content is detected on a page when the site is generated, the feature will not be activated and no additional downloads will occur.

To activate lazy loading with [lazysizes], add `lazyload` to the `class` attribute of your images/iframes in conjunction with a `data-src` and/or `data-srcset` attribute:

```html
<!-- non-responsive -->
<img data-src="image.jpg" class="lazyload">
```

```html
<!-- responsive with automatic sizes calculation -->
<img
  data-sizes="auto"
  data-src="image2.jpg"
  data-srcset="image1.jpg 300w, image2.jpg 600w, image3.jpg 900w"
  class="lazyload">
```

```html
<!-- iframe example -->
<iframe frameborder="0"
  class="lazyload"
  allowfullscreen
  data-src="//www.youtube.com/embed/ZfV-aYdU4uE">
</iframe>
```

To help get you started, gwurr3tech includes a _Shortcode_ taking advantage of this feature, enabling you to easily create [lazy-loaded `figure` elements](#content-reuse) within your markdown content.

Additional information and examples, including how to set-up and use LQIP (Low-Quality Image Placeholders), are available on the [lazysizes] repository on GitHub.


### BPG Image Support

The BPG image format provides [high-fidelity images](http://xooyoozoo.github.io/yolo-octo-bugfixes/#vintage-car&jpg=s&bpg=s) which look more like PNGs but loads as fast as a JPG. From a compression standpoint, BPG really shines when handling animations. With support for alpha transparency and given its compression, BPG [literally steamrolls](https://bellard.org/bpg/animation.html) the GIF format of yesteryear.

**Why haven't I heard of BPG?** You have now, and you'll learn about all kinds of cool stuff like this by keeping your eye on [Perf.Rocks](http://perf.rocks/). Please help push BPG forward by encouraging browser makers to improve [current support levels](http://caniuse.com/#search=bpg).

Use BPG just like any other image with the `img` element with a `.bpg` image file extension on any [encoded image](https://webencoder.libbpg.org/). gwurr3tech will asynchronously download a BPG polyfill and render the image in a `canvas` element.

BPG image support is enabled by default in gwurr3tech. To disable support for BPG images add the following to your site configuration:

```toml
[params.seo]
  disable_bpg = true # Disable BPG image support
```

### Related Content

Promote more of your content to your site visitors. By offering your readers more content that's relevant to them you can increase your site's page views, the time spent on your site and reader loyalty.

Related content surfaces content across sections by matching taxonomy tags. If gwurr3tech finds related content, it will automatically output a list of links to that content in reverse chronological order below the byline of your post content.

By default gwurr3tech will display up to 7 items by title along with their reading times. You can limit the number of items displayed by setting the following optional parameter in the `[params]` section of your `config.toml` file:

```toml
related_content_limit = 5
```

### Table Of Contents

Help users locate and share information on your site. By providing a <abbr title="Table Of Contents">TOC</abbr>, users will spend less time scrolling to locate information in larger documents and are more likely to deep to specific information on a page.

To automatically generate a TOC for a post based on the [page outline](https://gsnedders.html5.org/outliner/), add the following to your post front matter:

```toml
toc = true
```

To hide the TOC set `toc = false`, or simply remove the setting from the post front matter.

gwurr3tech uses the HTML5 [`details`](http://devdocs.io/html/element/details) and [`summary`](http://devdocs.io/html/element/summary) elements to provide a TOC which does not require use of CSS or JavaScript to function.

When a page is first loaded, the TOC will be collapsed so it does not clutter up the page. Once expanded, selecting an item in the TOC will smooth scroll to that section within the document, highlight the section header and updating the browser's location bar for deep linking and back-button support.

### Open Graph

gwurr3tech leverages Open Graph tags using the *undocumented* [internal template](https://github.com/spf13/hugo/blob/142558719324aa1628541d556ef1fa2d123f1e68/tpl/tplimpl/template_embedded.go#L159-L201) to achieve rich sharing cards for Facebook and other social networks, as shown here:

![Open Graph sharing card screenshot](https://raw.githubusercontent.com/comfusion/after-dark/a647938/images/docs/feat-social-awareness.png "Example Open Graph sharing card produced by gwurr3tech")

To create a social sharing card like the one shown above, specify `author` in `config.toml` and, optionally, override it from your front matter as shown here:

```toml
title = "Become a Digital Nomad in Bali: The Lost Guide"
description = "Everything you need to know to become a Digital Nomad in Bali."
author = "After Dark"
date = "2017-02-02T11:57:24+08:00"
publishdate = "2017-01-28T02:31:22+08:00"
images = [
  "https://source.unsplash.com/-09QE4q0ezw/2000x1322"
]
```

**Why use array notation for one image?** [The Open Graph protocol](http://ogp.me) supports [arrays](http://ogp.me/#array) and users may wish to extend Hugo internal templates to do so.

To configure site-wide Open Graph images to use as fallbacks for posts not specifying their own open graph images, add an array of URLs to the `[params]` section in `config.toml`:

```toml
images = [
  "https://source.unsplash.com/-09QE4q0ezw/2000x1322" # Default Open Graph image for site
]
```

See [Unsplash Source](https://source.unsplash.com/) for image configuration options.

**Note:** While it would be possible, gwurr3tech does not currently support relative links to images. If you would like to see this feature, please [open a new issue](https://github.com/comfusion/after-dark/issues/new).

### Search Optimization

gwurr3tech is built with SEO in mind. Schema Structured Data and other meta are applied to give robots what they want, automatically, without any configuration necessary.

The default set-up helps ensure your gwurr3tech site will rank well in <abbr title="Search Engine Results Pages">SERP</abbr>s and prevent search crawlers from indexing undesirable content. Fine-tune your search settings using the following available options.

#### Webmaster Verification

Verify your site with several webmaster tools including Google, Bing, Alexa and Yandex. To allow verification of your site with any or all of these providers simply add the following to your `config.toml` and fill in their respective values:

```toml
[params.seo.webmaster_verifications]
  google = "" # Optional, Google verification code
  bing = "" # Optional, Bing verification code
  alexa = "" # Optional, Alexa verification code
  yandex = "" # Optional, Yandex verification code
```

**Note:** Claiming your site with Alexa was [retired in May 2016](https://support.alexa.com/hc/en-us/articles/219135887-Claiming-has-been-retired-May-2016). However, Alexa verification has been added as a convenience for existing sites migrating to gwurr3tech.

#### Meta Descriptions

Well-crafted page titles help catch the human eye on search results pages and meta descriptions provide a summary of the content and why its relevant for the reader, driving click-throughs.

To add a custom meta description to your pages and posts add `description` to the front matter:

```toml
description = "Everything you need to know to become a Digital Nomad in Bali."
```

In addition to appearing in search engines, meta descriptions also appear on individual pages and in content summaries in gwurr3tech, adding transparency to how your page will appear in search.

If no custom description is provided, gwurr3tech will fallback to the description provided in `config.toml`. Learn more on [how to craft your meta descriptions](https://moz.com/learn/seo/meta-description).

#### Modification Dating

Help user agents know when posts were last modified. To do so add `publishdate` to your page front matter and update `date` anytime you make an update to a post.

Updates will be made visible to readers by surfacing content higher in your page and post listings and by using visible callouts on content summaries. For robots, making this change will automatically update Schema Structured Data and Web feeds, as well as the `lastmod` setting in your `sitemap.xml` file.

You can be specific and use a datetime (with timezone offset) like:

```
date = "2017-02-02T01:20:56-06:00"
publishdate = "2016-11-21T10:32:33+08:00"
```

Or less specific and use just the dates:

```
date = "2017-02-02"
publishdate = "2016-11-21"
```

In addition to `date` and `publishdate`, it's also possible to set an expiry date. Expired posts will automatically disappear when the site is built, but the content will be retained. To future- or back-date content for expiration, set the `expirydate` field in the front matter.

#### Index Blocking

Just because a page appears in your `sitemap.xml` does not mean you want it to appear in a SERP. Examples of pages which will appear in your `sitemap.xml` that you typically do not want indexed by crawlers include error pages, search pages, legal pages, and pages that simply list summaries of other pages.

Though it's possible to block search indexing from a `robots.txt` file, gwurr3tech makes it possible to block page indexing using Hugo configuration as well. By default the following page types will be blocked:

- Section Pages (e.g. Post listings)
- Taxonomy Pages (e.g. Category and Tag listings)
- Taxonomy Terms Pages (e.g. Pages listing taxonomies)

To customize default blocking configure the `noindex_kinds` setting in the `[params]` section of your `config.toml`.

For example, if you want to enable crawling for sections appearing in [Section Menu](#adding-a-section-menu), add the following to your configuration file:

```
[params]
  noindex_kinds = [
    "taxonomy",
    "taxonomyTerm"
  ]
```

To block individual pages from being indexed add `noindex` to your page's front matter and set the value to `true`, like:

```toml
noindex = true
```

And, finally, if you're using Hugo `v0.18` or newer, you can also add an `_index.md` file with the `noindex` front matter to control indexing for specific section list layouts:

```shell
├── content
│   ├── modules
│   │   ├── starry-night.md
│   │   └── flying-toilets.md
│   └── news
│       ├── _index.md
│       └── return-flying-toasters.md
```

To learn more about how crawlers use this feature read [block search indexing with meta tags](https://support.google.com/webmasters/answer/93710).

#### Link Types

For related content split across multiple pages in a sequence gwurr3tech supports use of `prev` and `next` settings in your front matter. Use them to provide semantic relationships between pages in a segmented article or series or [LiveBlogPosting](https://schema.org/LiveBlogPosting).

```toml
prev = "/series/learn-to-code/part-one/"
next = "/series/learn-to-code/part-three/"
```

Link Types are commonly shown at the top of the page in terminal browsers as auxiliary means of navigation and may help crawlers better understand relationships within your content.

Learn more about [link types](http://devdocs.io/html/link_types) and how to [customize Hugo taxonomies](https://gohugo.io/taxonomies/overview/).

#### Meta Keywords

Meta keywords offer semantic detail to crawlers regarding the subject matter of your content. Keywords meta are generated automatically for pages given the tags used for that page, and for other pages using the site categories taxonomy. Keywords and key phrases may be customized by setting a `keywords` array in your front matter:

```toml
keywords = [
  "web development",
  "digital marketing",
  "social media",
  "link building"
]
```

While not considered relevant to some crawlers, keywords can be a useful way to document target search terms for use in <abbr title="Pay-Per-Click">PPC</abbr> online advertising and provide semantic value to your pages.

### Content Reuse

Keep your content <abbr title="Don't Repeat Yourself">DRY</abbr> and improve thematic consistency across your site. gwurr3tech provides a number [Shortcodes](https://gohugo.io/extras/shortcodes) and composable components to help you keep your content and layouts easy to maintain.

Take for example gwurr3tech's `blockquote` shortcode:

```html
<blockquote {{ with .Get "class" }}class="{{ . }}"{{ end }} {{ with .Get "citelink" }}cite="{{ . }}"{{ end }}>
  {{ .Inner }}
  {{ with .Get "citelink" }}
    <cite><a target="_blank" href="{{ . }}">{{ $.Get "cite" }}</a></cite>
  {{ else }}
    <cite>{{ .Get "cite" }}</cite>
  {{ end }}
</blockquote>
```

Use it in your page or post markdown files like:

```html
{{< blockquote cite="Bitly" citelink="https://bitly.is/2mkxskj" >}}
  <p>When you create your own Branded Short Domain, you can expect to see up to a 34% increase in CTR when compared to standard bit.ly links.</p>
{{< /blockquote >}}
```

Additional theme-provided shortcodes at your disposal:

- `figure` - Similar to the Hugo built-in, but with [Intelligent Lazy Loading](#intelligent-lazy-loading), an adjusted caption title and smaller caption text.




## License

Copyright © 2016-2017 Josh Habdas <josh@habd.as>
<br>
Copyright © 2017 Glenn Wurr III <glenn@wurr.net>
<br> DWTFYWTPLv2 and ISC License

<br>
<br>
Proudly includes Javascript from [Lazysizes](https://github.com/aFarkas/lazysizes/blob/gh-pages/LICENSE) `MIT License` <br>
Copyright (c) 2015 Alexander Farkas
<br>
Proudly includes Javascript from [Smoothscroll](https://github.com/iamdustan/smoothscroll/blob/master/LICENSE) `MIT License` <br>
Copyright (c) 2016 Dustan Kasten, Jeremias Menichelli
<br>
Proudly includes Javascript from [libbpg](https://bellard.org/bpg/) `LGPL/BSD License` <br>
Copyright (c) 2016 Fabrice Bellard
<br>

