# Load, process, and analyze Xenocs Xeuss 3.0 GIWAXS data 
For GIWAXS data recorded with the beam at the corner of the Eiger2 1M detector!
This code is all a work-in-progress. Contributions are welcome! 

## Overview
Loading and processing code here requires an up-to-date [PyHyperScattering](https://github.com/usnistgov/PyHyperScattering) version (containing the `CMSGIWAXSLoader.py` and `PGGeneralIntegrator.py` scripts). PyHyperScattering utilizes [Xarray](https://docs.xarray.dev/en/stable/) for convenient and consistent structuring of data, especially useful when working with large, multidimensional datasets. The core operations within PyHyperScattering are performed with [pyFAI](https://pyfai.readthedocs.io/en/v2023.1/) & [pygix](https://pypi.org/project/pygix/) (and of course numpy). 

## Recommended conda environment setup:
1. Clone PyHyperScattering to a chosen directory in your local machine (I like to keep all my cloned github repos in the same place)
   - Enter terminal/powershell, ensure git is installed, navigate to directory where you want to clone to, then run:
     `git clone https://github.com/usnistgov/PyHyperScattering.git`
   - This step will be unecessary when the GIWAXS support code is included in published PyHyperScattering pip release 
2. Create conda environment (python >= 3.9), along with some basic packages:
   - `conda create -n name python numpy matplotlib jupyter ipympl`
   - **Activate your environment after creating it: `conda activate name`**
4. Install the local cloned PyHyperScattering package into your conda environment with `pip install -e`:
   - `pip install -e /path-to-PyHyperScattering-git-repository`
5. Install all required packages (this should handle pygix):
   - `pip install -r /path-to-PyHyperscattering-git-repository/requirements.txt`
6. Install lmfit `pip install lmfit` if fitting is desired in plotting notebook.

## General use:
- Processing notebook:
  - Load raw data from recored .edf's into xarray DataArrays
  - Check data, combine 'line erasor' detector images if applicable
    - Stitch horizontal detector gap and average all points where data overlaps between the two images
  - Rotate image so that bottom horizon is flat (if it isn't already)
  - Use PyHyperScattering to apply pygix-backed transformations
  - Save/export data from processed DataArrays as desired (.zarr stores recommended for moving forward with plotting notebook)
    
- Plotting notebook
  - Load processed zarr stores
  - Generate plots from cartesian (q_xy vs q_z) or polar (chi vs q_r) reciprocal space data
  - Use lmfit for fitting peaks in selected regions of interest

### Notes:
- When first importing PyHyperScattering, many warnings pop up. As long as the cell executes and you are using a correct PyHyperScattering version, this is fine.
