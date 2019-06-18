WRF model 
==========

WRF and WRF-CHEM have been installed on Abel. To check which version is
available:


::

   module avail wrf

   ------------------------ /cluster/etc/modulefiles --------------------------
   wrf/3.9.1.1

| 
| To set-up your environment for WRF (no chemistry):


::

   export WRF_CHEM=0
   module load wrf/3.9.1.1

   
| 
| To set-up your environment for WRF-CHEM (with chemistry):


::

   module load wrf/3.9.1.1
     

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
  1) use.own                 4) hdf5/1.8.14_intel       7) jasper/1.900.1         10) wrf/3.9.1.1
  2) intel/2015.0            5) netcdf.intel/4.3.3.1    8) ncl/6.2.0
  3) openmpi.intel/1.8.3     6) intel-libs/2013.sp1.3   9) Anaconda3/5.1.0

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


WRF Preprocessing System (WPS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Most parameters for WPS must be given in namelist.wps

define_grid.py uses namelist.wps to plot your define

Most parameters for WPS must be given in namelist.wps

-  define_grid.py uses namelist.wps to plot your defined grid
-  run_geogrid.py  runs geogrid.exe to create static data for your
   domain
-  run_ungrib.py runs ungrib.exe to unpack your input GRIB data
-  run_metgrid.py runs metgrid.exe to interpolate input data onto your
   model domain

WRF
~~~

Most parameters for WRF must be given in namelist.input

-  run_init_wrf.py runs real.exe to initialize WRF and creates two files
   such as wrfinput_d<domain> and wrfbdy_d<domain> where <domain> is the
   domain number (01, 02, etc.)

-  run_wrf.py runs the numerical integration program wrf.exe

| 

TIPS: to get the usage of these python scripts, use -h or --help. For
instance:

::

   run_ungrib.py -h
   Usage: run_ungrib.py --expid expid --vtable vtable --datadir datadir [--start_date startdate --end_date enddate] [--interval_seconds val] [--prefix FILE]

   Options:
     -h, --help            show this help message and exit
     -s startdate, --start_date=startdate
                            A list of MAX_DOM character strings of the form
                           'YYYY-MM-DD_HH:mm:ss' specifying the starting UTC date
                           of the simulation for each domain.
     -e enddate, --end_date=enddate
                           A list of MAX_DOM character strings of the form 'YYYY-
                           MM-DD_HH:mm:ss' specifying the ending UTC date of the
                           simulation for each domain
     -t interval_seconds, --interval_seconds=interval_seconds
                           The integer number of seconds between time-varying
                           meteorological input files. No default value.
     -p FILE, --prefix=FILE
                           prefix of the intermediate meteorological data files
     -v Vtable, --vtable=Vtable
                           vtable filename
     -i expid, --expid=expid
                           Experiment identifier
     -d datadir, --datadir=datadir
                           Directory where input fields can be found

::

For each of these python scripts, some arguments are optionals and
indicated in squared brackets such as start_date and end_date. If you
don't specify them on the command line, it will keep what has been
defined in your namelist.  This is usually how you will run your "real"
cases.

WRF examples:
-------------

All WRF examples can be found in $WRF_EXAMPLES

A subdirectory can be found for each example and it contains all you
need to run the corresponding examples (namelist.wps, namelist.input,
Vtable, workflow.bash, and all the DATA required to run the simulation).
You may found one or more files named workflow*.bash. These files are
describing the sequence of programs to run for each example and can be
divided in two groups:

| 

`January2000Case`_
~~~~~~~~~~~~~~~~~~

This case is the East Coast Winter Storm of January 24-25, 2000
(http://cimss.ssec.wisc.edu/goes/misc/000125.html)

To run it, you need to execute each of the statement in workflow.bash:

| 

.. code:: bodytext

   > cat workflow.bash
   #!/bin/bash

   # check your domain is OK
   define_grid.py --path /cluster/software/VERSIONS/wrf/examples/January2000Case

   # Run geogrid.exe to create static data for this domain:
   run_geogrid.py -p /cluster/software/VERSIONS/wrf/examples/January2000Case --expid January2000Case

   # Unpack input GRIB data (ungrib.exe)
   run_ungrib.py --expid January2000Case             \
                 --start_date 2000-01-24_12:00:00    \
                 --end_date 2000-01-25_12:00:00      \
                 --interval_seconds 21600            \
                 --prefix FILE                       \
                 --vtable  /cluster/software/VERSIONS/wrf/examples/January2000Case/Vtable \
                 --datadir /cluster/software/VERSIONS/wrf/examples/January2000Case/DATA/


   #Interpolate the input data onto our model domain (metgrid.exe)
   run_metgrid.py --expid January2000Case

   # Initialize WRF model (real.exe/ideal.exe)
   run_init_wrf.py --expid January2000Case \
                   --namelist /cluster/software/VERSIONS/wrf/examples/January2000Case/namelist.input

   # Run the model (wrf.exe)
   run_wrf.py --expid January2000Case \
              --namelist /cluster/software/VERSIONS/wrf/examples/January2000Case/namelist.input

| 
| You don't have to change paths or namelists. it has been set-up to
  execute WPS/WRF in your workdir ($WORKDIR) and a subdirectory named
  January2000Case (named from your experiment identifier given with
  --expid option).

To visualize your outputs (named wrfout*), you may use `ncview`_:

::

             module load ncview

             ncview wrfout_d01_2000-01-24_12:00:00

| 
| and select the variables you wish to plot.  For instance, to visualize
  SST:

+-----------------------------------+-----------------------------------+
| .. container:: floatnone          | .. container:: floatnone          |
|                                   |                                   |
|    |Ncview-p1.png|                |    |Ncview-p2.png|                |
+-----------------------------------+-----------------------------------+

| 

TIPS: ncview is not recommended for quality graphical displays, but is a
very handy tool for a quick first-look at the data.

| 

`HurricaneKatrina`_\  
~~~~~~~~~~~~~~~~~~~~~~

As for previous examples, you just need to run workflow.bash and check
the directory $WORKDIR/HurricaneKatrina

On August 28, 2005, Hurricane Katrina was in the Gulf of Mexico, where
it strengthened to a Category 5 storm on the Saffir-Simpson hurricane
scale, packing winds estimated at 175 mph.
(http://www.katrina.noaa.gov/).

.. _ncview: http://meteora.ucsd.edu/~pierce/ncview_home_page.html
.. _HurricaneKatrina: http://www2.mmm.ucar.edu/wrf/OnLineTutorial/CASES/SingleDomain/

.. |Ncview-p1.png| image:: ./WRFand%20WRF-CHEM%20-%20mn_geo_geoit_files/Ncview-p1.png
   :width: 570px
   :height: 529px
   :target: https://wiki.uio.no/mn/geo/geoit/index.php/File:Ncview-p1.png
.. |Ncview-p2.png| image:: ./WRFand%20WRF-CHEM%20-%20mn_geo_geoit_files/Ncview-p2.png
   :width: 363px
   :height: 327px
   :target: https://wiki.uio.no/mn/geo/geoit/index.php/File:Ncview-p2.png
   
   
.. _January2000Case: http://www2.mmm.ucar.edu/wrf/OnLineTutorial/CASES/JAN00/
.. _GEOGRID.TBL: http://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/users_guide_chap3.htm#_Description_of_GEOGRID.TBL
.. _METGRID.TBL: http://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/users_guide_chap3.htm#_Description_of_METGRID.TBL
.. _Vtable: http://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/users_guide_chap3.htm#_Creating_and_Editing
.. _terrestrial static input data: http://www2.mmm.ucar.edu/wrf/OnLineTutorial/Basics/GEOGRID/ter_data.htm
