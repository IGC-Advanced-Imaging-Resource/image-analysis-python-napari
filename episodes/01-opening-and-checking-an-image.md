---
title: 'Opening and Checking an Image'
teaching: 20
exercises: 6
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
image = imread('data/FluorescentCells_3channel.tif')
```

If you saved your images to a different location, you will need to change the file path
provided to imread accordingly. Paths will be relative to the location of the .ipynb
notebook file.

To view things in Python, usually we use `print()`. However if we try to print this
image to the Jupyter console, instead of getting the image we get something that may be
unexpected:

```output
> print(image)
array([[[ 16,  50,   0],
        [ 15,  44,   0],
        [ 18,  40,   1],
        ...,
        [  1,  15,   2],
        [  1,  15,   2],
        [  1,  15,   2]]], dtype=uint8)
```

Python has loaded and stored the image as a Numpy `array` object of numbers, and
`print()` displays the string representation or textual form of the data passed
to it, which is why we get a matrix of numbers printed to the screen.

If we want to see what the image looks like, we need tell Python to display it
as an image. We can do this with the `imshow` function from skimage, which can
be imported and called in a similar way to `imread`:

```python
from skimage.io import imshow
imshow(image)
```

You should see the image displayed below the current cell:

![](fig/1_1_imshow.jpg){alt='Displaying an image with imshow'}

Since images are multi-dimensional arrays of numbers, we can apply statistical functions
to them and extract some basic metrics. Numpy arrays have methods for several of these
already, including the image's shape, data type and minimum/maximum values:

```python
print(image.shape, image.dtype, image.min(), image.max())
(512, 512, 3) uint8 0 255
```

This shows that the image has a data type of uint8, it contains values between
0 and 255 and that it is in three dimensions. We can reasonably infer that the two `512` numbers
are the X and Y axes. The third axis in most cases will correspond to the number of channels.

We can select a single channel by **indexing** the array:

```python
imshow(image[:, :, 0])
```

Here, we select the entire X and Y coordinates using `:` with no numbers around them, and
the first channel (remember that Python counts from 0).

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 1: Loading an image

Load the test image 'FluorescentCells_3channel.tif':

- Try the same as the example above, but loading one of the other channels
- Save your single channel to a variable. What happens if you run `imshow` on
  `channel.T`?
- How can we select part of the image, i.e. crop it? Remember that to do this,
  we need to select a subset of the X and/or Y axes.

:::::::::::::::::::::::: solution
Other channels can be loaded with `image[:, :, x]`, where `image` is the variable
the image is saved to and `x` is the index of a channel to retrieve.

Next we can use `.T` to return a **transposed** version of the image. Running
`imshow()` on this results in an image that is flipped 90°:

```python
channel = image[:, :, 1]
imshow(channel.T)
```

![](fig/1_2_transpose.jpg){alt='Transposed image'}

Remember that Numpy arrays can be sliced and indexed the same way as lists, strings and
tuples. Up to this point we've been using `:` to select an entire axis, but we can
give it start and end bounds to select part of the X and Y axes, like:

```python
image[:256, 128:384, 1]
```

![](fig/1_3_2d_slice.jpg){alt='2-dimensional slice'}
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


Numpy arrays have many more methods available for checking them. Here are just a few to
start with:

### Pixel value statistics

- [`image.mean()`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.mean.html)
- [`image.min()`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.min.html)
- [`image.max()`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.max.html)
- [`image.std()`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.std.html)

### Image size

[`image.shape`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.shape.html)

### Object size

[`image.nbytes`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.nbytes.html)

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 2: Memory check

How much memory did it take to load FluorescentCells_3channel.tif?

:::::::::::::::::::::::: solution
Running `image.nbytes` shows that it takes up 786432 bytes, or ~786 kilobytes, or ~0.7 megabytes.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


## Displaying one channel at a time

We've seen from exercise 1 that we can view single channels by indexing
the array. We can show all channels together using a matplotlib figure:

```python
matplotlib.pyplot.figure(figsize=(12, 6))  # figure size, in inches
nchannels = image.shape[-1]

for i in range(nchannels):
    matplotlib.pyplot.subplot(1, nchannels, i+1)  # 1-indexing in subplots
    matplotlib.pyplot.imshow(image[:, :, i])
    matplotlib.pyplot.title('Channel %s' % i)
    matplotlib.pyplot.axis(False)

matplotlib.pyplot.show()
```

![](fig/1_4_all_channels.jpg){alt='All image channels'}


## Histograms

Another common metric to look at in an image is its histogram. This can be plotted by flattening
the image and passing it to matplotlib:

```python
import matplotlib.pyplot
matplotlib.pyplot.hist(image.flatten())
```

We need `flatten()` to condense the pixel values from two dimensions into one, since `hist()`
expects a one-dimensional array of values. `hist()` also takes an argument, `bins`, which
will change how many bins the data is split into.

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 3: Histograms

Combine the usage of `matplotlib.pyplot.hist()` and `matplotlib.pyplot.figure()`
introduced above above and plot a histogram of each of the three channels in
FluorescentCells_3channel.tif.

:::::::::::::::::::::::: solution
Multiple histograms can be plotted with:

```python
matplotlib.pyplot.figure(figsize=(12, 6))  # figsize is width and height, in inches
nchannels = img.shape[-1]

