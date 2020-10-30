Flexpart model 
===============
FLEXPART (FLEXible PARTicle dispersion model) is an Lagrangian transport and
dispersion model and can be used to study a large range of atmospheric transport
processes. The current version is FLEXPART V 10.4 and you can find more
information and resources on FLEXPART in the reference paper `reference paper`_
or at `flexpart.eu`_. 

FLEXPART installation
-------------------------------
FLEXPART is currently not available as a preloaded module on SAGA which means
that you have to compile the source code yourself. First you need to download
the FLEXPART source code by cloning the git repository.

::

    $git clone https://github.com/flexpart/flexpart.git



After you have cloned the repository you need to edit the paths in the Makefile
in flexpart/src/makefile. To compile on saga you would need change
the following paths:

::
    
    #Changes to be made in flexpart/src/makefile
    F90       = ifort
    MPIF90    = mpif90

    INCPATH1 = /usr/include
    INCPATH2 = /cluster/software/ecCodes/2.9.2-intel-2018b/include 
    LIBPATH1 = /cluster/software/ecCodes/2.9.2-intel-2018b/lib 



Before you compile FLEXPART you need to load the necessary fortran modules:

::

    module load ecCodes/2.9.2-intel-2018b
    module load netCDF-Fortran/4.4.4-intel-2018b




When compiling FLEXPART, you can add ncf=yes to
enable netCDF output.

::

    $ cd flexpart/src
    $ make ncf=yes

FLEXPART Setup 
--------------------- 
Setting up a FLEXPART is primarily done by
editing the COMMAND, RELEASES, OUTGRID files in the flexpart/options folder. For
a in depth explanation of the different settings in flexpart take a look at the
`reference paper`_ . FLEXPART expect following files and folders to be present:

::

    $ ls
    options output pathnames

- *options* folder containing the flexpart setting files COMMAND, RELEASES, OUTGRID etc.
- *output* folder which should be empty
- *pathnames* a files containing the necessary paths, particularly the path to the atmospheric forcing

:: 
    
    $ less pathnames
    ./options/
    ./output/
    /
    /path/to/AVAILABLE_WINDFIELDS
    ============================================
    pathnames (END)


*AVAILABLE_WINDFIELDS* is a text file contains the paths to where the
atmospheric forcing is stored and has to be formatted in a certain way. (A
`python script`_ which makes the AVAILABLE_WINDFIELDS). The formatted
AVAILABLE_WINDFIELDS file should look something like this.

:: 
    
    DATE     TIME         FILENAME             SPECIFICATIONS
        YYYYMMDD HHMISS
        ________ ______      __________________      __________________
    19990301 000000      /cluster/shared/flexpart/databases/flexpart/WIND_FIELDS/ECMWF/GLOBAL/ERA5/1999/03/EA99030100      ON DISC
    19990301 030000      /cluster/shared/flexpart/databases/flexpart/WIND_FIELDS/ECMWF/GLOBAL/ERA5/1999/03/EA99030103      ON DISC
    19990301 060000      /cluster/shared/flexpart/databases/flexpart/WIND_FIELDS/ECMWF/GLOBAL/ERA5/1999/03/EA99030106      ON DISC
    19990301 090000      /cluster/shared/flexpart/databases/flexpart/WIND_FIELDS/ECMWF/GLOBAL/ERA5/1999/03/EA99030109      ON DISC
    19990301 120000      /cluster/shared/flexpart/databases/flexpart/WIND_FIELDS/ECMWF/GLOBAL/ERA5/1999/03/EA99030112      ON DISC
                                                ...........................


On Saga atmospheric forcing for FLEXPART is stored in shared folder
*/cluster/shared/databases/flexpart* and currently only ERA-INTERIM is globally
available on Saga, from 1986 until 2016.

Running FLEXPART
----------------
To run FLEXPART Saga you would first need to make a job script:

::

    #! /bin/bash
    #SBATCH --account=nnXXXXk
    #SBATCH --time=00:40:00
    #SBATCH --ntasks=1
    #SBATCH --mem-per-cpu=6G
    #SBATCH --mail-user=example@student.geo.uio.no
    #SBATCH --job-name=FLEXPART_TEST
    #SBATCH --mail-type=FAIL
    set -o errexit
    set -o nounset
    module --quiet purge
    module load ecCodes/2.9.2-intel-2018b
    module load netCDF-Fortran/4.4.4-intel-2018b
    export PATH=/path/to/flexpart/src:$PATH
    time FLEXPART

Make sure you have the jobscript in the folder where the FLEXPART simulation has
been setup and then submit the job to the jobqueue.

::
    
    $ sbatch flexpart_job.sh

FLEXPART simulations does not depend on each other, which means that FLEXPART can
easily be parallelized by running many FLEXPART instances at the same time, through submitting
multiple jobs to the job queue. Though be careful as there is a limit of 10000 jobs that 
can be in the queue at the same time.     

.. _reference paper: https://gmd.copernicus.org/articles/12/4955/2019/
.. _flexpart.eu : https://www.flexpart.eu/
.. _python script : https://gist.github.com/Ovewh/ecb6b85ffcff8c25fe8b3847fe149b05