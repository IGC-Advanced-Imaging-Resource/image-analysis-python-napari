---
title: 'Thresholding and Segmentation in Napari'
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions 
- How can I apply thresholds and segmentation in Napari?
::::::::::::::::::::::::::::::::::::::::::::::::


## Exercise 23: Image labelling

Load the sample image 'napari builtins -> Human Mitosis' and perform the following processing on it:

- Connected component labelling

How many colonies do you get?

:::::::::::::::::::::::: solution
```python
max(viewer.layers[-1].data.flatten())
```
:::::::::::::::::::::::::::::::::

## Exercise 24: Connected components

Load up the sample image 'napari builtins -> Cells (3D+2Ch)' and perform
a watershed transform on it:

- Voronoi Otsu Labelling on the nuclei layer
- Seeded Watershed on the membrane layer, with the 'labelled nuclei' parameter
  set to the result of the Voronoi Otsu labelling

:::::::::::::::::::::::: solution
```python
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 

- Napari has plugins for performing multi-stage processes in one step
::::::::::::::::::::::::::::::::::::::::::::::::
