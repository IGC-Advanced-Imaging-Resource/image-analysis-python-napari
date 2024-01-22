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

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 15: Opening an image

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
:::::::::::::::::::::::::::::::::::::::::::::::

Napari works on the principle of **layers**. When a multi-channel image
is loaded, each colour channel becomes a separate layer. The result of
any operations performed on a layer will be saved as a new layer.


## Basic metrics

Napari doesn't have built-in support for plotting histograms, but because it's built with
Python and numpy, it is possible to run commands to extract basic metrics.

In the bottom left, use the '>_' button to open a terminal. This activates a Python session
where Napari is represented as an object called `viewer`:

- All the loaded layers are in a list called `viewer.layers`
- Within a layer, the image data is an attribute called `data`

::::::::::::::::::::::::::::::::::::: challenge
## Exercise 16: Basic metrics

- With sample image 'napari builtins -> Cells (3D + 2Ch)' loaded, open a terminal
  and look at `viewer.layers`. How many layers are currently loaded?
- Take a look at the attribute `data` on one of these layers. What kind of object is it?
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

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 
- Napari works with layers
- For a multi-channel image, each channel is a layer
- Most operations we performed earlier in Python can also be done in Napari,
  either graphically or in the terminal
::::::::::::::::::::::::::::::::::::::::::::::::
