---
title: "How to add a Comment Section to your Hugo PaperMod Blog using giscus"
date: 2024-12-20T19:04:00+01:00
draft: false
categories: [coding]
tags: [hugo, papermod, giscus, blog, coding]
ShowToc: true
tocopen: true
cover:
    image: "images/cover.webp"
    alt: "cover picture: example of a giscus comment"
    relative: true
    caption: ""
---

*A guide on how to set up a comment section in your [Hugo](https://gohugo.io/) [PaperMod](https://github.com/adityatelange/hugo-PaperMod) blog using [giscus](https://github.com/giscus/giscus).*

<!--more-->


## Versions used

At the time of writing this article, I'm using the following versions:
* Hugo: v0.139.4
* PaperMod: v8.0
* giscus: (is rolling release) last [changelog](https://github.com/giscus/giscus/blob/main/CHANGELOG.md) entry on 2024-11-28


## Next Steps depending on your PaperMod "Installation"
Depending on how you added PaperMod to your repo we may need to make some changes.

[Aditya Telange](https://github.com/adityatelange) discusses in the [installation instructions for PaperMod](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation) four ways to add this theme to your Hugo blog:
* **"Git Clone" and "Download and unzip"**

    If you have chosen this approach, you are lucky because you can immediately start modifying the code. However, you have to apply these changes again, when you update your PaperMod theme to a newer version. You can continue reading the chapter [*Modifying `comments.html`*](./#modifying-commentshtml).
    
* **"Git Submodule (recommended)"**

    Users that included PaperMod using this method need to make some changes in order to be able to push the commits to their repo on GitHub.
    Please read the chapter [*Forked Git Submodule*](./#forked-git-submodule) before continuing with chapter [*Modifying `comments.html`*](./#modifying-commentshtml).
    
* **"Hugo module"**

    I guess you can do the same as for "Git Clone" and "Download and unzip" but I am not sure how the Hugo [module update command](https://gohugo.io/hugo-modules/use-modules/#update-all-modules) behaves with local changes applied. It probably will just overwrite it. You can continue reading the chapter [*Modifying `comments.html`*](./#modifying-commentshtml).


## Forked Git Submodule
The issue of having the [original PaperMod repo](https://github.com/adityatelange/hugo-PaperMod) as a submodule in your blog repo is that we need to make some changes to the theme and push the commit to GitHub. But since we are tracking the original PaperMod repo we can not push our changes to the submodule.

The solution to this problem is to create a fork of the PaperMod repo and tell git that we now want to track this remote in our submodule. I recommend to perform the following steps:
1. Create a fork of the original PaperMod repo.
2. Create a branch called something like `add_giscus`.
3. In your repo folder run the following commands:
    * `git submodule set-url themes/PaperMod https://github.com/<YOUR_USERNAME>/hugo-PaperMod` - this will track the forked repo
    * `git submodule update --remote --recursive` - this will update the submodule to take the content from your forked repo
4. Navigate to your submodule folder (`cd themes/PaperMod`) and run the following command:
    * `git checkout add_giscus` - checks out the branch we have created before

Now we are ready to make our modifications.

## Modifying `comments.html`
In the folder `themes/PaperMod/layouts/partials/` you will find the file `comments.html`. Open this file with your favorite text editor (I like [micro](https://micro-editor.github.io/)) and delete the stuff that's already inside (there should have been just some comments).

Now we need to place the code for giscus here. On the [giscus page](https://giscus.app/), there is a nice config tool that generates the code for you. It will look something like this:
```html
<script src="https://giscus.app/client.js"
        data-repo="[ENTER REPO HERE]"
        data-repo-id="[ENTER REPO ID HERE]"
        data-category="[ENTER CATEGORY NAME HERE]"
        data-category-id="[ENTER CATEGORY ID HERE]"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="transparent_dark"
        data-lang="en"
        crossorigin="anonymous"
        async>
</script>
```

**I highly recommend the "transparent_dark"-theme, as it will work with both the dark and the bright blog theme of Hugo PaperMod.**

In case you don't have PaperMod added as a submodule you can continue with the next chapter. Otherwise, you would still need to commit and push the changes from the submodule to your PaperMod fork repo.


## Enable the Comments Feature in the Hugo Config File
In the Hugo YAML config you would just add `comments: true` to the `params`-section. Of course, you need to adjust the syntax in case you use a TOML or JSON config file.


## Enable Discussions for Your GitHub Repo
You can enable discussions under: Settings &rarr; General &rarr; Features &rarr; Discussions


## Install the giscus App in GitHub
You can find the [giscus app here](https://github.com/apps/giscus).


## Publishing
Commit and push all your changes from the blog repo to GitHub. In case you use the submodule approach, make sure that you also push this new version of the submodule to the blog repo.


## Last Words regarding Updating PaperMod in the Future
* Users that use the submodule method can follow this approach:
    1. Sync the `master` branch of your fork with the original repo.
    2. Merge the `master` branch in your `add_giscus` branch.
    3. Update your submodule using `git submodule update --remote --recursive`.
    4. Make sure that locally you are still in your `add_giscus` branch.
* All other users need to follow this approach:
    1. Store the code from the `comments.html` file somewhere.
    2. Perform your update.
    3. Apply the code back to the `comments.html` file.
