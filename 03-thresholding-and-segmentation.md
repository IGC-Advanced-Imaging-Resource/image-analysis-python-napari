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
- Demonstrate how to deal with edge cases like features stuck together
::::::::::::::::::::::::::::::::::::::::::::::::

These exercises will make use of the image you decided to use in exercise 10.

## Thresholding

There are both global and local thresholds in image analysis, but this course will deal
only with global filters. Global thresholds work by determining a single cutoff value that
is then used to create a binary image, where each pixel is either 0 or 1 depending on
whether its original value was above or below the threshold.

It's important to note that these threshold algorithms return a single number. In order to
binarise the image, we can apply a `>` operator to it:

    binary_image = image > threshold


::::::::::::::::::::::::::::::::::::: challenge
## Exercise 11: Thresholds

Look at the [skimage filter docs](https://scikit-image.org/docs/stable/api/skimage.filters.html)
and find each of the following threshold algorithms:

- Otsu threshold
- Triangle threshold

Try applying each of the above filters to your image from exercise 10. Which one
gives better results?

Optional challenge: contrast this with manual thresholding:

- Plot a histogram of your image
- Look at the histogram and use it to select a sensible threshold

:::::::::::::::::::::::: solution 
```python
import matplotlib.pyplot as plt

plt.imshow(img)

# Otsu threshold
plt.subplot(2, 2, 1)
threshold = skimage.filters.threshold_otsu(img)
plt.imshow(img > threshold)

# Triangle threshold
plt.subplot(2, 2, 2)
threshold = skimage.filters.threshold_triangle(img)
plt.imshow(img > threshold)

# Histogram
plt.subplot(2, 2, 3)
plt.hist(img.flatten(), bins=256)

# Manual threshold
plt.subplot(2, 2, 4)
threshold = 80
plt.imshow(img > threshold)
```
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Cleaning up

Once we have a binary image, we may still need to clean it up further. There may
still be extraneous pixels and other bits around the regions of interest, or there
may be holes in a shape that you need to be solid.

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 12: Erosion, dilation, opening and closing

Look at the [skimage morphology docs](https://scikit-image.org/docs/stable/api/skimage.morphology.html).
Select one of your binary images from exercise 11 and apply each of the following algorithms to the image:

  - Binary erosion
  - Binary dilation
  - Binary opening
  - Binary closing

Note: each of the algorithms above will require a kernel or footprint. For this,
use a disc-shaped kernel of radius 4:

    kernel = skimage.morphology.disk(4)

:::::::::::::::::::::::: solution
```python

kernel = skimage.morphology.disk(4)

# Original image
plt.subplot(2, 3, 1)
plt.imshow(binary_image)

# Erosion
plt.subplot(2, 3, 2)
matplotlib.pyplot.imshow(skimage.morphology.binary_erosion(binary_image, footprint=kernel))

# Dilation
plt.subplot(2, 3, 3)
plt.imshow(skimage.morphology.binary_dilation(binary_image, footprint=kernel))

# Opening
plt.subplot(2, 3, 4)
plt.imshow(skimage.morphology.binary_opening(binary_image, footprint=kernel))

# Closing
plt.subplot(2, 3, 5)
plt.imshow(skimage.morphology.binary_closing(binary_image, footprint=kernel))
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

    coords = skimage.feature.peak_local_max(dt, footprint=numpy.ones((7,7)), labels=binary_img)

This gives us a list of coordinates, so we need to convert this back into an
labelled image of maxima. This will be mostly blank with an individually
labelled  dot representing each one:

    mask = numpy.zeros(dt.shape, dtype=bool)
    mask[tuple(coords.T)] = True
    markers = skimage.morphology.label(mask)

Note the `.T` to transpose the maxima coordinates. Finally, the transform can
be done. One final note is that because the [way watershed transforms work](https://bioimagebook.github.io/chapters/2-processing/6-transforms/transforms.html#the-watershed-transform),
we need to convert the distance map from peaks to troughs by inverting it with `-`:

    labels = skimage.segmentation.watershed(-dt, markers, mask=binary_img)


::::::::::::::::::::::::::::::::::::: challenge
## Exercise 13: Image labelling

- Load up your image from exercise 10 and run `skimage.morphology.label`
  on the foreground features
  - Determine the number of features in the image (hint: try flattening
    your labelled image)
- Perform a watershed transform on your binary image as per above. Try
  displaying the result of each stage of the transform.
  - Determine the number of features now after watershed transformation
  - Does the watershed process work perfectly?

:::::::::::::::::::::::: solution

Labelling applies a unique number to each feature. This means that if we apply
thresholding and labelling to the image, we can flatten the result and cast
it to a set to find all the unique label numbers present, and find the highest
numbered label to get the number of features:

```python
threshold = skimage.filters.threshold_triangle(img)
binary_img = img > threshold

plt.subplot(1, 2, 1)
plt.imshow(binary_img)

labels = skimage.morphology.label(img)
plt.subplot(1, 2, 2)
plt.imshow(labels)

flat = labels.flatten()
print(set(flat))
print(max(flat))
```

However, we see from the intermediate images that multiple touching
nuclei are treated as a single feature.

Here is an example of using distance and watershed transforms to segment
touching objects:

```python
plt.subplot(2, 2, 1)
plt.imshow(img)

dt = scipy.ndimage.distance_transform_edt(binary_img)
plt.subplot(2, 2, 2)
plt.imshow(dt)

coords = skimage.feature.peak_local_max(dt, footprint=numpy.ones((7, 7)), labels=binary_img)
mask = numpy.zeros(dt.shape, dtype=bool)
mask[tuple(coords.T)] = True
plt.subplot(2, 2, 3)
plt.imshow(mask)

markers = skimage.morphology.label(mask)
labels = skimage.segmentation.watershed(-dt, markers, mask=binary_img)
plt.subplot(2, 2, 4)
plt.imshow(labels)
print(set(labels.flatten()))
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
