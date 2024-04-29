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

- Your masked watershed transformation from the previous chapter
- Your original image from exercise 10

### Processing one nucleus at a time

Remember that each nucleus is labelled with a unique number, so we can select a
single nucleus with masking. We can construct a binary mask from a labelled image
using numpy's [where function](https://numpy.org/doc/stable/reference/generated/numpy.where.html):

    nucleus_1 = numpy.where(masked_watershed == 1, 1, 0)
    nucleus_2 = numpy.where(masked_watershed == 2, 1, 0)

Here, each pixel will be set to either 1 or 0 depending on whether its value
matches the label we're looking for.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 14: Quantifying intensities

For each labelled nucleus:

- Construct a binary mask matching the label number
- Use this to isolate the individual nucleus out of the **original image**
  - Hint: try looking at the [`numpy.where`](https://numpy.org/doc/stable/reference/generated/numpy.where.html)
    docs to see the different ways it can be used.
- Find the mean pixel value for the nucleus

:::::::::::::::::::::::: solution
First, we need to know how many features we're looping over. We can get that from the
numbers that the labelling process provides - if your labelled image contains the
labels 1, 2 and 3, then you have 3 features.

Next, we need to generate a mask for that feature using `numpy.where`, that will
allow us to mask out the original image - in this case, either a corresponding
pixel value from the original image or `None`. We can then take the result of that,
select all pixels that are not `None` and use `numpy.mean`.

```python
plt.figure(figsize=(12, 16))
for i in range(1, max(masked_watershed.flatten()) + 1):
    _mask = numpy.where(masked_wt == i, 1, 0)
    _nucleus = numpy.where(_mask == 1, img, 0)
    plt.subplot(1, 3, i)
    plt.imshow(_nucleus, cmap='gray')
    pixels = [p for p in _nucleus.flatten() if p is not None]
    print('Feature %i:' % i, numpy.mean(pixels))
```

![](fig/4_1_mean_intensities.png){alt='Mean intensities'}

A few notes:

- The arguments to `range` are offset by +1, since we want to count up from 1, not 0,
  i.e. select only foreground objects - since 0 will be the background.
- In the first `numpy.where` above, we supply two single numbers, 1 and 0. The function
  can however take an n-dimensional array instead, which is what we do in the second one.
  In this case, it will select the value of the corresponding pixel.
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 
- Binary multiplication and `numpy.where` are powerful ways of combining images and masks
- The numbers assigned during labelling can be used to select and process one feature at a time
::::::::::::::::::::::::::::::::::::::::::::::::
