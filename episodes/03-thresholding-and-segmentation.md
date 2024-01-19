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

These exercises will make use of the image you decided to use in exercise 10.

## Thresholding

There are both global and local thresholds in image analysis, but this course will deal
only with global filters. Global filters work by determining a single cutoff value tha
is then used to create a binary image, where each pixel is either 0 or 1 depending on
whether its original value was above or below the threshold.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 11: Thresholds

- Look at the [skimage filter docs](https://scikit-image.org/docs/stable/api/skimage.filters.html)
- Find each of the following threshold algorithms:
  - Otsu threshold
  - Triangle threshold
- Try applying each of the above filters to your image from exercse 10

- Optional challenge: contrast this with manual thresholding:
  - Plot a histogram of your image
  - Look at the histogram and use it to select a sensible threshold
:::::::::::::::::::::::: solution 
```python
import matplotlib.pyplot
import skimage

img = skimage.io.imread('data/maize-seedlings.tif')[:, :, 0]

# Otsu threshold
matplotlib.pyplot.subplot(2, 2, 1)
threshold = skimage.filters.threshold_otsu(img)
matplotlib.pyplot.imshow(img > threshold, cmap='gray')

# Triangle threshold
matplotlib.pyplot.subplot(2, 2, 2)
threshold = skimage.filters.threshold_triangle(img)
matplotlib.pyplot.imshow(img > threshold, cmap='gray')

# Histogram
matplotlib.pyplot.subplot(2, 2, 3)
matplotlib.pyplot.hist(img.flatten(), bins=256)

# Manual threshold
matplotlib.pyplot.subplot(2, 2, 4)
threshold = 80
matplotlib.pyplot.imshow(img > threshold, cmap='gray')
```
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Cleaning up

Once we have a binary image, we may still need to clean it up further. There may
still be extraneous pixels and other bits around the regions of interest, or there
may be holes in a shape that you need to be solid.

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 12: Erosion, dilation, opening and closing

- Look at the [skimage morphology docs](https://scikit-image.org/docs/stable/api/skimage.morphology.html)
- Select one of your binary images from exercise 11
- Apply each of the following algorithms to the image:
  - Binary erosion
  - Binary dilation
  - Binary opening
  - Binary closing

Note: each of the algorithms above will require a kernel or footprint. For this,
use a disc-shaped kernel of radius 4:

    kernel = skimage.morphology.disk(4)

:::::::::::::::::::::::: solution
```python
matplotlib.pyplot.figure(figsize=(12, 6))
kernel = skimage.morphology.disk(4)

# Original image
matplotlib.pyplot.subplot(2, 3, 1)
matplotlib.pyplot.imshow(binary_image)

# Erosion
matplotlib.pyplot.subplot(2, 3, 2)
matplotlib.pyplot.imshow(skimage.morphology.binary_erosion(binary_image, footprint=kernel))

# Dilation
matplotlib.pyplot.subplot(2, 3, 3)
matplotlib.pyplot.imshow(skimage.morphology.binary_dilation(binary_image, footprint=kernel))

# Opening
matplotlib.pyplot.subplot(2, 3, 4)
matplotlib.pyplot.imshow(skimage.morphology.binary_opening(binary_image, ootprint=kernel), cmap='gray')

# Closing
matplotlib.pyplot.subplot(2, 3, 5)
matplotlib.pyplot.imshow(skimage.morphology.binary_closing(binary_image, ootprint=kernel), cmap='gray')
```
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Labelling

In some situations, it may be necessary to distinguish between individual
features, e.g. if counting cell nuclei. For this, it is necessary to
isolate or **label** individual separated objects in the foreground:

    skimage.morphology.label(binary_img)

This will result in an image similar to the binary image supplied, but
instead of just 1s, each feature will have a unique number assigned to
all of its pixels.

## Watershed transforms

Sometimes, isolating objects is more complicated, for example when cells are stuck
together. To solve this, several transforming or segmentation operations exist:

This transformation requires a few preparation steps. First, we need a
**distance transform** of the binary image, where each foreground pixel's value
is its Euclidean distance to the background. skimage doesn't have a function
for this, but scipy does:

    dt = scipy.ndimage.distance_transform_edt(binary_img)

Next, we need the coordinates of all **local maxima**, i.e. locations that are
furthest away from the background. This requires a kernel/footprint, which in
this example is a square kernel of 7x7:

    coords = skimage.feature.peak_local_max(dt, footprint=np.ones((7,7)), labels=binary_img)

This gives us a list of coordinates, so we need to convert this back into an
labelled image of maxima. This will be mostly blank with an individually
labelled  dot representing each one:

    mask = np.zeros(distance.shape, dtype=bool)
    mask[tuple(coords.T)] = True
    markers = skimage.morphology.label(mask)

Note the `.T` to transpose the maxima coordinates. Finally, the transform can
be done. One final note is that because the [way watershed transforms work](https://bioimagebook.github.io/chapters/2-processing/6-transforms/transforms.html#the-watershed-transform),
we need to convert the distance map from peaks to troughs by inverting it with `-`:

    labels = skimage.segmentation.watershed(-distance, markers, mask=binary)


::::::::::::::::::::::::::::::::::::: challenge
## Exercise 13: Image labelling

- Load up your image from exercise 10 and label the foreground features
- Determine the number of features in the image (hint: try flattening
  the array first)
- Perform a watershed transform on your binary image as per above. Try
  displaying the result of each stage of the transform.
- What happens if you leave out the `.T` when constructing the mask?

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
:::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: keypoints 

- Thresholding, labelling and feature isolation are all forms of segmentation
- Different thresholding algorithms can perform differently in different situations
- Quantitative image analysis often requires us to individually label features
- Processing such as watershed transforms can solve cases where features of interest
  are very close or stuck together
::::::::::::::::::::::::::::::::::::::::::::::::
