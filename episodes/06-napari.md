---
title: 'Introduction to Napari'
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 
- How do I install Napari?
- How can I view images and metadata in Napari?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Install Napari with pip
- Open and explore some images and compare the process
  of doing the same process in Python
::::::::::::::::::::::::::::::::::::::::::::::::

## Installing Napari

While Napari has developed enormously in recent years, installing it still requires knowledge of
installing Python packages with Pip, and/or Conda.

### Installation with Pip

Pip is Python's package manager. To install Napari into a Python virtual environment:

```
$ python -m venv path/to/new/virtualenv
$ source path/to/new/virtualenv/bin/activate
$ pip install 'napari[all]'
```

### Installation via Mamba

With Mamba installed, run:

```
$ mamba create -p path/to/new/env -c conda-forge napari pyqt
```

### Starting Napari

Once installed, run:

```
$ napari
```

This will start up Napari in a new window. This may take 60secs or more.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 14: Opening an image

- With Napari open, navigate to 'File -> Open Sample' and open the sample image 'napari builtins -> Cells (3D + 2Ch)'
- How many channels are in this image? How can you view each one independently? How many different ways can you
  change how they're viewed?
- How many dimensions are in this image? How can you change how the third dimension is represented?

:::::::::::::::::::::::: solution 
The image has 2 channels, one called 'nuclei' and one called 'membrane', and can be controlled from the
left hand side. The eye icon shows/hides the channel, and the top-left has controls for changing opacity,
alpha and lookup table.

There are 3 dimensions, which can be represented either by:

- A slider along the bottom to view each layer one at a time
- A 3D model view

The 'Toggle ndisplay' cube/square button switches between the two.
:::::::::::::::::::::::::::::::::

## Exercise 15: RGB images

We've seen already that we can view channels independently, but what about RGB images?
Load up sample image 'napari builtins -> Coffee (RGB)'. Do you see any way of viewing
each colour channel independently from right-clicking on the loaded 'coffee' layer?

:::::::::::::::::::::::: solution 
Right-clicking on the layer and select ing 'Split RGB' will split the image into
R, G and B channels.
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Basic metrics

Napari doesn't have built-in support for plotting histograms, but because it's built with
Python and numpy, it is possible to run commands to extract basic metrics.

In the bottom left, use the '>_' button to open a terminal. This activates a Python session
where Napari is represented as an object called `viewer`:

- All the loaded layers are in a list called `viewer.layers`
- Within a layer, the image data is an attribute called `data`

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 16: Basic metrics

- Open sample image 'napari builtins -> Cells (3D + 2Ch)' and look at
  `viewer.layers`. How many layers are currently loaded?
- Take a look at `data` for one of these layers. What kind of object is it?
  How can we check the image's dimensions, data type, mean, max, min and
  standard deviation for its pixel values?

:::::::::::::::::::::::: solution
Taking the first layer as an example, running `type(viewer.layers[0])` shows that
the object is a `numpy.ndarray`, meaning that we can extract basic information
the same way as for Python/Jupyter:

```python
img = viewer.layers[0].data
print(img.dtype)
print(img.shape)
print(img.mean())
print(img.min())
print(img.max())
print(img.std())
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 
- Napari works with layers
- For a multi-channel image, each channel is a layer
- Most operations we performed earlier in Python can also be done in Napari,
  either graphically or in the terminal
::::::::::::::::::::::::::::::::::::::::::::::::
