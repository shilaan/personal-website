---
title: Building your website using R {blogdown}
author: Shilaan Alzahawi
date: '2021-05-12'
draft: yes
slug: building-your-website-using-r-blogdown
categories: []
tags: []
subtitle: 'A concise step-by-step guide'
summary: ''
authors: []
lastmod: '2021-05-12T13:46:11-07:00'
featured: no
image:
  placement: 2
  caption: 'Image credit: [**Xie, Thomas, Hill 2018**](https://www.routledge.com/blogdown-Creating-Websites-with-R-Markdown/Xie-Hill-Thomas/p/book/9780815363729)'
  focal_point: 'Center'
  preview_only: no
projects: []
---

{{< toc >}} 


I finally decided to bite the bullet and create my own website using {{< icon name="r-project" pack="fab" >}} `{blogdown}`. If you're like me, you've already seen many inspiring examples of websites created with blogdown (for example, the webpages of [Julia Silge](https://juliasilge.com/about/), [Silvia Canelón](https://silvia.rbind.io), and  [Iván Mauricio Cely Toro](https://mauriciocely.github.io)), but the process of creating it sounds slightly overwhelming. 😰 

If so, this post is for you: I'll walk you through my step-by-step process of building a site using [blogdown](https://github.com/rstudio/blogdown) and the [Wowchemy](https://wowchemy.com) theme for [hugo](https://gohugo.io) and deploying it with [netlify](https://www.netlify.com). I largely followed the process documented [here](https://alison.rbind.io/post/new-year-new-blogdown/) by Alison Hill. The good news: we'll mostly build and customize the website from the comfort of RStudio. 🥳 

My goal is to write a concise step-by-step guide without having you worry too much about what's happening behind the scenes. For a much more comprehensive guide, please check out [Alison's blog](https://alison.rbind.io/post/new-year-new-blogdown/).


## Prerequisites 

- {{< icon name="download" pack="fas" >}} Install a recent version of [R](https://cran.r-project.org) and [RStudio](https://www.rstudio.com/products/rstudio/download/)
- {{< icon name="github" pack="fab" >}} [Create](https://github.com/join) a GitHub account
- {{< icon name="r-project" pack="fab" >}} [Connect](https://happygitwithr.com/connect-intro.html) RStudio to GitHub (preferably with [HTTPS](https://happygitwithr.com/credential-caching.html#credential-caching))
- {{< icon name="user-plus" pack="fas" >}} [Sign up](https://www.netlify.com) with Netlify using your GitHub account

## Step 1: Create GitHub repository 

{{< cta cta_text="Click to create a new repository" cta_link="https://github.com/new" cta_new_tab="true" >}}

{{% callout note %}}
Use the following settings:
- Keep the repository `Public`
- Add a `README` file
- Do not add `.gitignore`
{{% /callout %}}

{{< spoiler text="Click to view screenshots for Step 1" >}}
![](new-repository.png)
{{< /spoiler >}}

## Step 2: Create project with Version Control in RStudio


- [ ] Go to `https://github.com/your-username/your-repository` 
- [ ] Click on the green `Code` button 
- [ ] Copy the `HTTPS` link to your clipboard
- [ ] Go to `RStudio > File > New Project >  Version Control > Git`
- [ ] Copy paste the `HTTPS` link under `Repository URL`.
- [ ] Click `Create Poject`

{{< spoiler text="Click to view screenshots for Step 2" >}}
![](https.png)
![](new-project.png)
![](version-control.png)
![](git.png)
{{< /spoiler >}}

## Step 3: Create website with {blogdown}

```
install.packages("blogdown") # install the blogdown package
library(blogdown) # load blogdown
new_site(theme = "wowchemy/starter-academic") # create your website!
```

You will now be asked if you want to serve and preview the site locally (before publishing). Type `y` in your Console. 
![](y.jpg)

A preview will show up in your Viewer Pane. You can click on the  {{< icon name="external-link-alt" pack="fas" >}} "Show in new window" icon  next to the 🧹. 

{{< spoiler text="Click to view screenshots for Step 3" >}}
![](new-site.png)
![](viewer-in-new-window.png)
{{< /spoiler >}}

## Step 4: Push to GitHub 

In the console, run the following line of code to create a `.gitignore` file:
```
file.edit("gitignore")
```
Add the following lines to the `.gitignore` file: 
```
.Rproj.user
.Rhistory
.RData
.Ruserdata
.DS_Store
Thumbs.db
```

Before we make our first commit, we use blogdown to check our all our files:
```
blogdown::check_site()
```
This will give you a number of `[TODO]` items, like adding `public` and `resources` to the `.gitignore` file, which you can do safely. Don't worry about content flagged as `draft` or files with a future publish date. 

After running these checks, you're ready to commit to GitHub! 🎉  

{{< spoiler text="Show me how to commit to GitHub" >}}
{{% callout note %}}
- Go to the `Environment` Pane
- Click on `Commit` under `Git`
- Check files to `Stage` them
- Write a commit message 
- `Commit` and then `Push` {{< icon name="arrow-up" pack="fas" >}} 
{{% /callout %}}
{{< /spoiler >}}

## Step 5: Deploy site with Netlify

{{< cta cta_text="Log in to Netlify with GitHub" cta_link="https://github.com/login?client_id=0eef2fa971fd9f7d46a2&return_to=%2Flogin%2Foauth%2Fauthorize%3Fclient_id%3D0eef2fa971fd9f7d46a2%26redirect_uri%3Dhttps%253A%252F%252Fapi.netlify.com%252Fauth%252Fdone%26scope%3Duser%253Aemail%26state%3DeyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJwaWQiOiI1NmMzZjE1MDcxZTIwYTE4ZGIwMDAwMGYiLCJleHAiOjE2MjA4ODA5OTYsImxvZ2luIjp0cnVlfQ.5GilGKggLeOdejJI5_1VXSWmofo5n30SKHSX77VWncQ" cta_new_tab="true" >}} 

After logging in to Netlify through GitHub, you can deploy your website and change the url to your preferred site name, as follows:

- [ ] Select `New site from Git > Continuous Deployment: GitHub`[^1]
- [ ] Select your website repository
- [ ] `Deploy Site`
- [ ] `Settings > Site information > Change site name`

[^1]: Continuous deployment ensures that your website is rebuilt every time you push to GitHub.

Back in RStudio, change the baseurl to your new link in your configuration file: 

```
install.packages("rstudioapi")
library(rstudioapi) # to easily navigate to files
rstudioapi::navigateToFile("config.yaml")
```
In `config.yaml`, set the following:  
```
baseurl: 'http://your-site-name.netlify.app' # use the link you just created
```

Before committing, let's again run
```
blogdown::check_site() # run checks to resolve critical [TODOs] before commit
``` 

Among other things, you need to make sure that the version of Hugo that you are using locally with {blogdown} matches the version used by Netlify (which is specified in `netlify.toml`). You will likely need to change your `netlify.toml` file. Remember that you can easily navigate to this file using 
```
rstudioapi::navigateToFile("netlify.toml") 
```

## Step 6: Customize your site with Wowchemy 🎨

![](themes.png)

It's time to customize! Navigate to `config/_default/params.yaml`
``` 
rstudioapi::navigateToFile("config/_default/params.yaml")
```

Pick a built-in Wowchemy color theme [here](https://wowchemy.com/docs/getting-started/customization/#color-themes). I initially chose the `Rose` theme, by setting `theme: rose` in `config/_default/params.yaml`. However, I decided to further customize the color theme using the steps outlined [here](https://wowchemy.com/docs/getting-started/customization/#community-themes):

- [x] I located the `rose.toml` file[^2]  
- [x] I created a new `data/themes/` folder at the root of my site
- [x] I copied the `rose.toml` file into `data/themes/shilaan_theme.toml` 
- [x] I adjusted the colors as desired using [HTML color codes](https://htmlcolorcodes.com)
- [x] I set `theme: shilaan_theme` in `config/_default/params.yaml`

You can also [customize the font set](https://wowchemy.com/docs/getting-started/customization/#custom-font). 

[^2]: Themes are in `themes/github/com/wowchemy/wowchemy-hugo-modules/wowchemy/data`. In this folder, go to `/fonts` for font sets and to `themes` for color themes. 



Customize your menu by opening the file `config/_default/menus.yaml`
``` 
rstudioapi::navigateToFile("config/_default/menus.yaml")
```

## Acknowledgements 

My workflow, and a large part of the content of this post, is based on [Alison Hill's materials](https://alison.rbind.io/post/new-year-new-blogdown/). In addition, I would not have made this website if it wasn't for Daniël Lakens friendly urging me (about 5 months ago... it took me a while to accept my fate). Thank you, Alison and Daniël! 🙏
