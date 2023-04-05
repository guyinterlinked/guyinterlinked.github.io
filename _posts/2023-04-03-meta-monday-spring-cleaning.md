---
layout: post
title: "Meta Monday: Spring Linting"
date: 2023-04-03 21:39
---

Here at Guy, Interlinked, we like to do things right, but more importantly we like to do things. Sometimes in the aid of doing things, we have to choose between doing things and doing things right. We choose to err on the side of doing things.[^1] But another thing we like to do here at Guy, Interlinked is learn things, ideally while also doing things. In posting over the last three months, I've learned more about the types of things I want to get right, and I think it's about time to clean house.

## The Guy Interlinked Workflow

[![The Spring Clean - Unknown artist (Spanish School)](https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/The_Spring_Clean_-_Unknown_artist_%28Spanish_School%29.jpg/512px-The_Spring_Clean_-_Unknown_artist_%28Spanish_School%29.jpg "Unidentified artist, Spanish School, 19th Century, Public domain, via Wikimedia Commons")](https://commons.wikimedia.org/wiki/File:The_Spring_Clean_-_Unknown_artist_(Spanish_School).jpg)

My workflow is a mess, but not such a mess that it prevents me from "doing things." I use [VS Code](https://code.visualstudio.com/) for all my writing and development work. The first extension I added was [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker),[^2] then [Word Count](https://marketplace.visualstudio.com/items?itemName=ms-vscode.wordcount), and finally [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint).

## The Cruft

I've enjoyed [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint), as it definitely highlighted the lack of consistency and standards in my earlier posts. I don't want to be a slave to code style, but I also like the consistency and organization it brings. My more recent posts are already setup.

### Markdown Linting

There were lots of little things wrong here and there, but the most confusing warning was rule `MD025 - Multiple top-level headings in the same document`. Reading the [documentation](https://github.com/DavidAnson/markdownlint/blob/v0.27.0/doc/md025.md), the warning comes when a front matter `title` variable is set, that is considered to be the top level heading, and any other top level heading is considered a duplicate. The warning can be disabled, but I'm ok with following the rule at this time.

The other big flag was having inline HTML, most egregiously in [Three Hundred Game Mechanics](https://github.com/guyinterlinked/guyinterlinked.github.io/blob/main/_posts/2023-03-11-three-hundred-game-mechanics.md?plain=1). I have endeavored to convert it to pure markdown.

### Post Date/Times

As mentioned in [The Permanent Link]({% post_url 2023-01-03-the-permanent-link %}), for now my permalink style is "default", in this case `/year/month/day/slug.html`. On several posts since then I have included the wrong date in the posts front matter. If I fix the post date, it will change the permalink. Technically I could leave the posts the way they are as no one would notice or care the posts are off by a day, but I care, so I'm going to fix it. To do so, I have to enable the [jekyll-redirect-from](https://rubygems.org/gems/jekyll-redirect-from) plugin and and add the `redirect-from:` variable value to the front matter of the offending posts.[^3]

Post times and UTC offsets are another issue. I think I'm ok the way I am now, with timezone set in the `_config.yml`, but I was also setting UTC offsets in posts dates.  I'm going to remove those for now, but I need to do more research.

## TODO

I'm going full nerd and want to incorporate MLA style more fully to my posts, which means I'll have to be more strict about things like block quoting and citation. This will be most notable on [Three Hundred Game Mechanics]({% post_url 2023-03-11-three-hundred-game-mechanics %}), but will affect other posts as there will more distinction between footnotes and works cited. Guy, Interlinked is still a one Guy show, but I'd like to develop a style guide at some point.

## Further Reading

- [MLA Works Cited Style](https://www.scribbr.com/mla/works-cited/)

## Footnotes

[^1]: Ideally we don't do things wrong, just less than right.

[^2]: Prior to this, I wasn't spell checking posts at all! *gasp*

[^3]: The posts with the wrong dates were [Review: Sleuth]({% post_url 2023-02-24-review-sleuth-1983 %}) and in perfect irony [Updated Dates on Jekyll Posts]({% post_url 2023-03-18-updated-dates-on-jekyll-posts %}) and [The Permanent Link]({% post_url 2023-01-03-the-permanent-link %})
