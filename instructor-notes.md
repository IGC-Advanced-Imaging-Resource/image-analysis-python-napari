---
title: Instructor Notes
---

# Building the course

Set up R:

conda create -p path/to/conda -c conda-forge r-base pandoc

Install sandpaper as per the [docs](https://carpentries.github.io/sandpaper-docs):

```r
install.packages(c('sandpaper', 'varnish', 'pegboard'), c("https://carpentries.r-universe.dev/", getOption("repos")))
```

To build and serve interactively:

```r
library('sandpaper')
sandpaper::serve()
```
