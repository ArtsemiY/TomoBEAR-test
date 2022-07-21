# Welcome to the TomoBEAR wiki!

!!! attention
      Currently we are at the stage of alpha-testing. We will be happy if you try it, we will help you to get going.

      Please write to [Nikita Balyschew](mailto:nikita.balyschew@googlemail.com?subject=[GitHub]%20TomoBEAR) if you have problems.

**TomoBEAR** is a configurable and customizable software package specialized for cryo-electron tomography and subtomogram averaging completely written in the MATLAB scripting language. TomoBEAR implements **B**asics for **E**lectron tomography and **A**utomated **R**econstruction of cryo electron tomography data. With TomoBEAR you can easily process tomographic data acquired by an electron microscope starting from the raw tilt series, possibly dose fractionated, or already assembled tilt stacks to the biological structure of interest.

TomoBEAR is designed to operate on a data set in parallel where it is possible and that the user has minimal intervention and doesn't need to learn all the different software packages (MotionCor2, IMOD, Gctf, Dynamo) to be able to process cryo-electron tomography data. We tried to come up with a set of predefined defaults which should fit many projects in the cryo-electron tomography regime. However if some parameters need to be tweaked for specific projects to achieve best results you are free to do so.

??? question "Why use TomoBEAR?"
      * TomoBEAR is based on best practices in processing tomographic data
      * Uses default presets which work on a variety of tested datasets
      * Standalone and MATLAB versions are available
      * Parallel execution is possible
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
      * TomoBEAR is based on JSON configuration files which can be easily shared between others so that they can validate or improve your results
      * TomoBEAR was developed and tested on standard datasets to achieve same or better results as with manual processing
      * You are able to look at the intermediates optimize parameters and rerun the steps to achieve optimal results
      * You are never locked to the TomoBEAR processing pipeline and can easily breakout at various steps to other software tools you prefer
      * Uses links where possible
      * Clean up functionality to save storage
      * Tomograms to be processed can be limited to a subset
      * Uses Dynamo tilt-series alignment but injects the fiducial positions to IMOD for projection estimation
      * Alignment parameters for tilt series can be (optionally) manually refined
      * Routines for particle identification: template matching or geometry-assisted particle picking (with Dynamo)
      * Improved Dynamo template matching functionality
        * 10x - 14x speedup leveraging the GPU compared to 28 CPUs achieving 18x speedup
      * Sub-stacking analysis (by SUSAN) is integrated (work in progress)
      * TemplateMatchingPostprocessing
        * Extraction of particles and conversion to dboxes to speed up file system access speed
      * DynamoAlignmentProject
        * Classification by multi-reference alignment (MRA)
          * Generation of initial templates with with true structures and "noise traps"
        * Extraction of particles with SUSAN and conversion to dboxes now possible
        * Forcing of using SUSAN is also possible
          * BinStacks module with appropriate binning levels should be run upfront
        * Modification of particles box size is possible, as input can be used a factor, the absolute box size can be used as input
      * Introduced presorting for single numbered tilt series data (as in Wenbo’s case of published Ryanodine receptor data)
      * Automated exclusion of bad tilts in reconstructions based on refined fiducial file from BatchRunTomo module

## Wiki contents

On this wiki you may find the following information regarding TomoBEAR project:

1. [Cloning, installation and setup instructions]() along with TomoBEAR dependencies listing.
2. [Quick start guide]() as well as [Tutorials]() to learn how to use TomoBEAR.
2. [General description of TomoBEAR structure]() as well as detailed [description of all available modules](), their parameters and default values.
3. [Usage tips and example usage cases]() and with the corresponding configuration templates provided.
4. [Instructions on how you may contribute]() to the TomoBEAR project.

## Availability & installation
TomoBEAR is located on its GitHub repository: <https://github.com/KudryashevLab/TomoBEAR>.

Once cloned, you may follow [instructions on installation and setup](http://127.0.0.1:8000/getting_started/installation-and-setup/).

## Participating
We are highly appreciate your efforts to contribute to the TomoBEAR!
Please report bugs and suggest features using [Issue tracker](https://github.com/KudryashevLab/TomoBEAR/issues).

## Contacts
[Misha Kudryashev (Principal Investigator)](mailto:misha.kudryashev@gmail.com?subject=[GitHub]%20TomoBEAR)

[Nikita Balyschew (Developer)](mailto:nikita.balyschew@gmail.com?subject=[GitHub]%20TomoBEAR)

[Vasili Mikirtumov (Application Engineer)](mailto:mikivasia@gmail.com?subject=[GitHub]%20TomoBEAR)

[Artsemi Yushkevich (Contributor)](mailto:artsemi.yushkevich@gmail.com?subject=[GitHub]%20TomoBEAR)
