SURFEX/Crocus model 
====================

(in progress...)

Brief description of model
--------------------------


How to run it?
--------------------------
On Wessel, load the module Surfex ``module load surfex``. From the terminal then run Surfex/Crocus with the command ``s2m``. Type s2m for help via the terminal. An offline simulation command for running CROCUS can look like:

``s2m -b 20181001 -e 20181009 -f FORCING_xarray11.nc -g -o outputTest/``



How to create forcing data for offline simulation?
--------------------------

The file format must be Netcdf, in format NETCDF3_CLASSIC, with all variables in float64 type. The dimension time must be set as an unlimited dimension. All variable should have an attribute _fillValue=-9999999.0

An example file can found: https://opensource.umr-cnrm.fr/projects/snowtools_git/repository/revisions/master/raw/DATA/FORCING_test_base.nc

https://opensource.umr-cnrm.fr/projects/snowtools_git/wiki/Generate_your_own_forcing_files

**Required variables:**

- LAT           (Number_of_points) float64 ...
- LON           (Number_of_points) float64 ...
- LWdown        (time, Number_of_points) float64 ...
- PSurf         (time, Number_of_points) float64 ...
- Qair          (time, Number_of_points) float64 ...
- Rainf         (time, Number_of_points) float64 ...
- SCA_SWdown    (time, Number_of_points) float64 ...
- Snowf         (time, Number_of_points) float64 ...
- Tair          (time, Number_of_points) float64 ...
- UREF          (Number_of_points) float64 ...
- Wind          (time, Number_of_points) float64 ...
- Wind_DIR      (time, Number_of_points) float64 ...
- ZREF          (Number_of_points) float64 ...
- ZS            (Number_of_points) float64 ...
- aspect        (Number_of_points) float64 ...
- slope         (Number_of_points) float64 ...

The forcing data can be previewed with netcdf programs such as ncview on linux (http://meteora.ucsd.edu/%7Epierce/ncview_home_page.html).


Where to find further information?
--------------------------

Vionnet, V., et al. "The detailed snowpack scheme Crocus and its implementation in SURFEX v7. 2." Geoscientific Model Development 5 (2012): 773-791. doi:10.5194/gmd-5-773-2012

https://opensource.umr-cnrm.fr/projects/snowtools_git/wiki


