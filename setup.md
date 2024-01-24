---
title: Setup
---

Part 1 of this course requires access to some kind of Python environment, which
may be any of:

- A plain Python interpreter on the command line
- An IDE, e.g. PyCharm, Visual Studio Code
- A managed JupyterHub instance at your organisation
  - For courses taught at the University of Edinburgh, staff and students have access
    to Noteable at https://noteable.edina.ac.uk/login. If you wish to use this, log
    in here and proceed to GUI setup or command line setup below.
- A JupyterLab instance on your local machine

For part 2 on Napari, you'll need access to a machine with Napari installed.
Installation of Napari is covered below.


## Datasets

Todo: Test data is available at [data zip file](https://example.com/FIXME).


## Python Setup

::::::::::::::::::::::::::::::::::::::: discussion
### Details

We recommend Jupyter as a powerful way of saving your progress and displaying your
work in Python. All setup done here is based on either [Conda](https://docs.conda.io)
or [pip](https://pip.pypa.io) with Python
[virtual environments](https://docs.python.org/3/library/venv.html).
:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution
### JupyterHub - GUI setup

Once logged in, select 'New -> Folder' in the top right menu. This will create a new
folder called 'Untitled'. To rename it, check the box next to it, select 'Rename' in
the top left and enter a suitable name for this course. Navigate into the folder by
selecting its name in the navigator.

Now do the same steps again to create a folder called 'test_data' and navigate into
it.

Now that you're in a sensible location, you can upload select the 'Upload' button in
the top right and select the files to upload.

In the folder you've created, select 'New' -> 'Python 3 (ipykernel)'. This will open a
Jupyter notebook in a new browser tab.
:::::::::::::::::::::::::

:::::::::::::::: solution
### JupyterHub - command line setup

Once logged in, select 'New -> Terminal' in the top right. This will open a command line
shell in a new tab. Once here:

- Use `mkdir` to create a folder for this course and `cd` into it
- Inside here, use `mkdir` to create a 'test_data' folder and `cd` into it
- Use `wget` to download the test data

Now that the folders and test data are set up, navigate to your new course folder in the
GUI and select 'New -> 'Python 3 (ipykernel)'. This will open a Jupyter notebook in a new
browser tab.
:::::::::::::::::::::::::

:::::::::::::::: solution
### Jupyter installation

If you don't have access to a JupyterHub instance but will want to use Jupyter, you can
install it locally:

       $ conda install -c conda-forge jupyterlab  # with Conda
       $ pip install jupyterlab                   # with Pip

Once installed, start Jupyter with:

       $ jupyter-lab

This will start the server process and open JupyterLab in a new browser window. Follow the same
GUI or graphical folder setup as for JupyterHub.
:::::::::::::::::::::::::

:::::::::::::::: solution
### Local setup

If you wish to use Python on its own, then that is also possible.
You will also need the packages skimage, imageio, numpy and matplotlib.

When working with a local Python instance, it's almost always a good idea to install
packages into a [virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#creating-a-virtual-environment)
such as those created by Conda or Python's `venv` module.

       $ python -m venv ./python_environment
       $ source ./python_environment/bin/activate
       $ pip install skimage imageio numpy matplotlib
:::::::::::::::::::::::::


## Napari setup

::::::::::::::::::::::::::::::::::::::: discussion

### Details

While Napari has developed enormously in recent years, installing it still requires
the use of Pip and/or Conda.

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution
### Installation with Pip

Pip is Python's package manager. To install Napari into a Python virtual environment:

```
$ python -m venv path/to/new/virtualenv
$ source path/to/new/virtualenv/bin/activate
$ pip install 'napari[all]'
```
:::::::::::::::::::::::::

:::::::::::::::: solution
### Installation with Conda

A Conda environment containing Napari can be created with:

```
$ conda create -p path/to/new/env -c conda-forge napari pyqt
```

You may find that solving the environment is extrememly slow or fails
altogether. Up-to-date Conda setups are able to use a much faster
solver, [Mamba](https://mamba.readthedocs.io):

```
$ conda list -n base mamba  # make sure that conda-libmamba-solver is present in the base environment
$ conda install -n base conda-libmamba-solver  # install the Mamba solver if needed
$ conda config --show solver  # check the configured solver
$ config config --set solver libmamba  # if the confgured solver is 'classic'
```

:::::::::::::::::::::::::
