This page contains tomoBEAR tutorials which should showcase the capabilities of the processing pipeline. These tutorials assume that you have already cloned the [tomoBEAR github repository](https://github.com/KudryashevLab/tomoBEAR), installed all the required software ans setup tomoBEAR.

# Ribosome (EMPIAR-10064)

As a first data set to showcase the capabilities of tomoBEAR we have chosen the Ribosome data set with the number 10064 i the EMPIAR database.

You can get the data set from [here](https://www.ebi.ac.uk/empiar/EMPIAR-10064/). In our case we used just the mixedCTEM data and achieved 11.2Å in resolution with ~3.5k particles. If you want you can additionally use the CTEM data to be able to pick more particles.

After downloading the data extract it in a folder of your choice. One thing one should note about this data is that

* the data is already motion corrected
* the stacks are already assembled
* the pixel size is not in the header
* the tilt angles are not provided

Because of such circumstances which sometimes occur tomoBEAR is able to inject this data along with the JSON which describes the processing pipeline.

If you have already cloned the [tomoBEAR github repository](https://github.com/KudryashevLab/tomoBEAR) to your local machine you can find in the configurations folder a file called `ribosome_empiar_10064_dynamo.json`. This file describes the processing pipeline which should be setup by tomoBEAR to process this data set.

The following paragraphs will explain the changes you need to do to the JSON file to be able to run tomoBEAR on your local machine. In the end of this chapter the whole JSON file is shown.

First you need to show tomoBEAR the path to the data

`"general": {`  
`    "project_name": "Ribosome",`  
`    "project_description": "Ribosome EMPIAR 10064",`  
`    "data_path": "/path/to/ribosome/data/*.mrc",`  
`    "processing_path": "/path/to/processing/folder",`  
`    "expected_symmetrie": "C1",`  
`    "apix": 2.62,`  
`    "tilt_angles": [-60.0, -58.0, -56.0, -54.0, -52.0, -50.0, -48.0, -46.0, -44.0, -42.0, -40.0, -38.0, -36.0, -34.0, -32.0, -30.0, -28.0, -26.0, -24.0, -22.0, -20.0, -18.0, -16.0, -14.0, -12.0, -10.0, -8.0, -6.0, -4.0, -2.0, 0.0, 2.0, 4.0, 6.0, 8.0, 10.0, 12.0, 14.0, 16.0, 18.0, 20.0, 22.0, 24.0, 26.0, 28.0, 30.0, 32.0, 34.0, 36.0, 38.0, 40.0, 42.0, 44.0, 46.0, 48.0, 50.0, 52.0, 54.0, 56.0],`  
`    "gold_bead_size_in_nm": 9,`  
`    "template_matching_binning": 8,`  
`    "binnings": [2, 4, 8],`  
`    "reconstruction_thickness": 1400,`  
`    "as_boxes": false`  
`}`  



Here comes the full JSON file to setup the processing pipeline in tomoBEAR and process the data

`{   
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
}`

# HIV1 (EMPIAR-10164)

First you need to get the data set from [here](https://www.ebi.ac.uk/empiar/EMPIAR-10164/).