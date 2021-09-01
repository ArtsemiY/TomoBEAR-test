# Prerequisites

This software was developed and tested on machines with the following properties:

-   Operating System (OS): CentOS 7 / Ubuntu 21.04

Other Linux-based OSs should also be possible as long as MATLAB and all the other needed tools are runnable.

-   Graphics Processing Unit (GPU): at least one GPU with a minimum of 8GB of Video Random Access Memory (VRAM)

It is better to have more GPUs with possibly greater amount of VRAM. With more GPUs you can process more data units in parallel. With more VRAM you can fit bigger tilt stacks or tomograms in memory especially in template matching.

-   Random Access Memory (RAM): at least 16GB of RAM

The more RAM you have the better. With more RAM you can run more parallel processes to process to process your data faster. This depends on the execution method of the modules.

-   Hard Disk Drive (HDD) / Solid State Disk (SSD): depends on size of data

You need enough storage to store your data and all the intermediate data and results. Although it is possible to clean up the intermediate data during processing you will still need temporarily enough storage to store it until it can be deleted. The amount of needed storage is larger when you have more processes running in parallel.

# Setup

There are two ways to operate tomoBEAR. 

* The first way is to use it directly from MATLAB
* The second way is to use a standalone executable which is available precompiled or can be compiled on your own

For both methods of operation you have to install the additional software mentioned below in the chapter of the same name.

## MATLAB

If you want to run on a local machine then it is advised to run tomoBEAR from within MATLAB. This way you also don't need to download and install the MATLAB Compiled Runtime (MCR) if it is not already installed in your facility.

Everything you need to get tomoBEAR on your machine is to change to some folder where you want to have tomoBEAR and execute the following command 

* `git clone https://github.com/KudryashevLab/tomoBEAR.git`. 

After that change to the folder tomoBEAR with 

* `cd tomoBEAR`

Inside of the tomoBEAR folder you will find a configurations folder in which the file `defaults.json` can be found. Open the file `defaults.json` and adjust the following variables inside the general section

* `"pipeline_location": ""` put in the double quotes the location of tomoBEAR
* `"motion_correction_command": ""` put in the double quotes the executable name of MotionCor2 or the full path (with the executable name)
* `"ctf_correction_command": ""` put in the double quotes the executable name of Gctf or the full path (with the executable name)
* `"dynamo_path": ""` put in the double quotes the path to your dynamo folder

and start MATLAB with the command 

* `./run_matlab.sh`

if MATLAB in your systems `PATH` variable.

## Standalone

