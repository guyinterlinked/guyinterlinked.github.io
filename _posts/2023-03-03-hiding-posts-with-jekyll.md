---
layout: post
title: "Hiding Posts From the Home Page With Jekyll"
date: 2023-03-03 21:22
hidden: true
---

A couple of weeks ago I was working on the [game of posts]({% post_url 2023-02-10-a-game-of-posts%}) and wanted to implement a structure with a main post visible from the home page, and from that main post link to other posts in the series (and from those posts, link to other posts, etc.). Aside from the main post, I would like all other posts in the series to be invisible from the home page, the idea being the only way to read the posts is to play the game.[^1] During my research I came across the post variable `hidden`, which when set `hidden: true` will prevent a given post from appearing in [`paginator.posts`](https://jekyllrb.com/docs/pagination/). I assumed that setting `hidden: true` would also hide the posts from other places, but that is not the case.[^2]

My understanding of Jekyll is still quite superficial, and in a hurry to implement *something*, I misunderstood `hidden: true` to be a magic bullet I could ... shoot at the problem. Let's check out my [index.md](https://github.com/guyinterlinked/guyinterlinked.github.io/blob/main/index.md?plain=1) and see if I can get any clues. See below in it's entirety:

```markdown
{% raw %}
    ---
    # Feel free to add content and custom Front Matter to this file.
    # To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

    layout: home
    ---
{% endraw %}
```

Well, that isn't very helpful. Another thing that isn't very helpful in this situation is that default Jekyll installs use the [minima theme](https://github.com/jekyll/minima), and layout files for themes are in the theme's gem _layout directory, [and not in the sites _layout directory](https://stackoverflow.com/questions/52709891/where-to-find-default-layouts-in-jekyll). To solve this problem I had to go off to the mines, the minima-2.5.1 gem[^3] directory to find the `home.html` layout. Relevant excerpt below.

```html
{% raw %}
      {%- for post in site.posts -%}
      <li>
        {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
        <span class="post-meta">{{ post.date | date: date_format }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
        {%- if site.show_excerpts -%}
          {{ post.excerpt }}
        {%- endif -%}
      </li>
      {%- endfor -%}
{% endraw %}
```

This code[^4] is for generating the list item elements for each post in `site.posts`. I now know that `hidden: true` only hides posts from `paginator.posts` I've seen some people suggest using `published: false` on the post, which would indeed prevent the posts from appearing on the home page, but only because the post wouldn't be published, and therefore not visible anywhere. As I understand the issue now, I could use the `hidden` variable to hide a post, but I'd have to change the home layout from stock. I plan to adapt the solution from [here](https://stackoverflow.com/questions/23935062/jekyll-github-pages-how-to-hide-a-post), something like the following:

```html
{% raw %}
    {%- for post in site.posts -%}
        {% if post.hidden == null or post.hidden == false %}
            <li>
            {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h3>
                <a class="post-link" href="{{ post.url | relative_url }}">
                    {{ post.title | escape }}
                </a>
            </h3>
            {%- if site.show_excerpts -%}
                {{ post.excerpt }}
            {%- endif -%}
            </li>
        {% endif %}
    {%- endfor -%}
{% endraw %}
```

The eagle eyed among you will have noticed  `if post.hidden == null or post.hidden == false` just before the `<li>` tag. This checks the potential post's `hidden` variable and if it's not set, or `false`, it will display the post in the home page. Otherwise the post will not be listed.

There are two paths to go about changing this. The simplest is to [override the theme default](https://jekyllrb.com/docs/themes/#overriding-theme-defaults) home layout as above. The less simple would be to throw the whole theme out entirely and roll my own, something a bit more personal.[^5] I had always intended to update my layouts and css anyway, just not quite so soon. I don't have plans to hide many posts, and if I use the `hidden` variable, there is potential its behavior in Jekyll might change in the future in a way I don't expect. I'll probably just do things the easy way for now and override the theme. A site redesign will wait until later.

## Further Reading

- [A more general approach to filtering site.posts](https://talk.jekyllrb.com/t/how-to-use-variable-in-a-jekyll-liquid-loop/7279)
- [Escaping double curly braces inside a markdown code block with Jekyll](https://stackoverflow.com/questions/24102498/escaping-double-curly-braces-inside-a-markdown-code-block-in-jekyll)
- [Even more about escaping curly braces for Jekyll (it's complicated)](https://blog.slaks.net/2013-06-10/jekyll-endraw-in-code/)

## Footnotes

[^1]: When I first imagined the game of posts I simply planned to publish all posts on the same day, all visible. But this wouldn't actually work because if I publish six or seven posts on the same day, Jekyll is going to render them in a certain order, and the whole point of the game is that there isn't a specific order to the posts. I don't want there to be a "canonical" order, explicitly or implicitly. In order to control the reader's flow through the posts, I'd have to create a single landing post, and branch out from there.

[^2]: I bet you didn't notice that *this* post has `hidden: true`. Although this means that if and when I do figure out how to hide posts using the `hidden` variable, this post will disappear the main page. I appreciate the irony.

[^3]: ![Finding a gem](/assets/images/uploads/hiding-posts-finding-gems.jpg)

[^4]: I didn't think I was actually going to be able to share this code, since I was in a hurry earlier and couldn't figure out how to post Jekyll code in a markdown file that would end up being parsed by Jekyll. My first attempts were actually quite interesting, in that I had [liquid](https://jekyllrb.com/docs/liquid/) within a markdown code block, so when Jekyll parsed the liquid it generated the HTML, but instead of rendering as a webpage, it just rendered the HTML raw, within the code block. That actually seems like it could be useful, like if you had to demonstrate the HTML liquid would produce, instead of writing out the html or copying from a generated file, you could just put the liquid in a code block.

[^5]: I have not hidden the fact I would like a mid-to-late 1990s web aesthetic for my site. A friend of mine was imagining a deep fried millennial style, as you might find on [neocities.org](https://neocities.org/browse). I'm not sure I'm that bold. I was thinking a conservative [Yahoo! in 1994](https://www.webdesignmuseum.org/exhibitions/web-design-in-the-90s/yahoo-1994) style. But with [acid green]({% post_url 2023-01-14-hot-design-tips-circa-1998 %})
