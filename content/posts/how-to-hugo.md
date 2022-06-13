+++
title = "How to setup your own website powered by Hugo"
summary = "How to use Hugo to setup your own website"
date = "2021-06-29T16:23:24-03:00"
draft = false
authors = ["Tales L. Aparecida"]
categories = ["Guides"]
tags = ['blog', 'hugo', 'nocode']
featuredImage = "https://raw.githubusercontent.com/gohugoio/gohugoioTheme/master/static/images/hugo-logo-wide.svg?sanitize=true"
+++

# What is Hugo

> Hugo is a static HTML and CSS website generator written in [Go](https://golang.org/). It is optimized for speed, ease of use, and configurability. Hugo takes a directory with content and templates and renders them into a full HTML website.
>
> src: https://github.com/gohugoio/hugo

Or, in my case, it's a tool that allowed me to setup my own blog, writing posts using Markdown and github.io for hosting.

You can follow their [tutorial](https://gohugo.io/getting-started/quick-start/) to setup your static website too! Trust me, it's pretty straight forward, just:

1) [Install Hugo](https://gohugo.io/getting-started/installing/)
2) Create a new site with: `hugo new site NAME_HERE`
3) Add and customize a theme (there are dozens of options!)
4) Add some content
5) Check your workings
6) Publish!


# Add and customize a theme

Hugo community has created many (almost 300 up until now) beautiful themes. You can choose one at their [gallery](themes.gohugo.io/). Each one has its own set of customizations, which you can usually find more about in their GitHub page.

This site, the one you are reading right now, uses [Hugo-Coder](https://github.com/luizdepra/hugo-coder/) as its theme, created by [Luiz de Prá](https://github.com/luizdepra), which needed no effort besides cloning as a submodule with `git submodule add https://github.com/luizdepra/hugo-coder.git themes/hugo-coder` and setting a `config.toml` file, which you may copy [from my repo](https://github.com/tales-aparecida/tales-tips-and-tricks/blob/main/config.toml).

![Hugo-coder home page](https://github.com/luizdepra/hugo-coder/raw/main/images/screenshot.png)


# Add some content

In this step you will need to run:

```sh
hugo new posts/my-first-post.md
```

And then fill the [markdown](https://www.markdownguide.org/) file with your own ideas!

Note that Hugo posts have a header called "[Front Matter](https://gohugo.io/content-management/front-matter)", a section at the top of the file in [4 different possible formats](https://gohugo.io/content-management/front-matter#front-matter-formats) where you can set attributes about your post, like `title`, `description`, `date` and `tags`.

# Check your workings

To visualize your website in a browser, run:

```sh
hugo server -D
```

If there aren't any errors, this should print something like:

```
Start building sites … 

                   | EN  
-------------------+-----
  Pages            | 26  
  Paginator pages  |  0  
  Non-page files   |  0  
  Static files     |  5  
  Processed images |  0  
  Aliases          | 10  
  Sitemaps         |  1  
  Cleaned          |  0  

Built in 79 ms
Watching for changes in /home/tales/codes/tales-tips-and-tricks/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /home/tales/codes/tales-tips-and-tricks/config.toml, /home/tales/codes/tales-tips-and-tricks/themes/hugo-coder/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

Which, if you read it carefully, tells that your website can be accessed through http://localhost:1313.

# Publish

At last, to publish your new static website there are some [alternatives](https://gohugo.io/hosting-and-deployment/hosting-on-github/), the one I've chosen was [GitHub Pages](https://help.github.com/articles/what-is-github-pages/) hosted on GitHub's github.io domain, using GitHub Actions.

You just need to create a branch "gh-pages" then go to the repository settings and activate it as the source to your website.

![Activate GitHub Page in the repo settings](/tales-tips-and-tricks/images/gh-pages.png)

Then, create a `.github/workflows/gh-pages.yml` in your main branch to setup the GitHub Actions pipeline, which will build the statics into the `gh-pages` branch, you can copy my [`gh-pages.yml`](https://github.com/tales-aparecida/tales-tips-and-tricks/blob/main/.github/workflows/gh-pages.yml) if you want to, which I got from https://github.com/peaceiris/actions-hugo.

That's it, in your next `git push` you will trigger the deploy and your website will be accessible through the URL informed while activating the GitHub Pages. :D

## User/Organization page vs Repository page

You'll notice that the URL hosting your website looks something like: `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/`.
To fix that, either rename your repository to `YOUR-USERNAME.github.io` or create a repository with that name with a page responsible for redirecting into the correct path.
In that case, you just need to activate the GitHub Pages in the new repo and add the file `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>Redirecting...</title>
    <!-- Redirects to the correct GitHub Pages path in 0 seconds -->
    <meta http-equiv="refresh" content="0; URL='/YOUR-REPO-NAME/'"/>
</head>
<body>

</body>
</html>
```


# Alternatives

If Hugo didn't spark your interest, give a chance to [Jekyll](https://docs.github.com/pt/pages/setting-up-a-github-pages-site-with-jekyll),
the static website generator endorsed by GitHub.
