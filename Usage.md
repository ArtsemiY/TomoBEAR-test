# TomoBEAR Configuration

If you followed the instructions thoroughly then you should have a
working copy of TomoBEAR. Now you need to generate a json file for your
project to start to process your acquired cryo EM data automated.

# Raw Tomography Data with Fiducials

To process tomogrpahy data with fiducials the following template should
be used as configuration for the pipeline. For some projects there are
more sophisticted features available which can be switched on or fine
tuned if needed.

```json
    {
        "general": {
            "project_name": "your project name",
            "project_description": "your project name description",
            "data_path": "/path/to/data/prefix*.mrc",
            "processing_path": "/path/to/processing/folder",
            "expected_symmetrie": "Cx",
            "template_matching_binning": xx,
            "gold_bead_size_in_nm": xx,
            "reconstruction_thickness": xxxx,
            "rotation_tilt_axis": xx,
            "aligned_stack_binning": x,
            "pre_aligned_stack_binning": x
            "binnings": [x, x, xx]
        },
        "MetaData": {
        },
        "SortFiles": {
        },
        "MotionCor2": {
        },
        "CreateStacks": {
        },
        "DynamoTiltSeriesAlignment": {
        },
        "DynamoCleanStacks": {
        },
        "BatchRunTomo": {
            "skip_steps": [4],
            "ending_step": 6
        },
        "StopPipeline": {
        },
        "BatchRunTomo": {
            "starting_step": 8,
            "ending_step": 8
        },
        "GCTFCtfphaseflipCTFCorrection": {
        },
        "BatchRunTomo": {
            "starting_step": 10,
            "ending_step": 13
        },
        "BinStacks": {
        },
        "Reconstruct": {
            "reconstruct": "binned"
        },
        "DynamoImportTomograms": {
        },
        "EMDTemplateGeneration": {
            "template_emd_number": "xxxx",
            "template_bandpass_cut_off_resolution_in_angstrom": 20
        },
        "DynamoTemplateMatching": {
            "size_of_chunk": [512, 720, 500],
            "sampling": 15
        },
        "TemplateMatchingPostProcessing": {
            "parallel_execution": false,
            "cc_std": 2.5,
            "crop_particles": true
        }
    }
```

## Raw Tomography Data without Fiducials

Focuse Ion-beam Milling Tomography Data

```json
    {
        "general": {
            "project_name": "fibmil project",
            "project_description": "fibmil project description",
            "data_path": "/path/to/data/prefix*.tif",
            "processing_path": "/path/to/processing/folder",
            "gain": "/path/to/gain.mrc",
            "dark": "/path/to/dark.mrc",
            "expected_symmetrie": "Cx",
            "template_matching_binning": xx,
            "reconstruction_thickness": xxxx,
            "rotation_tilt_axis": xx,
            "aligned_stack_binning": x,
            "pre_aligned_stack_binning": x,
            "binnings": [x, x, xx],
            "tilt_scheme": "bi_directional",
            "tilt_angles": [-9, -6, -3, 0, 3, 6, 9, 12, 15, 18, 21, 24, 27, 30, 33, 36, 39, 42, 45, 48, 51, 54, 57, 60, -12, -15, -18, -21, -24, -27, -3033, -36, -39, -42, -45, -48, -51, -54, -57, -60]
        },
        "MetaData": {
        },
        "SortFiles": {
        },
        "MotionCor2": {
        },
        "CreateStacks": {
        },
        "BatchRunTomo": {
            "starting_step": 0,
            "ending_step": 8,
            "directives": {
                "runtime.Fiducials.any.trackingMethod": 1,
                "comparam.xcorr_pt.tiltxcorr.SizeOfPatchesXandY": "512,512",
                "comparam.xcorr_pt.tiltxcorr.IterateCorrelations": 100,
                "runtime.PatchTracking.any.adjustTiltAngles": 0,
                "runtime.AlignedStack.any.eraseGold": 0,
                "runtime.Positioning.any.hasGoldBeads": 0,
                "comparam.xcorr_pt.tiltxcorr.FilterRadius2": 0.3,
                "comparam.xcorr_pt.tiltxcorr.FilterSigma2": 0.4,
                "comparam.xcorr_pt.tiltxcorr.OverlapOfPatchesXandY": "0.5,0.5"
            }
        },
        "GCTFCtfphaseflipCTFCorrection": {
        },
        "BatchRunTomo": {
            "starting_step": 10,
            "ending_step": 13
        },
        "BinStacks": {
        },
        "Reconstruct": {
            "reconstruct": "binned"
        },
        "StopPipeline": {
        },
        "DynamoImportTomograms": {
        },
        "EMDTemplateGeneration": {
            "template_emd_number": "xxxx",
            "template_bandpass_cut_off_resolution_in_angstrom": 20
        },
        "DynamoTemplateMatching": {
            "sampling": 15
        },
        "TemplateMatchingPostProcessing": {
            "cc_std": 2.5,
            "crop_particles": true
        }
    }
```

# Tilt Stacks with Fiducials

If you want to process already assembled tilt stacks with tomoBEAR you
need to provide the tilt angles in the order in which they apear in the
tilt stacks.

