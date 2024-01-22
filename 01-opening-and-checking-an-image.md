---
title: 'Opening and Checking an Image'
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions 
- How do I open an image file in Python for processing?
- How can I explore an image once it's open?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Open an image with skimage
- Discuss how to open proprietary image formats
- Display an opened image to the screen
::::::::::::::::::::::::::::::::::::::::::::::::

## Opening an image

At its core, an image is a multidimensional array of numbers, and as such can be opened
by programs and libraries designed for working with this kind of data. For Python, one
such library is scikit-image. This library provides a function called `imread` that we
can use to load an image into memory. In a new notebook cell:

```python
from skimage.io import imread
image = imread('image.tif')
```

To view things in Python, usually we `print()` them. However if we try to `print()` this
image to the Jupyter console, instead of getting the image we get something that may be
unexpected:

```output
> print(image)
array([[ 40,  32,  24, ..., 216, 200, 200],
       [ 56,  40,  24, ..., 232, 216, 216],
       [ 64,  48,  24, ..., 240, 232, 232],
       ...,
       [ 72,  80,  80, ...,  48,  48,  48],
       [ 80,  80,  80, ...,  48,  48,  48],
       [ 96,  88,  80, ...,  48,  48,  48]], dtype=uint8)
```

Python has loaded and stored the image as an `array` object of numbers, and `print()`
displays string representations (i.e. the textual form) of the data passed to it,
which is why we get a matrix of numbers printed to the screen.

If we want to see what the image looks like, we need to import the `imshow`
function from skimage, the same as for `imread`. Then we can call it on the
loaded image:

```python
from skimage.io import imshow
imshow(image)
```

You should be able to see the image displayed below the current cell.

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 1: Loading an image

- Try loading one of the test images. What does it look like when you try printing
  it to the console?
- Try running `imshow` on the image you've just loaded.
::::::::::::::::::::::::::::::::::::::::::::::::

Images can come in many formats, but for many of the common ones such as TIFF,
PNG and BMP, skimage is smart enough to know from the file name how to read each
one.

## Proprietary formats

Some image formats are associated with specific instruments equipment and
require specialised packages to open. Depending on your system, these may
already be available via `import` the same as any other Python package. If
not, then these should be installed into whatever Python instance you are
using.

If using JupyterHub or JupyterLab, go to 'New' -> 'Terminal'. This will open
a shell session in a new browser tab, where you can run `pip install` commands.

### Carl Zeiss .czi

Images in .czi format can be opened with the `czifile` library. In the Terminal
you opened above:

    $ pip install czifile

### Nikon .nd2

    $ pip install nd2

### Imaris .ims

    $ pip install imaris-ims-file-reader

### Leica .lif

    $ pip install readlif


## Altering the lookup table

Now that we've been able to open an image, we can start to explore it and display
it in different ways.

The `imshow()` function can take extra arguments in addition to the image to display. One
of these is called `cmap`, standing for 'colour map', which can apply alternate lookup tables:

```python
imshow(image, cmap='gray')
```

Skimage uses lookup tables from the plotting library matplotlib. A list of available tables
can be obtained with:

```python
from matplotlib import colormaps
print(sorted(colormaps))
```

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 2: Lookup tables

Try displaying your loaded image with a different lookup table passed in via `cmap=`. What happens
if you append '_r' to the lookup table name?

:::::::::::::::::::::::: solution 
```output
$ mkdir test_data
$ cd test_data
$ wget <test_data>
```
:::::::::::::::::::::::::::::::::

## Exercise 3: RGB images

Load [rgb_image.png] and try displaying it with different lookup tables. What results do
you get, and why?

:::::::::::::::::::::::: solution 
The image will always look the same regardless of the lookup table used. In this case, this is
because [rgb_image.png] is an RGB image, and the lookup table will be ignored since the colours
are explicitly encoded into the pixel values.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Channels

Just because we can't see what the image looks like from looking at an image's raw numpy array
doesn't mean that we can't do anything useful with it. For example, we can determine the image's
number of colour channels, z-slices and timepoints from looking at the shape of the array.

Numpy arrays have an attribute called `shape`, which we can call to determine how many dimensions
it has, as well as the size of each dimension.

Load up 'data/trial-020.jpg' and call `shape` on it:

```python
image = imread('data/trial-020.jpg')
print(image.shape)
(2000, 3008)
```

This gives us a tuple showing that the image has two dimensions, one of 2000 pixels and one
of 3008.

Let's try another one:

```python
image = imread('data/maize-seedlings.tif')
print(image.shape)
(800, 800, 3)
```

This image has 3 dimensions. The first two we can reasonably infer to be width and height. Beyond
this, however, it's less obvious as to which dimensions are which. In this case, the third dimension
is unlikely to be depth/z-slices, as we would expect more than 3. However, a dimension of length 3
is characteristic of an RGB image, i.e. each X/Y coordinate has 3 values for red, green and blue.

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 4: Dimensions

Load up each of these test images and look at their shapes:

- test_1.png
- test_2.png
...

What dimensions might these be?

:::::::::::::::::::::::: solution 
The images in this exercise are, in order:

