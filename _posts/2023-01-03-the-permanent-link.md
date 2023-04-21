---
layout: post
title: The Permanent Link
date: 2023-01-02 21:44
redirect_from:
 - /2023/01/03/the-permanent-link.html
---

## No Permanence is Ours

I've been delaying posting for some time because of permalinks. Web content is addressed by Uniform Resource Locators (URLs) and URLs are expected to remain the same as long as the page is published on the web. The reality is URLs change all the the time for all sorts of reasons, contributing to [link rot](https://en.wikipedia.org/wiki/Link_rot) and overall degradation of the web as a useful and interesting interconnection of resources.[^1]

In order for me to publish things to the web they have to have URLs. This site uses Jekyll to convert markdown files to html, and Jekyll also handles URL construction based on configurable rules. The ways Jekyll can be configured to create URLs from content properties is outside the scope of this post, but the question I have to answer is, what are the best URLs to create? The answer is ... it depends. I created www.guyinterlinked.com with a few principles in mind, and and at least two of them are encoded in the URL of the website itself. The principles are interlinked-ness, and intentionality in expressing meaning in every design and structural choice of the site. After all, [the medium is the massage](https://en.wikipedia.org/wiki/The_Medium_Is_the_Massage)

<!-- more -->

The first principle, interlinked-ness, is best served by permanent links. Once a link is created on my site, I'd like it to remain in perpetuity.[^2] By [default](https://jekyllrb.com/docs/configuration/default/), Jekyll permalink style is `date`, so blog posts on this site will have the style `www.guyinterlinked.com/2022/09/25/hello-world.html`. Fine. Good. It's unique, permanent. Also, it's ugly. The date in the context of a blog post is almost entirely irrelevant to the user[^3]. Permalink style of `date` passes the first principle, but fails the second. An URL can and should be more than just an address. Like a [cool map](https://en.wikipedia.org/wiki/Carta_marina), or the [first sentence in a book](https://review.gawker.com/the-50-best-first-sentences-in-fiction-1665532271), I want URLs that suggest to the user what to expect, and perhaps suggest to them what they will find if they follow it. I want an URL to look nice up in the address bar. Is this slavery to aesthetics too extreme? Probably. Will anyone notice or appreciate the effort? Probably not, but the whole purpose of this site is *"the site"* and not just *"the content."* Or to put it better the content, presentation and URLs *are* *"the content."*

A natural first step in the pursuit of elegant URLs would be to include post categories. This is easily supported, but also leads to further questions. As of this post I don't know what kind of categories I would use or want, so what happens if I wish to change categories of a post in the future? Any solution I choose now would remain at risk of mutability.

So what is a Guy to do? If I post without changing the permalink style, I end up with permanent yet unrefined URLs. If I research how to create the quintessential URL, I'll never post anything. At first I wasn't sure if there is even a way to resolve this problem, but duh, of course there is. Since GitHub Pages on github.com are just repositories, I nosed around on one of the blogs that inspired me, [https://orbitalflower.github.io](https://orbitalflower.github.io). I was looking at the front matter for the post [Data's password](https://orbitalflower.github.io/tv/startrek/datas-password.html). Below is an excerpt of the header:

```markdown
---
title: Data's password
categories: tv startrek
redirect_from: /2015-05-22-how-strong-is-datas-password.html
description: "How strong is the password Lt. Commander Data sets in the Star Trek: TNG episode 'Brothers'?"
---
```

Do you see the solution? Of course you do! The `redirect_from` line. It looks like back when the Orbital Flower blog was started, it had a different permalink setting, but when it was changed, the redirect_from handled the links coming from the hold style permalink! However, what was it changed to? Well, looking at the [_config.yml](https://github.com/orbitalflower/orbitalflower.github.io/blob/master/_config.yml) it apparently is `none`. If I wasn't so lazy right now I could go back through the changelog and figure out the exact timeline of changes.

I'm not going to pretend I know what that means or how to get what I really want at this point, but I'm going to blaze forward with faith and ugly URLs, confident that one day I'll have the knowledge to bring everything into alignment with my principles.

>No permanence is ours; we are a wave  
>That flows to fit whatever form it finds:  
>Through day or night, cathedral or the cave  
>We pass forever, craving form that binds.
>
> -- Herman Hesse, The Glass Bead Game

## Further Reading

- [Jekyll Docs on Permalinks](https://jekyllrb.com/docs/permalinks/)
- [The Inventors of the Internet Are Trying to Build a Truly Permanent Web](https://www.wired.com/2016/06/inventors-internet-trying-build-truly-permanent-web/)

## Footnotes

[^1]: Although continuing with the spiderweb metaphor, webs break all the time.

[^2]: Or, as Avi Halaby in Neal Stephenson's *Cryptonomicon* would suggest, "for as long as men are capable of evil."

[^3]: Upon reflection, for some categories or types of posts, the date would be of interest. Some posts could indeed be timeless, and others would indeed be time bound. Would the date help or hinder.
