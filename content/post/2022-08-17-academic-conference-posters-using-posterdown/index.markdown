---
title: Academic conference posters using {posterdown}
author: Shilaan Alzahawi
date: '2022-08-17'
slug: academic-conference-posters-using-posterdown
categories: [rmarkdown]
tags: []
subtitle: 'A quick guide to generating reproducible and automatically formatted conference posters in R Markdown'
summary: 'A quick guide to generating reproducible and automatically formatted conference posters in R Markdown using the posterdown package'
authors: 
  - admin
lastmod: '2022-08-17T13:21:49-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---




{{< toc >}} 

## Motivation

If you know me, you know I like using <i class="fab fa-r-project" aria-hidden="true" style="color:#035AA6"></i> for just about anything. When it was time to create my first academic conference poster, I knew I didn't want to waste any time moving between different software environments or copy-pasting and manually formatting text, tables, figures, and results.  

Instead, I wanted to generate a fully reproducible and nicely formatted conference poster using R Markdown. Turns out; the R package [**{posterdown}**](https://github.com/brentthorne/posterdown) makes this quite simple! In this post, I'll briefly show you how to create your own poster using the posterdown package.

For a sneak peek of what you can do with posterdown, you can [**find my first posterdown creation here**](https://sjdm.org/presentations/2021-Poster-Alzahawi-Shilaan-crowds-variability-credibility~.pdf) and the [**underlying code here**](https://github.com/shilaan/Many-Analysts/blob/main/poster/GSPA_Poster.Rmd). 

## Step 1: Install {posterdown}

First, we'll install the posterdown package.

```r
install.packages("posterdown")
```

After you've successfully installed posterdown, restart R. 

## Step 2: Open a posterdown template

After you've installed posterdown and restarted R, you should now be able to find a few new templates in your RStudio. Navigate to   
`File > New File > R Markdown ... > From Template`  
and select any of the posterdown templates. 

{{< spoiler text="Click to view screenshot" >}}
![](navigate.png)
{{< /spoiler >}}

There are three options available: Posterdown HTML, Posterdown Betterland, and Posterdown Betterport. 

![](templates.png)

While the HTML template looks more like a classic scientific poster, the Betterland and Betterport templates create a poster with a large amount of space dedicated to a main take-away message. The difference between the latter two is that Better*land* is *landscape*-oriented, while Better*port* is *portrait*-oriented.  

You can find more about the different templates at the [**posterdown wiki**](https://github.com/brentthorne/posterdown/wiki).   

## Step 3: Personalize ✨

After you've selected a template (I've gone with Posterdown HTML), you can simply 🧶 knit 🧶 it to see what the generated html document looks like. Next, you can personalize the poster to your liking, starting with the `yaml` at the top of the document (i.e., all the metadata specified between `---`, such as `title` and `author`). 

The posterdown package allows for quite a bit of customization. For example, you can set the dimensions of your poster using `poster_height` and `poster_width`; set fonts for the text and title of the poster using `font_family` and `titletext_fontfamily`; and you can set colors using `primary_colour`, `secondary_colour`, and `accent_colour`. For a comprehensive list of the `yaml` options, [**check out the Wiki of your chosen template**](https://github.com/brentthorne/posterdown/wiki/posterdown_html).  

### Icons 

Since we're using R Markdown to create our poster, we can use any R package to further customize it. For example, I really like to use icons, so I used the `icons` package for this. First, I installed and loaded the icons package.

```r
remotes::install_github("mitchelloharawild/icons") #install icons package
library(icons) #load icons package
```

Next, you can write inline code (code between single back ticks) to customize your poster with icons. For example,

````
`r icons::fontawesome("github")`
````

will create an inline github icon:  
<svg viewBox="0 0 496 512" style="height:1em;position:relative;display:inline-block;top:.1em;" xmlns="http://www.w3.org/2000/svg">  <path d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"></path></svg>

If you'd like to further customize your icons, you can wrap the code above with the `icon_style()` function and pass additional comma-separated arguments, such as `scale` and `fill` to set the size and color of the icon.  
For example,    

````
`r icon_style(icons::academicons("osf"), scale = 2, fill = "#035AA6")`
````

will create a larger OSF icon in blue:  
<svg viewBox="0 0 512 512" style="position:relative;display:inline-block;top:.1em;fill:#035AA6;height:2em;" xmlns="http://www.w3.org/2000/svg">  <g label="icon" id="layer6" groupmode="layer">    <path id="path2" d="m 255.9997,7.9999987 c -34.36057,0 -62.21509,27.8545563 -62.21509,62.2151643 0,20.303056 9.87066,38.160947 24.91769,49.517247 0.18814,-20.457899 16.79601,-36.993393 37.29685,-36.993393 20.50082,0 37.11091,16.535494 37.29909,36.993393 15.04533,-11.3563 24.9177,-29.212506 24.9177,-49.517247 C 318.21272,35.854555 290.35915,7.9999987 255.99915,7.9999987 Z M 293.29654,392.2676 c -0.18814,20.4601 -16.79601,36.99338 -37.29684,36.99338 -20.50082,0 -37.10922,-16.53551 -37.29684,-36.99338 -15.04759,11.35627 -24.91769,29.21246 -24.91769,49.51722 0,34.36059 27.85453,62.21518 62.2151,62.21518 34.36056,0 62.21508,-27.85459 62.21508,-62.21518 0,-20.30306 -9.87066,-38.16095 -24.91767,-49.51722 z M 441.78489,193.78484 c -20.30301,0 -38.16309,9.87068 -49.51717,24.91769 20.45786,0.18819 36.99333,16.79605 36.99333,37.29689 0,20.50085 -16.53547,37.11096 -36.9911,37.29916 11.35634,15.04533 29.21249,24.91769 49.51721,24.91769 C 476.14549,318.21327 504,290.35948 504,255.99942 504,221.6394 476.14549,193.78425 441.78489,193.78425 Z M 82.738898,255.99997 c 0,-20.50139 16.535509,-37.11096 36.993392,-37.29689 -11.35632,-15.04756 -29.214164,-24.91769 -49.517197,-24.91769 -34.36057,0 -62.2150945,27.85455 -62.2150945,62.21517 0,34.3606 27.8545245,62.21516 62.2150945,62.21516 20.303033,0 38.160877,-9.87068 49.517197,-24.91773 -20.457883,-0.18818 -36.993391,-16.796 -36.993391,-37.29688 z M 431.3627,80.636814 c -24.29549,-24.295544 -63.68834,-24.295544 -87.9844,0 -14.35704,14.357057 -20.00298,33.963346 -17.39331,52.633806 -0.0824,0.0809 -0.18198,0.13437 -0.26434,0.21491 -14.578,14.57799 -14.578,38.21689 0,52.79488 14.57797,14.57799 38.21681,14.57799 52.79484,0 0.0824,-0.0824 0.13455,-0.18198 0.21732,-0.26434 18.66819,2.60796 38.27445,-3.03799 52.63151,-17.39336 24.29378,-24.29778 24.29378,-63.68837 -0.003,-87.986153 z M 186.2806,378.51178 c 14.57798,-14.57799 14.57798,-38.21461 0,-52.79319 -14.57798,-14.57853 -38.21683,-14.57798 -52.79481,0 -0.0825,0.0824 -0.13448,0.18199 -0.21476,0.26215 -18.67046,-2.60795 -38.276723,3.03572 -52.63376,17.39505 -24.297753,24.29552 -24.297753,63.6884 0,87.98449 24.29551,24.29552 63.68833,24.29552 87.98439,0 14.35702,-14.35703 20.00297,-33.96333 17.39333,-52.63386 0.0848,-0.0786 0.18364,-0.13228 0.26672,-0.21505 z m 0,-245.02583 c -0.0826,-0.0824 -0.18198,-0.13436 -0.26445,-0.21494 2.60795,-18.66823 -3.038,-38.27452 -17.39332,-52.633811 -24.29777,-24.295544 -63.68832,-24.295544 -87.984405,0 -24.297747,24.297781 -24.297747,63.688381 0,87.986151 14.357042,14.35706 33.963315,20.00301 52.631515,17.39336 0.0808,0.0824 0.13447,0.18199 0.21475,0.26434 14.57799,14.57799 38.21684,14.57799 52.79482,0 14.57797,-14.57802 14.57797,-38.21689 0,-52.79488 z m 245.0821,209.89048 c -14.35703,-14.35703 -33.96329,-20.00301 -52.63378,-17.39505 -0.0809,-0.0824 -0.13228,-0.18199 -0.21506,-0.26215 -14.57797,-14.57799 -38.21685,-14.57799 -52.79482,0 -14.57797,14.57799 -14.57797,38.21461 0,52.79316 0.0827,0.0828 0.18198,0.13455 0.26434,0.21505 -2.60796,18.67053 3.03802,38.27683 17.39334,52.63386 24.29552,24.29552 63.68834,24.29552 87.98439,0 24.29775,-24.29552 24.29775,-63.68841 0.003,-87.98451 z" style="stroke-width:0.07717"></path>  </g></svg>

### CSS 

If you're like me, you might want to customize your poster even more. In that case, posterdown allows you to pass a `css` file with further customizations. For example, as you may have noticed from spending more than 5 seconds on my website, [**I really like this color**](https://www.colorhexa.com/035aa6). I wanted all my bold text to show up in this color, so I did the following two things:

#### Create a css file with further customizations

First, I created a `style.css` file in my working directory (i.e., the place where my `.Rmd` is located). In this css file, I specified my preferred color for bold text: 

```css
:root {
  --text-bold-color: #035AA6;
}

strong {
  font-weight: bold;
  color: var(--text-bold-color);
}
```

#### Reference the css file in the `yaml`

After creating my `style.css` file in the working directory, I referenced it in the metadata of my `Rmd`:


```yaml
output: 
  posterdown::posterdown_html:
    self_contained: true
    css: style.css
```

Et voilà! My bolded text is now in [**my favorite shade of blue**](https://www.colorhexa.com/035aa6). 


## Step 4: Share 📡

You will likely want to share your poster with an external audience; to do so, it helps to create a self-contained document by setting `self_contained: true` under `output`. In addition, I knew that I wanted to automatically generate a PDF for sharing, so I added the following line to my `yaml`: 
```yaml
knit: pagedown::chrome_print
```

And that's it! 

![](all.png)

## Acknowledgements 🙏🏼

❤︎ The posterdown package was developed by [**Brent Thorne**](https://twitter.com/wbrentthorne)  
❤︎ The icons package was developed by [**Mitchell O'Hara-Wild**](https://blog.mitchelloharawild.com)



I hope this was helpful! Feel free to [**leave a comment** {{< icon name="twitter" pack="fab" >}}](https://twitter.com/shilaan01/status/1560734307450101760)