- a 2D image with one colour channel
- a 2D image with two colour channels
- a 2D RGB image
- a z-stacked image
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

### Rearranging channels

You may find yourself in a situation where the arrangement of dimensions in your image
is incorrect for the processing you wish to perform on it. Maybe a function requires
that an image be oriented in a particular way. This is where it's useful to be able
to rearrange the dimensions of an image. To do this, we're going to use numpy's
`moveaxis` function:

```python
import numpy
image = imread('data/maize-seedlings.tif')
print(image.shape)
(800, 800, 3)
rearranged_image = numpy.moveaxis(image, -1, 0)
print(rearranged_image.shape)
(3, 800, 800)
print(image.shape)
(800, 800, 3)
```

`moveaxis` returns a rearranged copy of the image given to it, i.e. the original
image is not altered. The arguments supplied are:

- The image data
- The current position of the dimension to move
- The position to move that dimension to

In this case, we are moving the dimension at position -1 (i.e. the one at the end) to
position 0 (the start).

### Displaying one channel at a time

If the image contains multiple channels, it may be because it's a composite image, e.g. of
cells that have been stained for different organelles in different colours, and it may be
useful to display each one individually. To do this, it's possible to use a matplotlib figure:

```python
matplotlib.pyplot.figure(figsize=(12, 6))  # figure size, in inches
nchannels = img.shape[-1]

for i in range(nchannels):
    matplotlib.pyplot.subplot(1, nchannels, i+1)  # 1-indexing in subplots
    matplotlib.pyplot.imshow(img[:, :, i])
    matplotlib.pyplot.title('Channel %s' % i)
    matplotlib.pyplot.axis(False)

matplotlib.pyplot.show()
```
## Histograms

One common metric to look at in an image is its histogram. This can be plotted by flattening
the image and passing it to matplotlib:

```python
import matplotlib.pyplot
matplotlib.pyplot.hist(image.flatten())
```

We need `flatten()` to condense the pixel values from two dimensions into one, since `hist()`
expects a one-dimensional array of values. `hist()` also takes an argument, `bins`, which
will change how many bins the data is split into. We will explore this more in the exercise below.

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 5: Histograms

- Load up [some_multi_channel_image.png], select one channel and plot a histogram of its values
- Try plotting the same histogram with a greater number of bins. How does this affect your results?
- What does your histogram look like if you don't `flatten()` the image first?

:::::::::::::::::::::::: solution 
The first histogram can be plotted as per above:

```python
matplotlib.pyplot.hist(image.flatten())
```

Plotting with a greater `bins` value will reveal detail in the histogram that
may be missed otherwise:

```python
matplotlib.pyplot.hist(image.flatten(), bins=50)
```

If the image is not passed through `flatten()` first, the histogram from matplotlib will appear
as a distinctive series of multi-coloured peaks.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


### Multiple histograms on a multi-channel image

Dealing with multiple channels, it makes sense to build a single **figure** displaying the
histograms together:

```python
matplotlib.pyplot.figure(figsize=(12, 6))
nchannels = img.shape[-1]

for i in range(nchannels):
    matplotlib.pyplot.subplot(1, nchannels, i+1)  # 1-indexing in subplots
    matplotlib.pyplot.hist(img[:, :, i].flatten(), bins=50)
    matplotlib.pyplot.title('Channel %s' % i)
    matplotlib.pyplot.axis(False)

matplotlib.pyplot.show()
```

## Other QC

Singe images are multi-dimensional arrays of numbers, we can apply statistical functions
to them and extract some basic metrics:


### Pixel value statistics

- [`image.mean()`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.mean.html)
- [`image.min()`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.min.html)
- [`image.max()`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.max.html)
- [`image.std()`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.std.html)

### Image size

[`image.shape`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.shape.html)

### Object size

[`image.nbytes`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.nbytes.html)

### Pixel size

To read this, it is necessary to read the image's **metadata**. For this, we need a different
library - imageio. There are a couple of different places we can look:

    m = imageio.v3.immeta('path/to/image.png')
    p = imageio.v3.improps('path/to/image.png')

`immeta()` gives us a dict including the key 'unit', and `improps` gives an object with the
property 'spacing'. However Peter Bankhead notes that
[these numbers can be misleading](https://bioimagebook.github.io/chapters/1-concepts/5-pixel_size/python.html#imageio)
and require interpretation and cross-checking.

The unit may be returned as escaped Unicode:

    'unit': '\\u00B5m'

This can be un-escaped with:

    >>> m['unit'].encode().decode('unicode-escape')
    μm


::::::::::::::::::::::::::::::::::::: challenge
## Exercise 6: Basic metrics

- Load up [some_multi_channel_image.png]

- How much memory does it take up, in megabytes?

:::::::::::::::::::::::: solution 
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints
- Common image formats can usually all be loaded in the same way
- Specialised proprietary formats may require specialised libraries
- Lookup tables can change how a single-channel image is rendered
- An RGB image contains 3-channels for red, green and blue
- Histograms and basic metrics can tell a great deal about the encoding of the image
::::::::::::::::::::::::::::::::::::::::::::::::
