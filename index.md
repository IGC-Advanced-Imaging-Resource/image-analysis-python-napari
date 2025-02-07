---
site: sandpaper::sandpaper_site
---

This image analysis course provides an introduction to basic image analysis techniques in two environments:
[Python](https://www.python.org) and [Napari](https://napari.org).

These two environments have different interfaces for user interaction, but both offer powerful tools for image
processing and analysis. Both are discussed here in order to highlight the relative strengths of each one, and
to present the analysis techniques in this course as generic and independent of one particular platform.

The first section on Python consists of working in a Jupyter notebook setting, but can equally be done in the
terminal. This way of working also lends itself quite well to adapting your analysis process into a workflow that
can be applied to a batch of many images.

Napari is a point-and-click graphical user interface, which is well-suited to exploring and previewing images,
and is particularly useful for viewing multi-dimensional data such as 3D Z-stacks and time series. Incidentally,
Napari is built on top of Python, using many of the same tools available in a Jupyter or terminal setting. The
interactive nature of Napari makes it less easy to adapt to batch processing of many images, but it does have
ways of exporting the processing steps performed in the GUI to a text file or even a Jupyter notebook, that can
then be adapted for your own purposes.

This course will cover a range of basic filtering and segmentation techniques in Jupyter, and then the final chapter
will explore equivalents of these in Napari.

Two main sources were used for building this course:

- [Introduction to Bioimage Analysis by Pete Bankhead](https://bioimagebook.github.io)
- [Image data science with Python and Napari @EPFL](https://biapol.github.io/Image-data-science-with-Python-and-Napari-EPFL2022)

Building and publishing was done with the [Carpentries Workbench](https://carpentries.github.io/sandpaper-docs) and [GitHub Pages](https://pages.github.com).