TomoBEAR can also be used as a standalone application for that you will need the standalone package itself which can be found [here](https://github.com/KudryashevLab/tomoBEAR/releases) with all the additional software which is mentioned below installed. Additionally you will need the MCR (MATLAB Compiled Runtime) from [here](https://www.mathworks.com/products/compiler/matlab-runtime.html). There you need to get the newest [MCR 2021a](https://ssd.mathworks.com/supportfiles/downloads/R2021a/Release/4/deployment_files/installer/complete/glnxa64/MATLAB_Runtime_R2021a_Update_4_glnxa64.zip) to be able to run tomoBEAR. When the download of the MCR is finished you will need to give it execution rights.

For that change to the folder where the file was downloaded to and execute the following command

* `chmod u+x MATLAB_Runtime_R2021a_Update_4_glnxa64.zip`

Alternatively you can change to some folder and execute the following command before you execute the previous one

* `wget https://ssd.mathworks.com/supportfiles/downloads/R2021a/Release/4/deployment_files/installer/complete/glnxa64/MATLAB_Runtime_R2021a_Update_4_glnxa64.zip`

Afterwards you need to extract the archive either with a command or through your file explorer

* `unzip MATLAB_Runtime_R2021a_Update_4_glnxa64.zip`

Change to the directory where the files were extracted and run the installation with the following command and follow the wizard which is displayed on screen

* `./install`

## Additional Software

As tomoBEAR is also wrapping standardized tools to fulfill some of the processing steps these need to be installed and executable. The advantage of such an Best of Breed approach is that you can profit of developments in algorithms in these tools and you can use them in the pipeline without any changes to the code at best.

### Module System 

If you are working in a cryo electron microscopy facility and employ a cluster with a module system where all the needed software is already deployed as modules it is fairly easy to setup tomoBEAR. If not all the software packages are available as modules you have two possibilities.

1. The first and probably the easiest possibility for inexperienced users is to ask the administrator or some responsible person for the module system to introduce the needed software as modules

2. The second and probably faster possibility is to install the software on your own in your home folder if you don't have root permissions and put it your PATH variable or adjust the defaults.json so that the variables to tools contain the full path. 

If all the software is available as modules you need to head to the `defults.json` file and find the entry `"modules": []` just replace it with `"modules": ["IMOD", "Gctf", "MotionCor2"]`. Be aware that these module names are just placeholders for your real module names. you can find them out with the command `module available` or the shortcut `module avail`.

As for the other software packages you can add the required CUDA versions also to the field modules.

### Manual Installation

The easiest way for the manual installation is to add the repositories with CUDA to your specific OS package manager. That is yum in CentOS and apt or apt-get in Ubuntu. The other way is a manual installation from the executables which are available from the [NVIDIA homepage](https://developer.nvidia.com/cuda-toolkit-archive). 

#### CUDA

For all the additional software packages the proper CUDA toolkits with the newest driver for your graphics card need to be installed. 

To install CUDA you can use the package manager of your OS install it manually or just use the module system of your facility if you employ one.

##### CentOS 7

A description of how to install a specific CUDA version for CentOS 7 is available [here](https://linuxconfig.org/how-to-install-nvidia-cuda-toolkit-on-centos-7-linux). Please follow the instructions there. To get other CUDA versions you will need to replace the rpm `cuda-repo-rhel7-10.0.130-1.x86_64.rpm` with a suitable rpm from [here](https://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/).

You need to repeat the steps multiple times until you have all the needed CUDA versions in ypur system installed to be able to run all the tools which are mentioned below.

##### Ubuntu 21.04

To get the newest CUDA on an Ubuntu system the easiest way is to install it via graphic

#### Dynamo

For the non standalone version of tomoBEAR you need a Dynamo version with tilt stack alignment capabilities. The newest version can be downloaded from here.

To minimize the dependencies on different CUDA versions it is advised to recompile the CUDA kernel for averaging with the newest CUDA version which is at best already available on your machine. If not please revise the chapter CUDA on this page.

To recompile the kernel you just need to the location where dynamo was extracted and access the folder ´cuda´ inside. There you will find a file called ´makefile´ which you need to open and modify the second line containing the variable ´CUDA_ROOT=´. Please put in there the path to your most recent CUDA release available on the system.

To recompile just execute the following two commands:

* `make clean`
* `make`

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

#### Software Installation

Please follow the instructions for all the software packages you
downloaded. At best you will find the paths to the executables of the
downlaoded software in your `PATH` variable of your Linux system. If this
is not the case you need to adjust the paths to the executables in the
`defaults.json` file which you can find in the folder `configurations`.

The keys where the values needs to be adjusted can be found in the
general section of the json file and are the following ones:

* `"pipeline_location": ""`
* `"motion_correction_command": ""`
* `"ctf_correction_command": ""`
* `"dynamo_path": ""`

If you don't have the software in your path you can provide the full
path to the executable as the value. Also if you downloaded a newer
version you will need to adjust the name of the executable even if you
can find it in the PATH variable. Also be sure to use a version which
supports your current CUDA installation. Sometimes newer CUDA libraries
can handle versions compiled with older CUDA libraries. So it is worth
to try executables compiled for older CUDA libraries if you have newer
ones installed.

If you install IMOD the normal way then IMOD should be already in your
`PATH` variable and therefore callable from everywhere. This is the only
way supported by tomoBEAR.
