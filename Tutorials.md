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
This can take a while. It depends on your infrastructure and setup. After tomoBEAR stops you caninspect the fiducial model in the folder of ```batchruntomo``` which you can find in your processing folder.

```shell
cd /path/to/your/processing/folder/5_BatchRunTomo_1
```

Now you can inspect the alignment of every tilt stack one after the other and can possibly refine it if needed. For that you can use the following command. Please replace ```xxx``` with the tomogram number you want to inspect.

```shell
etomo tomogram_xxx/*.edf
```
When ```etomo``` starts just chose the ```fine alignment``` step which should be lila if everything went fine for that tomogram and then click on ```edit/view fiducial model``` to start ```3dmod``` with the right options to be able to refine the gold beads. Before you start to refine just press the arrow up button in the top left corner of the window with the viewport. To refine the gold beads click on ```Go to next big residual``` in the window with the stacked buttons from top to bottom and theview in the viewport window should change immediately to the location of a gold bead with a big residual. Now see if you can center the marker better on the gold bead with the right mouse button. It is important that you don't put it on the peak of the red arrow but center it on the gold bead. When you are finished with this gold bead just press again on the ```Go to next big residual``` button. After you are finished with recentering the marker on the gold beads you need to press the ```Save and run tiltalign``` button.

After you finished the inspection of all the alignments you can start tomoBEAR again as previously and it will continue from where it stopped up to the nex ```StopPipeline``` section.

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