Welcome to TomoBEAR a software package specialized for cryo electron
tomography which is written in MATLAB. With TomoBEAR you can process
tomographic data acquired from an electron microscope from the raw tilt
stacks to the biological structure of interest.

TomoBEAR is designed so that the user has minimal intervention and
doesn't need to learn the different software packages to be able to
process tomographic data from cryo EM. We tried to come up with a set of
defaults which should fit many projects in the cryo ET regime. However
if some parameters needs to be tweaked you are free to do so.

## Prerequisites

This software was tested on a machine with following properties:

-   OS: CentOS 7
-   GPU: at least one gpu with a minimum 10GB of RAM
-   RAM: 189GB
-   HDD/SDD: enough storage to store intermediate data, depends on size
    of data

## Setup

### Additional Software

#### MotionCor2

Head to the
[MotionCor2](https://docs.google.com/forms/d/e/1FAIpQLSfAQm5MA81qTx90W9JL6ClzSrM77tytsvyyHh1ZZWrFByhmfQ/viewform)
download page. There you need to register and download MotionCor2. A
MotionCor2 version greater than 1.4.0 is desired.

-   Alternative download [link](https://emcore.ucsf.edu/ucsf-software).

#### Gctf

Head to the
[Gctf](https://www2.mrc-lmb.cam.ac.uk/research/locally-developed-software/zhang-software/)
downlaod page and download Gctf version 1.06. Gctf version 1.18 could
also work but is not tested.

#### IMOD

Head to the
[IMOD](https://bio3d.colorado.edu/ftp/latestIMOD/RHEL7-64_CUDA8.0)
download page and get the IMOD version 4.10.42.

### Software Installation

Please follow the instructions for all the software packages you
downloaded. At best you will find the paths to the executables of the
downlaoded software in your PATH variable of your linux system. If this
is not the case you need to adjust the paths to the executables in the
"defaults.json" file which you can find in the "configurations" folder.

The keys where the values needs to be adjusted can be found in the
general section of the json file and are the following ones:

-   "motion_correction_command": "/path/to/MotionCor2_1.4.0_Cuda102"
-   "ctf_correction_command": "/path/to/Gctf-v1.06_sm_30_cu8.0_x86_64"

If you don't have the software in your path you can provide the full
path to the executable as the value. Also if you downloaded a newer
version you will need to adjust the name of the executable even if you
can find it in the PATH variable. Also be sure to use a version which
supports your current CUDA installation. Sometimes newer CUDA libraries
can handle versions compiled with older CUDA libraries. So it is worth
to try executables compiled for older CUDA libraries if you have newer
ones installed.

If you install IMOD the normal way then IMOD should be already in your
PATH variable and therefore callable from everywhere. This is the only
way supported by TomoBEAR.
