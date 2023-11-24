---
title: 'Thresholding and Segmentation'
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions 
- How can I isolate features of interest in my image?
- How can I uniquely label features once I've found them?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Apply various thresholds an image
- Quantify features of interest in an image
- Demonstrate ho to deal with edge cases like features stuck together
::::::::::::::::::::::::::::::::::::::::::::::::

These exercises will make use of the image you decided to use in the previous chapter. There
are multiple ways of applying global thresholds to an image, but they all work in the same
way. They all determine a single cutoff value that is then used to create a binary image,
where each pixel is either 0 or 1, depending on whether its original value was above or below
the threshold.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 9: Thresholds

Try applying the following thresholds to your image:

- Histogram (i.e. plot a histogram of your image and use it to select a threshold)
- Otsu threshold
- Triangle threshold

How do they compare?

:::::::::::::::::::::::: solution 
```python
import matplotlib.pyplot
import skimage

img = skimage.io.imread('data/maize-seedlings.tif')[:, :, 0]

matplotlib.pyplot.subplot(2, 2, 1)
matplotlib.pyplot.hist(img.flatten(), bins=256)

matplotlib.pyplot.subplot(2, 2, 2)
threshold = 80
matplotlib.pyplot.imshow(img > threshold, cmap='gray')

matplotlib.pyplot.subplot(2, 2, 3)
threshold = skimage.filters.threshold_otsu(img)
matplotlib.pyplot.imshow(img > threshold, cmap='gray')

matplotlib.pyplot.subplot(2, 2, 4)
threshold = skimage.filters.threshold_triangle(img)
matplotlib.pyplot.imshow(img > threshold, cmap='gray')
```
:::::::::::::::::::::::::::::::::

## Exercise 10: Erosion and dilation

Select one of your binary images from exercise 9 and display:

- the original binary image
- the image after an erosion with a disc kernel of radius 4
- the image after a dilation with a disc kernel of radius 4

What do you get if you erode the image and then dilate it again?

:::::::::::::::::::::::: solution
```python
matplotlib.pyplot.figure(figsize=(12, 6))
kernel = skimage.morphology.disk(4)

matplotlib.pyplot.subplot(1, 3, 1)
matplotlib.pyplot.imshow(binary_image)

matplotlib.pyplot.subplot(1, 3, 2)
matplotlib.pyplot.imshow(skimage.morphology.binary_erosion(binary_image, footprint=kernel))

matplotlib.pyplot.subplot(1, 3, 3)
matplotlib.pyplot.imshow(skimage.morphology.binary_dilation(binary_image, footprint=kernel))
```
:::::::::::::::::::::::::::::::::

## Exercise 11: Opening

Select one of your binary images from exercise 9 and try displaying:

- the original image
- the image after an open (i.e. erode and then dilate it) with a disc kernel of radius 4
- the image after an area opening with an area threshold of 100 pixels

How do the results compare?

:::::::::::::::::::::::: solution
```python
import matplotlib.pyplot
import skimage

img = skimage.io.imread('data/maize-seedlings.tif')[:, :, 0]

threshold = skimage.filters.threshold_triangle(img)
img = img > threshold
disc = skimage.morphology.disk(4)

matplotlib.pyplot.subplot(1, 3, 1)
matplotlib.pyplot.imshow(img, cmap='gray')

matplotlib.pyplot.subplot(1, 3, 2)
matplotlib.pyplot.imshow(skimage.morphology.opening(img, disc), cmap='gray')

matplotlib.pyplot.subplot(1, 3, 3)
matplotlib.pyplot.imshow(skimage.morphology.area_opening(img, 100), cmap='gray')
```
:::::::::::::::::::::::::::::::::

## Exercise 12: Image labelling

Load up [some_image.png] and perform the following processing on it:

- gaussian filter with sigma = 2
- triangle threshold
- feature labelling with skimage

How many colonies do you get?

:::::::::::::::::::::::: solution
```python
import matplotlib.pyplot
import skimage

img = skimage.io.imread('data/colonies-01.tif')[:, :, 0]
img = 255 - img
threshold = skimage.filters.threshold_triangle(img)
img = img > 200

matplotlib.pyplot.subplot(1, 2, 1)
matplotlib.pyplot.imshow(img, cmap='gray')

matplotlib.pyplot.subplot(1, 2, 2)

labels = skimage.morphology.label(img)
matplotlib.pyplot.imshow(labels)
print(set(labels.flatten()))
```
:::::::::::::::::::::::::::::::::

## Exercise 13: Connected components

Load up [some_image.png] and perform a watershed transform on it:

- Otsu threshold on the original image
- Euclidean distance transform on the binary
- Local peaks on the distance map with a square 7x7 kernel
- Inverse of the distance transform
- Use the coordinates of the peaks to construct a mask (it should be same size as the image, with two dots on it indicating the peak positions)
- Label the mask
- Watershed transform on the inverted distance map, the labelled mask and the binary image

:::::::::::::::::::::::: solution
```python
import skimage
import matplotlib.pyplot as plt
import numpy as np

plt.figure(figsize=(12, 6))

img = skimage.io.imread('test.tiff')[500:700, 900:1100, 1]
threshold = skimage.filters.threshold_otsu(img)

binary = img > threshold
plt.subplot(2, 3, 1)
plt.imshow(binary, cmap='gray')

distance = scipy.ndimage.distance_transform_edt(binary)
plt.subplot(2, 3, 2)
plt.imshow(distance, cmap='gray')

plt.subplot(2, 3, 3)
plt.imshow(-distance, cmap='gray')

coords = skimage.feature.peak_local_max(distance, footprint=np.ones((7,7)), labels=binary)
print(coords)
print(coords.T)

mask = np.zeros(distance.shape, dtype=bool)
mask[tuple(coords.T)] = True

plt.subplot(2, 3, 4)
plt.imshow(mask, cmap='gray')

markers = skimage.morphology.label(mask)
labels = skimage.segmentation.watershed(-distance, markers, mask=binary)

plt.subplot(2, 3, 5)
plt.imshow(markers, cmap='gray')

plt.subplot(2, 3, 6)
plt.imshow(labels, cmap='gray')
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 

- Thresholding is a form of segmentation
- Different thresholding algorithms can perform differently in different situations
- Processing such as watershed transforms can solve cases where features of interest
  are very close or stuck together
::::::::::::::::::::::::::::::::::::::::::::::::