```json
    {
        "general": {
            "project_name": "Ribosome",
            "project_description": "Ribosome Benchmark",
            "data_path": "/sbdata/PTMP/nibalysc/ribosome/data/*.mrc",
            "processing_path": "/sbdata/PTMP/nibalysc/ribosome",
            "expected_symmetrie": "C1",
            "tilt_scheme": "bi_directional",
            "apix": 2.62,
            "tilt_angles": [-60.0, -58.0, -56.0, -54.0, -52.0, -50.0, -48.0, -46.0, -44.0, -42.0, -40.0, -38.0, -36.0, -34.0, -32.0, -30.0, -28.0, -26.024.0, -22.0, -20.0, -18.0, -16.0, -14.0, -12.0, -10.0, -8.0, -6.0, -4.0, -2.0, 0.0, 2.0, 4.0, 6.0, 8.0, 10.0, 12.0, 14.0, 16.0, 18.0, 20.0, 0, 24.0, 26. , 28.0, 30.0, 32.0, 34.0, 36.0, 38.0, 40.0, 42.0, 44.0, 46.0, 48.0, 50.0, 52.0, 54.0, 56.0],
            "gold_bead_size_in_nm": 9
    
        },
        "MetaData": {
        },
        "CreateStacks": {
            "execution_method": "sequential"
        },
        "DynamoTiltSeriesAlignment": {
            "execution_method": "sequential"
        },
        "DynamoCleanStacks": {
        },
        "BatchRunTomo": {
            "skip_steps": [4],
            "ending_step": 6
        },
        "BatchRunTomo": {
            "starting_step": 8,
            "ending_step": 8
        },
        "GCTFCtfphaseflipCTFCorrection": {
        },
        "BatchRunTomo": {
            "starting_step": 10,
            "ending_step": 13
        },
        "BinStacks": {
        },
        "Reconstruct": {
        },
        "DynamoImportTomograms": {
        },
        "EMDTemplateGeneration": {
            "template_emd_number": "3420"
        },
        "DynamoTemplateMatching": {
        },
        "TemplateMatchingPostProcessing": {
        },
        "DynamoAlignmentProject": {
            "iterations": 3,
            "classes": 4,
            "use_symmetrie": false,
            "use_noise_classes": true
        },
        "DynamoAlignmentProject": {
            "iterations": 3,
            "classes": 4,
            "use_noise_classes": true,
            "use_symmetrie": false,
            "selected_classes": [1]
        },
        "DynamoAlignmentProject": {
            "iterations": 3,
            "classes": 4,
            "use_noise_classes": true,
            "use_symmetrie": false,
            "selected_classes": [1]
        },
        "BinStacks": {
            "use_ctf_corrected_aligned_stack": false,
            "binnings": [2, 4, 8]
        },
        "DynamoAlignmentProject": {
            "classes": 1,
            "iterations": 1,
            "use_noise_classes": false,
            "swap_particles": false,
            "use_symmetrie": false,
            "selected_classes": [1],
            "as_boxes": true,
            "use_SUSAN": true,
            "binning": 8,
            "threshold":0.75
        },
        "DynamoAlignmentProject": {
            "classes": 1,
            "iterations": 1,
            "use_noise_classes": false,
            "swap_particles": false,
            "use_symmetrie": false,
            "selected_classes": [1,2],
            "as_boxes": true,
            "use_SUSAN": true,
            "binning": 4,
            "threshold":0.75
        },
        "DynamoAlignmentProject": {
            "classes": 1,
            "iterations": 1,
            "use_noise_classes": false,
            "swap_particles": false,
            "use_symmetrie": false,
            "selected_classes": [1,2],
            "as_boxes": true,
            "use_SUSAN": true,
            "binning": 2,
            "threshold":0.75
        },
        "DynamoAlignmentProject": {
            "classes": 1,
            "iterations": 1,
            "use_noise_classes": false,
            "swap_particles": false,
            "use_symmetrie": false,
            "selected_classes": [1,2],
            "as_boxes": true,
            "use_SUSAN": true,
            "binning": 1,
            "threshold":0.75
        }
    }
```

# Executing the Workflow

After you have generated an apropriate json file for your project you
may start the processing. There are two different execution strategies
which are described further in the follwoing chapters.

## Local Execution

To execute the workflow you just need to type:

`   tomoBEAR local /path/to/project.json /path/to/defaults.json`

## SLURM Execution

For the execution of the workflow on a cluster you need to adjust the
following keys in the general section:

Set the value for the key "slurm_partition" to decide on which cluster
partition the should run. Be sure to use a partition where the nodes
have GPUs installed.

-   "slurm_partition": "partition"

If you want to limit the processing to a set of specific nodes to be
nice to others so you leave some nodes for them for processing you need
to set the value for the key "slurm_node_list".

-   "slurm_node_list": \["node1", "node2", "node3"\]

To execute the workflow you just need to type:

`   tomoBEAR slurm /path/to/project.json /path/to/defaults.json`

## Cleanup

If the later described **keep_intermediates** flag is set during the
processing of a project to true you can cleanup the not needed
intermediate data afterwards if you run the following command.

Files that are kept are

-   tilt stacks
-   binned tilt stacks
-   ctf corrected tilt stacks
-   ctf corrected binned tilt stacks
-   aligned tilt stacks
-   binned aligned tilt stacks
-   ctf corrected aligned tilt stacks
-   ctf corrected binned aligned tilt stacks
-   tomograms
-   binned tomograms
-   ctf corrected tomograms
-   ctf corrected binned tomograms
-   batchruntomo related files (\*.rawtlt, \*.tlt, \*.xf, \*.ali)
-   particles with different binnings
-   particles table
-   some small tomoBEAR related files

all other files are removed.

`   tomoBEAR cleanup /path/to/project.json /path/to/defaults.json`