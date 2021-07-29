---
title: Writing reproducible manuscripts in R
author: Shilaan Alzahawi
date: '2021-07-11'
slug: writing-reproducible-manuscripts-in-r
categories: 
  - rmarkdown
tags: []
subtitle: ''
summary: 'A workshop on writing reproducible APA manuscripts using R Markdown and papaja'
authors:
    - admin
lastmod: '2021-07-11T10:34:17-07:00'
featured: no
image:
  placement: 2
  caption: ''
  focal_point: 'Center'
  preview_only: no
projects: []
---

{{< toc >}} 

## Motivation

I recently gave a talk on writing reproducible manuscripts using R Markdown and the R package [*papaja*](https://github.com/crsh/papaja) (*p*reparing *apa j*ournal *a*rticles). One of my main motivations for using R to write up my manuscripts is contained in this tweet by [Pamela Jacobsen](https://twitter.com/pamelacjacobsen?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1394207042462785539%7Ctwgr%5E%7Ctwcon%5Es1_&ref_url=http%3A%2F%2Flocalhost%3A4321%2Fpost%2Fwriting-reproducible-manuscripts-in-r%2F): 

<center>

{{< tweet 1394207042462785539 >}}

</center>

  
### A reproducible workflow 

In the workshop, I go through methods that keep you from manually transcribing and/or copy pasting tables, figures, statistical output, and citations over and over again.   

Perhaps my favorite introduction to the idea of a [**reproducible workflow**](https://www.youtube.com/watch?v=s3JldKoA0zw) (and the horrors of a non-reproducible workflow) is given in this short YouTube video: 

<center>

{{< youtube s3JldKoA0zw >}}

</center>

#### Benefits

The video hints at some of the benefits of a reproducible workflow. A reproducible workflow is...  

⚠️  Less error-prone  
⏳  Less time-consuming  
⇄   More dynamic  
🔎  More transparent  

### The result

You can find a list of papers written with papaja [here](https://github.com/crsh/papaja#papers-written-with-papaja). Here's a sneak peek of what's happening behind the scenes, and the rendered result:  
![](manuscript.png)
![](cites.png)




## Materials


If you want to learn more, you can [find the slides here {{< icon name="images" pack="far" >}}](https://shilaan-apa.netlify.app).  

If you have trouble loading the slides, you can browse through (and save) a static version of the slides below.   

💡 Pro tip: if you are on a mobile device, click on `75%` in the toolbar to adjust the size of the slides to `Zoom to: Page Width`.

<iframe src="https://onedrive.live.com/embed?cid=9D05BA28CB0393F3&resid=9D05BA28CB0393F3%21711&authkey=APVG1vgy5PxC238&em=2" width="100%" height="540" frameborder="0" scrolling="no"></iframe>

If you'd like an example manuscript to work with, I've uploaded some materials to [this OSF folder](https://osf.io/a8gqn/?view_only=a3ea3f76f667482bbc5a2d8a8ddc2aaf).  

Feel free to [{{< icon name="twitter" pack="fab" >}}leave a comment or a question](https://twitter.com/shilaan01/status/1413946789699325953)

### Acknowledgements

❤︎ Slides created with the R package [xaringan](https://github.com/yihui/xaringan)   
❤︎ papaja created by [Frederik Aust](http://frederikaust.com)  
❤︎ GIFs created by [Shannon Pileggi](https://www.pipinghotdata.com/posts/2020-09-07-introducing-the-rstudio-ide-and-r-markdown/)  
❤︎ Artwork created by [Allison Horst](https://github.com/allisonhorst/stats-illustrations)  
