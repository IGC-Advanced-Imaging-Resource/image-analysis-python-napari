---
title: 'Applying Filters'
teaching: 10
exercises: 3
---

:::::::::::::::::::::::::::::::::::::: questions
- How can I make it possible to isolate features in my images?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Apply mean and gaussian filters to an image
- Select one of these images to use for processing in subsequent chapters
- Discuss Difference of Gaussian and Laplacian of Gaussian algorithms
::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Images often need to have noise removed in order for the results of further
processing to be meaningful.

A mean filter will, for each pixel:
  - Create a kernel of pixels around it, in a size/shape of the user's choosing
  - Take the mean of all pixels within this kernel
  - Assign this new value to the pixel

The kernel in this case can be considered a 2-dimensional sliding window.

A gaussian filter is similar to a mean filter, except that pixels further from the
kernel centre will have less effect on the result.

There are many smoothing filters beyond these, they all perform differently and have
their own advantages depending on the situation and the image in question.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 7: Applying filters

Load [some_test_image.png] and build a figure displaying it in the following forms:

- original image
- 5x5 square mean filter applied
- 15x15 square mean filter
- gaussian filter of sigma = 3
- gaussian filter of sigma = 5

How do the different methods compare?

:::::::::::::::::::::::: solution
```python
import matplotlib.pyplot as plt
import skimage
import numpy

img = skimage.io.imread('data/maize-seedlings.tif')[:, :, 0]  # test image

plt.subplot(2, 3, 1)
plt.imshow(img, cmap='gray')

plt.subplot(2, 3, 2)
mean3x3 = skimage.filters.rank.mean(img, skimage.morphology.square(5))
plt.imshow(mean3x3, cmap='gray')

plt.subplot(2, 3, 3)
mean15x15 = skimage.filters.rank.mean(img, skimage.morphology.square(15))
plt.imshow(mean15x15, cmap='gray')

plt.subplot(2, 3, 4)
gauss3 = skimage.filters.gaussian(img, 3)
plt.imshow(gauss3, cmap='gray')

plt.subplot(2, 3, 5)
gauss5 = skimage.filters.gaussian(img, 5)
plt.imshow(gauss5, cmap='gray')
```
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

A rolling ball algorithm is often used in cases where the background is not entirely distinct
from the foreground and need to be subtracted.

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 8: Rolling ball background intensity

Load [some_test_image.png] and display:

- the original image
- image with a rolling ball of radius 100 applied
- image with a rolling ball of radius 50 applied
- the image with the radius=50 rolling ball subtracted

How long does it take to compute?

:::::::::::::::::::::::: solution 

```python
import datetime
import skimage
import matplotlib.pyplot as plt

img = skimage.io.imread('data/maize-seedlings.tif')[:, :, 0]
t0 = datetime.datetime.now()

plt.subplot(1, 3, 1)
plt.imshow(skimage.restoration.rolling_ball(img), cmap='gray')
t1 = datetime.datetime.now()

plt.subplot(1, 3, 2)
rolling_ball = skimage.restoration.rolling_ball(img, radius=50)
plt.imshow(rolling_ball, cmap='gray')
t2 = datetime.datetime.now()

plt.subplot(1, 3, 3)
plt.imshow(img - rolling_ball, cmap='gray')

plt.show()
print(t1 - t0)
print(t2 - t1)
print(t2 - t0)
```
:::::::::::::::::::::::::::::::::

## Exercise 9: Dogs and logs

Load [some_test_image.png] and display:

- the original image
- image with a DoG/Difference of Gaussians (sigma = 2 and sigma = 3.2) applied
- image with a LoG/Laplacian of Gaussians (sigma = 2) applied

:::::::::::::::::::::::: solution 

```python
import matplotlib.pyplot as plt
import skimage.io
import skimage.filters
import numpy

img = skimage.io.imread('data/maize-seedlings.tif')[:, :, 0]  # test image
plt.figure(figsize=(12, 6))
sigma = 2

plt.subplot(2, 3, 1)
plt.imshow(img, cmap='gray')

plt.subplot(2, 3, 2)
gauss1 = (skimage.filters.gaussian(img, sigma) * 255).astype(numpy.uint8) 
gauss2 = (skimage.filters.gaussian(img, sigma*1.6) * 255).astype(numpy.uint8)
dog = (gauss1 - gauss2).astype(numpy.uint8)
plt.imshow(dog, cmap='gray')

plt.subplot(2, 3, 3)
log = (skimage.filters.laplace(skimage.filters.gaussian(img, sigma)) * 255).astype(numpy.uint8)
plt.imshow(log, cmap='gray')
```
:::::::::::::::::::::::::::::::::

## Exercise 10: Choosing a filter

Look at the images you produced in exercises 7 and 9, and select one to use in
subsequent chapters for thresholding and segmentation!
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 
- There are many ways of smoothing an image
- Different methods will perform better in different situations
::::::::::::::::::::::::::::::::::::::::::::::::
