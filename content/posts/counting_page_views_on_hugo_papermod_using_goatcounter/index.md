---
title: "How to count Page Views on your Hugo PaperMod Blog using GoatCounter"
date: 2025-01-01T15:51:00+01:00
draft: false
categories: [coding]
tags: [hugo, papermod, goatcounter, blog, coding]
ShowToc: true
tocopen: true
cover:
    image: "images/cover.webp"
    alt: "cover picture: hugo papermod blog using goatcounter"
    relative: true
    caption: "Sources: [GoatCounter Logo](https://www.goatcounter.com/), [Hugo Logo](https://gohugo.io/)"
---

*A guide on how to count Page Views on your [Hugo](https://gohugo.io/) [PaperMod](https://github.com/adityatelange/hugo-PaperMod) blog using [GoatCounter](https://www.goatcounter.com/).*

<!--more-->

## Why GoatCounter?
There are many reasons why I think GoatCounter is a good solution for my needs. Here are just a few of them:

* It's FOSS (Free Open Source Software).
* You can self-host it, but it's not necessary.
* It does not track users with unique identifiers and therefore does not need a GDPR notice.
* It's super easy to add to your page.
* It offers JavaScript tracking with extended tracking information, but also non-JavaScript pixel-based tracking with reduced tracking information.

My initial goal was to keep this blog JavaScript-free. Unfortunately, I had to make an excuse for the [giscuss comment section](/posts/using_giscus_with_hugo_papermod/). Nevertheless, I don't want to spam watchers of this page with tons of scripts and therefore I chose the GoatCounter pixel-tracking approach over the JavaScript approach.


## Caveats of using GoatCounter
* The websites `goatcounter.com` and/or `gc.zgo.at` seem to be blocked by many add-blockers and therefore not ALL page visits are tracked.
* GoatCounter ignores Do Not Track. (Here are the author's [arguments against DNT](https://www.arp242.net/dnt.html).)


## Versions used
At the time of writing this article, I'm using the following versions:
* Hugo: v0.139.4
* PaperMod: v8.0
* GoatCounter: v2.5.0


## Before starting
In this guide, I assume you have already created an account on [www.goatcounter.com](https://www.goatcounter.com/).

> &#x26a0;&#xfe0f; Depending on how you added PaperMod to your Hugo repo the next steps might differ. I have already explained the differences in an [article](/posts/using_giscus_with_hugo_papermod/) before. Mainly, the sections [*Next Steps depending on your PaperMod "Installation"*](/posts/using_giscus_with_hugo_papermod/#next-steps-depending-on-your-papermod-installation), [*Forked Git Submodule*](/posts/using_giscus_with_hugo_papermod/#forked-git-submodule), and [*Last Words regarding Updating PaperMod in the Future*](/posts/using_giscus_with_hugo_papermod/#last-words-regarding-updating-papermod-in-the-future) are interesting here.


## Add `analytics.html`
In the folder `themes/PaperMod/layouts/partials/` you need to add the file `analytics.html`. Depending on your preference, you can either add this code for using the JavaScript-tracking:
```html
<script data-goatcounter="https://MYCODE.goatcounter.com/count"
        async src="//gc.zgo.at/count.js"></script>
<noscript>
    <img src="https://MYCODE.goatcounter.com/count?p={{ .Path | safeURL }}">
</noscript>
```
or as I did, you can just use the following line for pixel-tracking:
```html
<img src="https://MYCODE.goatcounter.com/count?p={{ .Path | safeURL }}">
```

> &#x26a0;&#xfe0f; Don't forget to replace the string `MYCODE` with your actual code that you selected during account creation.


## Modify `baseof.html`
In the folder `themes/PaperMod/layouts/_default/` you need to modify the file `baseof.html`. This file acts as the base for all our pages.

We need to add the line `{{ partial "analytics.html" . .Path -}}` below `</main>` and above our *footer*. See the highlighted line below:
```go-html-template {hl_lines=["13"]}
<body class="
{{- if (or (ne .Kind `page` ) (eq .Layout `archives`) (eq .Layout `search`)) -}}
{{- print "list" -}}
{{- end -}}
{{- if eq site.Params.defaultTheme `dark` -}}
{{- print " dark" }}
{{- end -}}
" id="top">
    {{- partialCached "header.html" . .Page -}}
    <main class="main">
        {{- block "main" . }}{{ end }}
    </main>
    {{ partial "analytics.html" . .Path -}}
    {{ partialCached "footer.html" . .Layout .Kind (.Param "hideFooter") (.Param "ShowCodeCopyButtons") -}}
</body>
```


## Publishing
Commit and push all your changes from the blog repo to GitHub. If you use the submodule approach, make sure that you also push this new version of the submodule to the blog repo.


## View Analytics
To view the analytics of your website you need to open the GoatCounter-dashboard and sign in with your account credentials. Here the link: `https://MYCODE.goatcounter.com/`
