---
layout: post
title: Showing Updated Date on Jekyll Posts - Without Using Plugins
date: 2023-03-17 16:01 -0700
---

Let's say you've performed a [blog content audit](https://www.contentpowered.com/blog/perform-blog-content-audit/) on your Jekyll powered website and identified some popular but older posts that need brushing up. You make the edits, and you want all your visitors to know that the posts are revamped by showing the new date on the post. The naïve way to solve the problem would be to update the `date` variable in the post front matter, but has high probably of unintended consequences.[^1] 

The simplest way to show an updated date in a post is to use a custom variable in the front matter block of the post and update the post layout to use the new variable. In this post I'll walk through the steps, and cover some further considerations and alternatives.

<!-- more -->

# Step 1: Creating the Custom Variable
I've chosen to create a custom variable named `updated_date` and include it in the front matter of the target post. Creating a custom variable is as simple as adding the variable name to the front matter, as demonstrated below:

```
---
layout: post
title: Really Old Post with Recent Update
date: 1982-10-13 09:00 -0700
updated_date: 2023-03-17 09:15 -0700
---
```

# Step 2: Updating the Post Layout

Adding a variable to a post's front matter will not necessarily change the way the post is generated. The post's structure will depend on the structure defined in the `_layouts/post.html`. The Jekyll default [minima theme](https://rossrichardson.github.io/minima-theme.html) has the following post layout for the date:

```
{% raw %}
<time class="dt-published" datetime="{{ page.date | date_to_xmlschema }}" itemprop="datePublished">
    {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
    {{ page.date | date: date_format }}
</time>
{% endraw %}
```
On a post, this ends up looking like this:

![Minima theme default published date](/assets/images/uploads/updated-date-original-template.png)

For my new post layout, I've chosen to display both the published date and the updated date. I need to add [Liquid flow control](https://shopify.github.io/liquid/tags/control-flow/) to check for the `page.updated_date` variable, and output if it exists. I want to add a short description to each date in order to distinguish them. I do this by editing the `_layouts/posts.html` to the following:

```
{% raw %}
<time class="dt-published" datetime="{{ page.date | date_to_xmlschema }}" itemprop="datePublished">
    {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
    Published {{ page.date | date: date_format }}
    {%- if page.updated_date -%}
    &nbsp// Updated {{ page.updated_date | date: date_format }}
    {%- endif -%}
</time>
{% endraw %}
```
For posts with an `updated_date`, the new layout will look like this:

![New template with published and updated date](/assets/images/uploads/updated-date-new-template.png)

That's it! In the future as I update old posts, I just have to remember to add the `updated_date` variable to the front matter, and the next build will include updated date on the post.

# Other Considerations
 - You may not have a `_layouts/post.html` file in your site, especially if you've just started with Jekyll. In that case you'll have to create one. See the Jekyll documentation for [overriding theme defaults](https://jekyllrb.com/docs/themes/#overriding-theme-defaults).
 - If you want to show only the most recent date on a post, just use an if-else statement in the post layout file. See Tom Kadwell's post [Adding last modified date to Jekyll](https://tomkadwill.com/adding-last-modified-date-to-jekyll) for an example.
 - Depending on your home layout template, it may be setup to display the `post.date`, which you may want to change.

# Automatic Alternatives
I've chosen to update post dates manually for several reasons,[^2] but if that won't work for you, there are several different ways to achieve similar results automatically.

- Plugins, more specifically [last-modified-at](https://github.com/gjtorikian/jekyll-last-modified-at). It uses front matter variables just as I've outlined in this post, but does the layout work for you. Note: as of the publishing of this post, `last-modified-at` is not on the GitHub Pages [plugin whitelist](https://pages.github.com/versions/), so you'll have to jump through some additional hoops to use it with GitHub Pages.
- Plugin Hooks. I'll admit I know little about this option, but you can read more about it this [StackOverflow answer](https://stackoverflow.com/a/36769049) and the [Jekyll docs on hooks](http://jekyllrb.com/docs/plugins/hooks/). Like most plugins, hooks are not natively supported by GitHub Pages
- Pre-commit scripts. These will update the contents of a post's front matter variables before changes are committed. See [StackOverflow](https://stackoverflow.com/a/33721446) for more.
- JavaScript. Ryan Baumann shared a neat little trick to [use GitHub API to get latest commit for current page](https://ryanfb.xyz/etc/2020/04/27/last_modified_dates_for_github_pages_jekyll_posts.html). I also really like how Ryan links to the GitHub revision history of a post. I may steal that.

# Why "updated_date"?
Without [digressing]({% post_url 2023-01-16-meta-monday-digressions %}) into the [hard problems of computer science](https://darren-broemmer.medium.com/debunking-the-infamous-only-two-hard-problems-in-computer-science-b412a31c00df), I chose the variable name `updated_date` over other potential options for the following reasons:
- I used [snake_case](https://en.wikipedia.org/wiki/Snake_case) because other [Jekyll variables](https://jekyllrb.com/docs/variables/) use it, and I cherish the consistency.
- The `post.date` variable is used implicitly as meaning either `published_date` or `date_published` and I wanted to use a similar term. (again, consistency)
- Although used interchangeably, "modified" and "updated" have different connotations. A "modified post" carries the sense that small, technical details of a post have changed, whereas "updated post" suggests that the content of the post has been refreshed or made new. Since I expect the readers of this site to be interested in the latter over the former, "updated" makes the most sense to communicate my intent.[^3]
- `updated_date` sounds better in my head when I read it compared with `date_updated`.


# Further Reading
 - [Published vs Last Updated Date: Which is Better for SEO?](https://www.contentpowered.com/blog/published-modified-date-seo/)
 - [Debunking the infamous “Only two hard problems in Computer Science"](https://darren-broemmer.medium.com/debunking-the-infamous-only-two-hard-problems-in-computer-science-b412a31c00df)

# Footnotes
[^1]: `post.date` variable is used in a lot of places internally to Jekyll. As an example, the default [permalink]({% post_url 2023-01-03-the-permanent-link %}) format includes a post's date in the URL, so when the `date` variable of a post is changed in can break incoming links.

[^2]: Reason 1: If I'm already revising a post, it isn't a bother to also update a variable. Reason 2: I need the control. All the automated solutions (apart from [last-modified-at](https://github.com/gjtorikian/jekyll-last-modified-at)) use the file modified date to set the "updated" variable. In my opinion a file being modified is not the same thing as a post being updated. Fixing spelling errors or grammar issues are not things I would consider an "update." Fixing broken links aren't necessarily an "update." Adding or removing front matter variables is not a post "update." I want to reserve the "update" terminology for when there are significant structural or content changes.

[^3]: Not that you, dear reader, will ever see the variable. Unless you are reading this post. In which case I hope you appreciate my attention to detail.