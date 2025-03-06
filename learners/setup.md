---
title: Setup
---

Part one of this course requires access to some kind of Python environment, which
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

::::::::::::::::::::::::::::::::::::::: discussion
Test data is available in the [GitHub repo](https://github.com/IGC-Advanced-Imaging-Resource/image-analysis-python-napari/tree/main/data).
These can be downloaded either by visiting each image and selecting 'Download raw file', or using a command line utility such as wget, as below.
::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution
### Downloading on the command line

The test images can be downloaded in fewer clicks with the command:

       wget https://raw.githubusercontent.com/IGC-Advanced-Imaging-Resource/image-analysis-python-napari/main/data/FluorescentCells_3channel.tif \
            https://raw.githubusercontent.com/IGC-Advanced-Imaging-Resource/image-analysis-python-napari/main/data/confocal-series_zstack.tif \
            https://raw.githubusercontent.com/IGC-Advanced-Imaging-Resource/image-analysis-python-napari/main/data/hela-cells_rgb.tif \
            https://raw.githubusercontent.com/IGC-Advanced-Imaging-Resource/image-analysis-python-napari/main/data/Ersi_organoid_WT2.nd2

:::::::::::::::::::::::::

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
the top left and enter a suitable name for the folder. Navigate into it by
selecting its name in the navigator.

Now do the same steps again to create a folder called 'test_data' and navigate into
it.

Now that you're in a sensible location, you can use the 'Upload' button in
the top right to select and upload your downloaded image files from GitHub.

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

If you don't have access to a JupyterHub instance but still want to use Jupyter, you can
install it locally either with Conda:

       $ conda install -c conda-forge jupyterlab  # with Conda

Or with pip:

       $ pip install jupyterlab                   # with Pip

Once installed, start Jupyter with:

       $ jupyter-lab

This will start the server process and open JupyterLab in a new browser window. Follow the same
GUI or graphical folder setup as for JupyterHub above.

You may need to install some extra packages, such as skimage. To do this, exit Jupyter and in the same
terminal window, run:

       $ pip install scikit-image

Or in Conda:

       $ conda install scikit-image

:::::::::::::::::::::::::

:::::::::::::::: solution
### Local setup with venv and pip

If you wish to use Python on its own, then that is also possible.
You will also need the packages skimage, imageio, numpy and matplotlib.

When working with a local Python instance, it's almost always a good idea to install
packages into a [virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#creating-a-virtual-environment)
such as those created by Conda or Python's `venv` module:

       $ python -m venv ./python_environment
       $ source ./python_environment/bin/activate
       $ pip install skimage imageio numpy matplotlib

Then to start up Python:

       $ python

:::::::::::::::::::::::::


## Napari setup

::::::::::::::::::::::::::::::::::::::: discussion

### Details

While Napari has developed enormously in recent years, installing it still requires
the use of Pip and/or Conda. See below for installation on Unix-like systems. The process
should take a few minutes to run.

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution
### Installation on Unix with Pip

Pip is Python's package manager. To install Napari into a Python virtual environment:

```
$ python -m venv path/to/new/virtualenv
$ source path/to/new/virtualenv/bin/activate
$ pip install 'napari[all]'
```
:::::::::::::::::::::::::

:::::::::::::::: solution
### Installation on Unix with Conda

A Conda environment containing Napari can be created with:

```
$ conda create -p path/to/new/env -c conda-forge -c pytorch devbio-napari pyqt
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

:::::::::::::::: solution
### Installation on Windows with Conda

This assumes that you already have Anaconda installed on your system. There should be an 'Anaconda
Prompt' option in the start menu. Select this to open a terminal session with Conda loaded, and run:

       > conda create --name devbio-napari napari=0.4.19 python=3.9 devbio-napari pyqt -c conda-forge -c pytorch

You can then activate the environment and run Napari from the terminal window:

       > conda activate devbio-napari
       > naparia

:::::::::::::::::::::::::
