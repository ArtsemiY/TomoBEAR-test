Welcome to the tomoBEAR wiki!

TomoBEAR a software package specialized for cryo electron tomography which is written in MATLAB and interfaces standardized tools for cryo electron tomography. With tomoBEAR you can easily process tomographic data from the raw tilt stacks which are acquired by an electron microscope to the biological structure of interest.

TomoBEAR is designed to operate on a data set in parallel where it is possible and that the user has minimal intervention and doesn't need to learn all the different software packages (MotionCor2, IMOD, Gctf, Dynamo) to be able to process cryo electron tomography data. We tried to come up with a set of predefined defaults which should fit many projects in the cryo electron tomography regime. However if some parameters need to be tweaked for specific projects to achieve best results you are free to do so.

There are various advantages to use tomoBEAR:
* It is based on best practices in processing tomographic data
* Uses default presets based on best practices
* Absolute binning levels (not relative)

* Standalone and MATLAB version available
* Parallel processing is possible
* Computing resources are configurable
* Supports SLURM cluster scheduler
* Standardized folder structure
* Can deal with
  * misnumbered tilt images due to SerialEM crashes, based on timestamps
  * different naming conventions
  * EERs, MRCs and TIFs from K2 and K3
  * duplicated projections due to tracking issues (first, last, keep)
* Restarting / resuming is possible (e.g. in case of errors, wrong configuration)
  * Checkpoints are created after every processing step of a tilt series or tomogram
* It is based on JSON configuration files which can be easily shared between others so that they can validate or improve your results
* It was developed and tested on well known datasets to achieve same or better results as with manual processing
* You are able to look at the intermediates optimize parameters and rerun the steps to achieve optimal results
* You are never locked to the tomoBEAR processing pipeline and can easily breakout at various steps to other software tools you prefer
* Uses links where possible
* Clean up functionality to save storage
* Tomograms to be processed can be limited to process only a subset
* Uses Dynamo tilt-series alignment but injects the fiducial positions to IMOD for projection estimation
* Fiducial positions can be manualy further refined
* Post-processing routines for particle extraction
* Improved Dynamo template matching functionality
  * 10x - 14x speedup leveraging the GPU compared to 28 CPUs achieving 18x speedup
* SUSAN is integrated


In the following chapters we will describe...

1. how to install and setup tomoBEAR
2. all the available modules and their parameters
3. how to use tomoBEAR for different datasets
4. how to develop modules for tomoBEAR to incorporate newly developed tools or algorithms