WRF model 
==========

WRF and WRF-CHEM have been installed on Abel. To check which version is
available:

| 

::

   module avail wrf

   -------------------------------------------------------------------------------------- /cluster/etc/modulefiles ---------------------------------------------------------------------------------------
   wrf/3.6.1

| 
| To set-up your environment:

| 

::

   module load wrf/3.6.1

| 
| When loading wrf, the following environment variables are defined:

-  **WRF_HOME**: directory where WRF has been installed; this can be
   used to check WRF sources
-  **WPS_HOME**: directory where WPS has been installed. It is unlikely
   you have to check or change WPS source code but you will find in this
   directory all the metadata required to run WPS (the `GEOGRID.TBL`_,
   `METGRID.TBL`_, and `Vtable`_ files).
-  **WPS_GEOG_PATH**: contains the\ `terrestrial static input data`_.
   Even if you use your own compiled version of WRF/WPS, it is NOT
   necessary to download these files again.
-  **WRFIO_NCD_LARGE_FILE_SUPPORT**: it is set to 1 to allow you to
   store large netCDF files (more than 2GB).
-  **WRF_EXAMPLES**: directory where all WRF and WRF-CHEM examples are
   stored. If you wish to run one of this example see our dedicated
   section on running tutorials.

 

WRF has been compiled with intel compilers and MPI:

::

   module list 

   Currently Loaded Modulefiles:
     1) use.own                 3) openmpi.intel/1.6.1     5) netcdf.intel/4.2.1.1    7) jasper/1.900.1          9) openmpi.intel/1.8      11) wrf/3.6.1
     2) intel/2011.10           4) hdf5/1.8.9_intel        6) intel-libs/2013.sp1.3   8) intel/2013.sp1.3       10) ncl/6.2.0

| 
| We suggest you specify the version you wish to use to avoid any
  problems if we install a new default version (as it is important to
  stick to the very same version for your  simulations).

| 

Running WRF:
------------

The following steps are necessary to run WRF on our systems:

#. WRF Preprocessing System (WPS)
#. WRF System

| 

WRF Preprocessing System (WPS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Most parameters for WPS must be given in namelist.wps

define_grid.py uses namelist.wps to plot your define

.. _GEOGRID.TBL: http://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/users_guide_chap3.htm#_Description_of_GEOGRID.TBL
.. _METGRID.TBL: http://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/users_guide_chap3.htm#_Description_of_METGRID.TBL
.. _Vtable: http://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/users_guide_chap3.htm#_Creating_and_Editing
.. _terrestrial static input data: http://www2.mmm.ucar.edu/wrf/OnLineTutorial/Basics/GEOGRID/ter_data.htm
