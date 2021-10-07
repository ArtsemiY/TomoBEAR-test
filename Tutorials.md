This page contains tomoBEAR tutorials which should showcase the capabilities of the processing pipeline. These tutorials assume that you have already cloned the [tomoBEAR github repository](https://github.com/KudryashevLab/tomoBEAR), installed all the required software ans setup tomoBEAR.

# Ribosome (EMPIAR-10064)

As a first data set to showcase the capabilities of tomoBEAR we have chosen the Ribosome data set with the number 10064 in the EMPIAR database.

You can get the data set from [here](https://www.ebi.ac.uk/empiar/EMPIAR-10064/). In our case we used just the mixedCTEM data and achieved 11.2Å in resolution with ~3.5k particles. If you want you can additionally use the CTEM data to be able to pick more particles.

After downloading the data extract it in a folder of your choice. One thing one should note about this data is that

* the data is already motion corrected
* the stacks are already assembled
* the pixel size is not in the header
* the tilt angles are not provided

Because of such circumstances which sometimes occur tomoBEAR is able to inject this data along with the JSON which describes the processing pipeline.

If you have already cloned the [tomoBEAR github repository](https://github.com/KudryashevLab/tomoBEAR) to your local machine you can find in the configurations folder a file called `ribosome_empiar_10064_dynamo.json`. This file describes the processing pipeline which should be setup by tomoBEAR to process this data set.

The following paragraphs will explain the variables contained in the JSON file and the needed changes to be able to run tomoBEAR on your local machine. In the end of this chapter the whole JSON file is shown.

First of all and most importantly you need to show tomoBEAR the path to the data and the processing folder. This must be done in the general section of the `json` file. 
```json
    "general": {
        "project_name": "Ribosome",
        "project_description": "Ribosome EMPIAR 10064",
        "data_path": "/path/to/ribosome/data/*.mrc",
        "processing_path": "/path/to/processing/folder",
        "expected_symmetrie": "C1",
        "apix": 2.62,
        "tilt_angles": [-60.0, -58.0, -56.0, -54.0, -52.0, -50.0, -48.0, -46.0, -44.0, -42.0, -40.0, -38.0, -36.0, -34.0, -32.0, -30.0, -28.0, -26.0, -24.0, -22.0, -20.0, -18.0, -16.0, -14.0, -12.0, -10.0, -8.0, -6.0, -4.0, -2.0, 0.0, 2.0, 4.0, 6.0, 8.0, 10.0, 12.0, 14.0, 16.0, 18.0, 20.0, 22.0, 24.0, 26.0, 28.0, 30.0, 32.0, 34.0, 36.0, 38.0, 40.0, 42.0, 44.0, 46.0, 48.0, 50.0, 52.0, 54.0, 56.0],
        "gold_bead_size_in_nm": 9,
        "template_matching_binning": 8,
        "binnings": [2, 4, 8],
        "reconstruction_thickness": 1400,
        "as_boxes": false
  }
```

Everything else should be fine for now and the processing can be started. To run the tomoBEAR on the Ribosome data set you need to type in the following command in the command window of MATLAB

```matlab
runTomoBear("local", "/path/to/ribosome_empiar_10064_dynamo.json")
```

or if you are using a compiled version of tomoBEAR and have everything set up properly type in the following command on the command line from the tomoBEAR folder

```shell
./run_tomoBEAR local /path/to/ribosome_empiar_10064_dynamo.json /path/to/defaults.json
```

When you followed all the steps thoroughly tomoBEAR should run up to the first appearence of StopPipeline. That means the following modules will be executed.

```json
    "MetaData": {
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
    }
```
This can take a while, as the result of this segment tomoBEAR will create a folder structure with subfolders for the individual steps. You can monitor the progress of the execution in shell and by inspecting the contents of the folders. Upon success of an operation a file SUCCESS is written inside each folder. If you want to rerun a step you can terminate the process, change parameters, remove the SUCCESS file (or the entire subfolder) and restart the process. Here the stacks have already been assembled, so neither `"Motioncorr2": {}`, not `"SortFiles": {}` modules were not needed. Here the key functionality is performed by `"DynamoTiltSeriesAlignment": {}` (a recommended tutorial can be found [here https://wiki.dynamo.biozentrum.unibas.ch/w/index.php/Walkthrough_on_GUI_based_tilt_series_alignment]) after which the projections containing low number of tracked gold beads are excluded by `"DynamoCleanStacks": {}`. Finally, the output is converted into an IMOD project.

The running time depends on your infrastructure and setup. After tomoBEAR stops you can inspect the fiducial model in the folder of `batchruntomo` which you can find in your processing folder.

```shell
cd /path/to/your/processing/folder/5_BatchRunTomo_1
```

Now you can inspect the alignment of every tilt stack one after the other and can possibly refine it if needed. For that you can use the following command. Please replace `xxx` with the tomogram number(s) that you want to inspect.

```shell
etomo tomogram_xxx/*.edf
```

When `etomo` starts just chose the `fine alignment` step which should be Lila if everything went fine for that tomogram and then click on `edit/view fiducial model` to start `3dmod` with the right options to be able to refine the gold beads. Before you start to refine just press the arrow up button in the top left corner of the window with the view port. To refine the gold beads click on `Go to next big residual` in the window with the stacked buttons from top to bottom and the view in the view port window should change immediately to the location of a gold bead with a big residual. Now see if you can center the marker better on the gold bead with the right mouse button. It is important that you don't put it on the peak of the red arrow but center it on the gold bead. When you are finished with this gold bead just press again on the `Go to next big residual` button. After you are finished with re-centering the marker on the gold beads you need to press the `Save and run tiltalign` button.

After you finished the inspection of all the alignments you can start `tomoBEAR` again as previously and it will continue from where it stopped up to the next `StopPipeline` section.

To continue running `tomoBEAR` on the Ribosome data set you need to type in as previously the following command in the command window of MATLAB

```matlab
runTomoBear("local", "/path/to/ribosome_empiar_10064_dynamo.json")
```

or if you are using a compiled version of tomoBEAR and have everything set up properly type in the following command on the command line from the `tomoBEAR` folder

```shell
./run_tomoBEAR local /path/to/ribosome_empiar_10064_dynamo.json /path/to/defaults.json
```

`tomoBEAR` should now detect that it has stopped at the previous step `StopPipeline` and continue from where it stopped. The following excerpt from the `ribosome_empiar_10064_dynamo.json` file is describing what `tomoBEAR` needs to do next.

```json
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
        "template_emd_number": "3420",
        "flip_handedness": true
    },
    "DynamoTemplateMatching": {
    },
    "TemplateMatchingPostProcessing": {
        "cc_std": 2.5
    },
```

This segment performs estimation of defocus using GCTF (GCTFCtfphaseflipCTFCorrection{}). You can inspect the quality of fitting by going into the folder and typing `imod tomogram_XXX/slices/*.ctf` and making sure that the Thon rings match the estimation. If not - play with the parameters of the GCTFCtfphaseflipCTFCorrection module. 

Then binned aligned ctf-corrected stacks are produced by BinStacks{} and tomographic reconstructions are generated for the binnnings specified in the general segment. In this example the particles are picked using template matching. First a template from EMDB is produced at a proper voxel side, then DynamoTemplateMatching{} creates cross-correlation volumes which can be inspected. Finally, highest cross-correlation peaks, over 2.5 standard deviations above the mean value in the CC volume are set for extraction to 3D particle files, the coordinates are stored in the particle_table files in the dynamo format. 

In the section below you will find subtomogram classification projects that should produce you a reasonable structure. They first use multi-reference alignment projects with "noise traps" to first classify out false-positive particles produced by template matching, this happens at the binning which was used for template matching. In the end of the segment you should have a reasonable set of particles in the best class. 

```json
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 3,
        "use_noise_classes": true,
        "selected_classes": [1],
        "box_size": 1.10,
        "binning": 4
    },
    "StopPipeline": {
    }
```

After that you will need to process tomograms at lower binning in order to reduce the voxel size. This is done in the extension of the JSON file below, however automated workflow is finished here as the user needs to play with the masks, particle sets, etc. 

Here comes the full JSON file to setup the processing pipeline in tomoBEAR and process the data

```json
{
    "general": {
        "project_name": "Ribosome",
        "project_description": "Ribosome EMPIAR 10064",
        "data_path": "/path/to/ribosome/data/*.mrc",
        "processing_path": "/path/to/processing/folder",
        "expected_symmetrie": "C1",
        "apix": 2.62,
        "tilt_angles": [-60.0, -58.0, -56.0, -54.0, -52.0, -50.0, -48.0, -46.0, -44.0, -42.0, -40.0, -38.0, -36.0, -34.0, -32.0, -30.0, -28.0, -26.0, -24.0, -22.0, -20.0, -18.0, -16.0, -14.0, -12.0, -10.0, -8.0, -6.0, -4.0, -2.0, 0.0, 2.0, 4.0, 6.0, 8.0, 10.0, 12.0, 14.0, 16.0, 18.0, 20.0, 22.0, 24.0, 26.0, 28.0, 30.0, 32.0, 34.0, 36.0, 38.0, 40.0, 42.0, 44.0, 46.0, 48.0, 50.0, 52.0, 54.0, 56.0],
        "gold_bead_size_in_nm": 9,
        "template_matching_binning": 8,
        "binnings": [2, 4, 8],
        "reconstruction_thickness": 1400,
        "as_boxes": false
    },
    "MetaData": {
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
    },
    "DynamoImportTomograms": {
    },
    "EMDTemplateGeneration": {
        "template_emd_number": "3420",
        "flip_handedness": true
    },
    "DynamoTemplateMatching": {
    },
    "TemplateMatchingPostProcessing": {
        "cc_std": 2.5
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 4,
        "use_noise_classes": true,
        "selected_classes": [1]
    },
    "DynamoAlignmentProject": {
        "iterations": 3,
        "classes": 3,
        "use_noise_classes": true,
        "selected_classes": [1],
        "box_size": 1.10,
        "binning": 4
    },
    "StopPipeline": {
    },
    "DynamoAlignmentProject": {
        "classes": 1,
        "iterations": 1,
        "use_noise_classes": false,
        "swap_particles": false,
        "selected_classes": [3],
        "binning": 4,
        "threshold":0.8
    },
    "DynamoAlignmentProject": {
        "classes": 1,
        "iterations": 1,
        "use_noise_classes": false,
        "swap_particles": false,
        "selected_classes": [1,2],
        "binning": 2,
        "threshold":0.9
    },
    "BinStacks":{
        "binnings": [1],
        "use_ctf_corrected_aligned_stack": false,
        "run_ctf_phaseflip": true
    },
    "Reconstruct": {
        "reconstruct": "unbinned"
    },
    "DynamoAlignmentProject": {
        "classes": 1,
        "iterations": 1,
        "use_noise_classes": false,
        "swap_particles": false,
        "selected_classes": [1,2],
        "binning": 1,
        "threshold":1
    }
}
```

# HIV1 (EMPIAR-10164)

First you need to get the data set from [here](https://www.ebi.ac.uk/empiar/EMPIAR-10164/).