---
title: 'Introduction to Napari'
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 
- How can I view images and metadata in Napari?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Open and explore some images and compare the process
  of doing the same process in Python
::::::::::::::::::::::::::::::::::::::::::::::::

## Starting Napari

With Napari installed, run:

    napari

This will start up Napari in a new window. This may take about a minute, especially
if being done for the first time.


## Looking around

In the 'File' menu, go to 'Open Sample -> 'napari builtins -> Cells (3D+2Ch). Looking
at the sidebar, Napari has opened two **layers** - 'membrane' and 'nuclei'. At the top
of the sidebar there are options for displaying layers in different colours and opacity,
and the eye symbol on the layer can be checked or unchecked to show/hide it.

Napari uses layers to represent each stage of processing an image - we run tools to
create new processed layers based off of a previous one. In this example, the image
consists of two channels, each of which has been loaded as a layer.

In the case of z-stack or time series images, there is a slider at the bottom that can
be used to scroll through the layers.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 15: Opening an image

With 'Cells (3D + 2Ch)' open, go to 'Tools -> Filtering / Noise Removal' and
look at the options available. What are these options? What happens if you
select one and try running it on a layer?

:::::::::::::::::::::::: solution 
The options here consist of smoothing filters, several of which will be familiar
from previous chapters. In some cases there are multiple versions of a filter
(e.g. there are three Gaussian filters), each of which uses a different
Python library.

Selecting one of these gives a sidebar letting us choose how to run it, including
which layer to use, plus any extra parameters it may take, such as sigma values
for Gaussian filters.
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::


## Loading real-world images

The example images work nicely with Napari, but sometimes it may be necessary to
coax Napari into loading an image correctly. In 'File', select 'Open File(s)...'
and load up FluorescentCells_3channel.tif. Doing this, we find the results aren't
quite what we expect - the three channels have been loaded like a z-stack.

To load this image correctly, we need to use the Python console. In the bottom
left, use the '>_' button to open a terminal. This activates a Python session
where Napari is represented as an object called `viewer`:

- All the loaded layers are in a list called `viewer.layers`
- Within a layer, the image data is an attribute called `data`

We can also create new layers with `viewer.add_image`:

```python
from skimage.io import imread
img = imread('path/to/FluorescentCells_3channel.tif')
img.shape
(512, 512, 3)
```

From this, we can see the three channels as the third number of the image's shape.
We can now load each channel as a new layer:

```python
viewer.add_image(img[:, :, 0], name='FluorescentCells_ch0', colormap='cyan')
viewer.add_image(img[:, :, 1], name='FluorescentCells_ch1', colormap='yellow', blending='additive')
viewer.add_image(img[:, :, 2], name='FluorescentCells_ch2', colormap='magenta', blending='additive')
```

The `blending='additive'` option prevents layers on top from obscuring layers below it
so that we can view multiple layers at once. We also need to colour the layers so we
can distinguish them from each other.

`view.add_image` is also able to load all these layers in one is we tell it which axis
represents channels:

```python
viewer.add_image(img, name='FluorescentCells', channel_axis=2)
```

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 16: Real world images

- Look at the `shape` of 'confocal-series_zstack.tif' and identify which
  axis looks like the channels
- Use `viewer.add_image` with the `channel_axis` argument to load the image.
  How has the fourth axis been represented?

:::::::::::::::::::::::: solution 
From loading the image in the Python console:

```python
img = skimage.io.imread('Documents/Image analysis/python course images/confocal-series_zstack.tif')
img.shape
(25, 2, 400, 400)
```

The second number, i.e. `img.shape[1]` looks like the channel axis. We can then
provide this when loading the image:

```python
viewer.add_image(img, name='FluorescentCells', channel_axis=1)
```

Napari will load the image as two layers, with the Z axis represented via the slider
at the bottom.
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Wrapup challenge: image segmentation in Napari

These two exercises will put together everything that has been covered in previous
chapters, and apply the data processing performed in Python/Jupyter to
Napari. Finally, we will see how the processing that we have built up in Napari
can be exported back to Python code that we can then run on many images as a script
to help automate our image processing.

::::::::::::::::::::::::::::::::::::: challenge

## Exercise 17: Image segmentation in Napari

Look at the 'nuclei' layer of 'Cells (3D + 2Ch)'. Explore
the options available in 'Tools' and try segmenting and
labelling the nuclei. A few things to think of:

- Although we're working in a different environment from
  Jupyter, the stages of processing will remain the same
- Remember that Napari works with layers, so the results
  of each tool used will depend on which layer it's used on
- Napari names some functions differently. When it's referring
  to thresholding, for example, it uses the term '**binarising**'
- Napari has some tools that combine esveral steps of processing
  into one. Try using the tools to perform each individual
  step

:::::::::::::::::::::::: solution

:::::::::::::::::::::::::::::::::

## Exercise 18: Exporting code

Open the Napari Assistant and find the 'Export Python code'
tool. Use this to generate a Python script that will perform
the processing that you performed in the previous exercise. Is
the script usable immediately without modification?

:::::::::::::::::::::::: solution
There are several options to export in different ways, including to
a Jupyter notebook. The simplest way is probably to copy to clipboard -
this can be pasted into a text editor or IDE.

Looking at it, though, we can see that it's not immediately usable.
Exported scripts do not make any reference to what image file was loaded,
and there is no code at the end for writing the resuults to a file.
Therefore, the script will require some tweaking to parameterise it
and get it to produce tangible output.

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 
- Napari works with layers, each of which represents an image channel
- Napari doesn't always know how to load a multi-channel image from the GUI
- We can use the Python console to perform custom operations that can't be
  done in the GUI
- Most operations we performed earlier in Python can also be done in Napari,
  either graphically or in the terminal
::::::::::::::::::::::::::::::::::::::::::::::::