for i in range(nchannels):
    matplotlib.pyplot.subplot(1, nchannels, i+1)  # 1-indexing in subplots
    matplotlib.pyplot.hist(img[:, :, i].flatten(), bins=255)
    matplotlib.pyplot.title('Channel %s' % i)
    matplotlib.pyplot.axis(False)

```
![](fig/1_5_histograms.jpg){alt='All image channels'}

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


## Proprietary formats

Images can come in many formats, but for many of the common ones such as TIFF,
PNG and BMP, skimage is smart enough to know how to read each one.

Some image formats are associated with specific instruments or equipment and
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


::::::::::::::::::::::::::::::::::::: challenge
## Exercise 4: Proprietary formats

Load the [nd2](https://pypi.org/project/nd2) package and use it to read
the test file 'Ersi_organoid_WT2.nd2'. Install it if you need to.

What axes are present in the image? Which look most likely to be the X and
Y axes? Use `imshow` to display a single frame from the image.

:::::::::::::::::::::::: solution
If not already present, the nd2 package can be installed with
`pip install nd2`. According to its documentation and page on PyPI, it has
an `imread` function that works in a similar way to the one in skimage:

```python
image = nd2.imread('data/Ersi_organoid_WT2.nd2')
```

`img.shape` shows that there are four axes, `(27, 3, 512, 512)`. The latter two numbers look like
the X and Y axes, while the second number looks like a number of channels as with the first example
image. We'll get to the first number in a moment. For now, we can show a single frame with:

```python
imshow(image[0, 0, :, :])
```

![](fig/1_6_nd2_image_frame.jpg){alt='ND2 image'}

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Channels, series and stacks

In the exercise above, we encountered an image with four axes. We could guess at what the latter
three are, but the first one could be a couple of things. It could correspond to a timepoint
(i.e. the image is a collection of 27 frames taken at different points in time), or it could
be a Z axis - images with a Z axis are often known as a **Z-stack**.

There is no clear indication from looking directly at the data though, and it's often
necessary to consult documentation or metadata for the image. You may also notice that here the
X/Y axes are the latter two numbers, but in the first example above, the X/Y axes were the first
two. This demonstrates the diversity and general lack of consistency with image files, and it's
necessary to consider carefully the image's format before processing it.

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
print(sorted(matplotlib.colormaps))
```

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 5: Lookup tables

Display each of the three channels of FluorescentCells_3channel.tif side by side in
a matplotlib figure, each in a different colour using `cmap=`.

:::::::::::::::::::::::: solution
There will be many ways to do this (and many colour maps to choose from!), but here
is one possible solution:

```python
matplotlib.pyplot.figure(figsize=(12, 6))
colours = ('Blues', 'Oranges', 'YlOrBr')

matplotlib.pyplot.subplot(1, 3, 1)
imshow(image[:, :, 0], cmap='Blues')

matplotlib.pyplot.subplot(1, 3, 2)
imshow(image[:, :, 1], cmap='Oranges')

matplotlib.pyplot.subplot(1, 3, 3)
imshow(image[:, :, 2], cmap='YlOrBr')
```

![](fig/1_7_colormaps.jpg){alt='ND2 image'}

:::::::::::::::::::::::::::::::::

## Exercise 6: RGB images

Load hela-cells_rgb.tif and try displaying it with different lookup tables. What results do
you get, and why?

:::::::::::::::::::::::: solution 
The image will always look the same regardless of the lookup table used. In this case, this is
because hela-cells_rgb.tif is an RGB image, and the lookup table will be ignored since the
colours are explicitly encoded into the pixel values.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Other notes

### Rearranging channels

You may find yourself in a situation where the arrangement of dimensions in your image
is incorrect for the processing you wish to perform on it - maybe a function requires
that an image be oriented in a particular way. This is where it's useful to be able
to rearrange the dimensions of an image. To do this, we can use Numpy's `moveaxis`
function:

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

We can see that calling `moveaxis` on an array gives us a rearranged version of the
array given to it - the channel axis that was at the end is now at the front. However,
we can see that the original copy of the image is unchanged. This is because by default,
`moveaxis` returns a rearranged **copy** of the image.

The arguments supplied are:

- The image or array
- The current position of the dimension to move
- The position to move that dimension to

In this case, we are moving the dimension at position -1 (i.e. the one at the end) to
position 0 (the start).

### Pixel size

Pixels are an approximation of an object - knowing that something is 50 pixels long and
50 pixels wide doesn't tell us anything about its actual size. To make any real-world
measurements on the image, we need the image's **pixel size**.

To get this, it is necessary to read the image's **metadata**. For this we need a different
library, imageio. There are a couple of different places we can look:

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


::::::::::::::::::::::::::::::::::::: keypoints
- Common image formats can usually all be loaded in the same way
- Specialised proprietary formats may require specialised libraries
- Histograms and basic metrics can tell a great deal about the encoding of the image
- This is often important when considering how to analyse the image
- Lookup tables can change how a single-channel image is rendered
- An RGB image contains 3-channels for red, green and blue
::::::::::::::::::::::::::::::::::::::::::::::::
