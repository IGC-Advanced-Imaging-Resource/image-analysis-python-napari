---
title: '07-napari-filters'
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 
- How do I filter an image in Napari? 
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Apply filters to images using the Tools menu
::::::::::::::::::::::::::::::::::::::::::::::::

o Apply best filter from python day1, threshold, create label image

· Measurements

§ Make a plot of favourite measurement

· Export code e.g. jupyter notebook (adapting), make figures


::::::::::::::::::::::::::::::::::::: challenge 

## Exercise 17: Filters

- Load sample image 'napari builtins -> Coins'
- With the layer selected, go to the 'Tools' menu and apply a Gaussian filter
  with sigma = 2
  - (Optional) Try some different filters, e.g. a mean filter with a 3x3 square kernel

:::::::::::::::::::::::: solution
Under 'Filtering / noise removal' there are three options for Gaussian filters,
based on different Python libraries. Selecting one of these will open a side window
where sigma values for X, Y and Z axes can be altered (depending on the option selected,
it may be called 'variance').

An option for mean filtering 
:::::::::::::::::::::::::::::::::

## Exercise 18: Rolling ball background subtraction

Select the original 'coins' layer and perform a rolling ball background
removal from the 'Tools' menu, with a ball radius of 40.

:::::::::::::::::::::::: solution 
Rolling ball background removal is available in 'Tools -> Filtering / background removal'
:::::::::::::::::::::::::::::::::

## Exercise 19: Dogs and logs

Run the following processing on the 'coins' layer:

- Difference of Gaussian (sigma 1 = 2, sigma 2 = 3.2)
- Laplacian of Gaussian (sigma = 2)

:::::::::::::::::::::::: solution 
Difference of Gaussian is under 'Tools -> Filtering', and Laplacian
of Gaussian is in 'Filtering / edge enhancement'
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 
- Various different filters and processing can be applied in the Tools menu
::::::::::::::::::::::::::::::::::::::::::::::::
