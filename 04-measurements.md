---
title: 'Measurements'
teaching: 10
exercises: 1
---

:::::::::::::::::::::::::::::::::::::: questions 
- How can I use image segmentation to make useful measurements in my image?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Find the mean pixel intensity in each nucleus
- Find each nucleus' area
::::::::::::::::::::::::::::::::::::::::::::::::

Having isolated individual nuclei in the example image in the previous chapter, we
can make some observations about the image features. The exercises here will
require:

- the result of the watershed from the previous chapter
- the original binary image from exercise 10

### Masked watershed

The watershed transform has helped us pick apart any nuclei that are close
together, but is not very informative about their morphology. Fortunately,
we already have this in the form of the original binary image we used to
create the watershed. We can use this as a **mask** on top of the watershed
image to create an image containing information from both:

    masked_watershed = watershed_transform * binary_img

Here we effectively multiply the watershed by the binary image. Since the
binary image's values are either 0 or 1, the watershed's pixels will either
be untouched (multiplied by 1 if a pixel is in the foreground) or set to 0
(multiplied by 0 if a pixel is in the background).

### Processing one nucleus at a time

Remember that each nucleus is labelled with a unique number, so we can select a
single nucleus by applying masking to the masked watershed image a second time.
We can construct a binary mask containing just the feature we want, but numpy
has a [useful function](https://numpy.org/doc/stable/reference/generated/numpy.where.html)
for this:

    nucleus_1 = numpy.where(masked_watershed == 1, 1, 0)
    nucleus_2 = numpy.where(masked_watershed == 2, 1, 0)

Here, each pixel will be set to either 1 or 0 depending on whether its value
matches the label we're looking for.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 14: Quantifying intensities

For each labelled nucleus:

- Construct a binary mask matching the label number
- Use this to isolate the individual nucleus out of the **original image**
  - Hint: try looking at the `numpy.where` docs to see the different ways
    it can be used.
- Find the mean pixel value for the nucleus

:::::::::::::::::::::::: solution
```python
import numpy

for i in range(1, max(masked_watershed.flatten()) + 1):
    mask = numpy.where(masked_watershed == i, 1, 0)
    nucleus = numpy.where(mask == 1, original_img, None)
    pixels = [p for p in nucleus.flatten() if p is not None]
    print('Pixel %i intensity: %f' % (i, numpy.mean(pixels)))
```
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 
- Binary multiplication and `numpy.where` are powerful ways of combining images and masks
- The numbers assigned during labelling can be used to select and process one feature at a time
::::::::::::::::::::::::::::::::::::::::::::::::
