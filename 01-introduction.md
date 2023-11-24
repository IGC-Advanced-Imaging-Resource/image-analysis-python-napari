---
title: 'Introduction'
teaching: 10
exercises: 1
---

:::::::::::::::::::::::::::::::::::::: questions 
- How do I get started in JupyterHub?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Set up a workspace in JupyterHub
- Download the test data for these exercises
- Open a new JupyterHub notebook
::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

For the exercises in this lesson you are free to use whatever Python environment you
wish, however we highly recommend Jupyter as a powerful way of saving your progress
and displaying your work. This introduction will cover how to set up a new workspace
in a JupyterHub, which is an online instance of Jupyter that you log into, as opposed
to a locally running instance of Jupyter.

## Getting started in JupyterHub

Under 'New' in the top right, select 'Folder'. This will create a new folder called
'Untitled'. To rename it, check the box next to it, select 'Rename' in the top left
and enter a suitable name. Navigate into the folder by selecting its name in the
navigator.

Now do the same steps again to create a folder called 'test_data' and navigate into
it.

Now that you're in a sensible location, you can upload the test data.

## Uploading test data via the web interface

Select the 'Upload' button in the top right and select the files to upload.

## Opening a new notebook in Jupyter

Select the new folder to navigate into it and select 'New' -> 'Python 3 (ipykernel)'.
This will open a new notebook in a new browser tab.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise 0 (optional): Uploading test data via the command line

If you wish to download test data into JupyterHub directly and are confident on the
command line, then you may wish to use this method:

- In the top right, select 'New' -> 'Terminal' to open a shell session in a new tab
- Create a folder for the test data if not already done
- Navigate into the new folder
- Use `wget` and the URL of the test data to download

:::::::::::::::::::::::: solution 
```output
$ mkdir test_data
$ cd test_data
$ wget <test_data>
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 

- JupterHub is an online instance of Jupyter
- It is possible to manage files graphically or on the command line

::::::::::::::::::::::::::::::::::::::::::::::::
