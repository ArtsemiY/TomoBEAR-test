This page contains tomoBEAR tutorials which should showcase the capabilities of the processing pipeline.

# Ribosome (EMPIAR-10064)

As a first data set to showcase the capabilities of tomoBEAR we have chosen the Ribosome data set with the number 10064 i the EMPIAR database.

You can get the data set from [here](https://www.ebi.ac.uk/empiar/EMPIAR-10064/). In our case we used just the mixedCTEM data and achieved 11.2Å in resolution with ~3.5k particles. If you want you can additionally use the CTEM data to be able to pick more particles.

After downloading the data extract it in a folder of your choice. One thing one should note about this data is that

* the pixel size is not in the header
* the tilt angles are not provided

Because of such circumstances which sometimes occur tomoBEAR is able to inject this data along with the JSON which describes the processing pipeline.

If you have already cloned the github repository of tomoBEAR to your local machine you can find in the configurations folder a file called `ribosome_empiar_10064_dynamo.json`. This file describes the processing pipeline which should be setup by tomoBEAR to process this data set.


# HIV (EMPIAR-10164)

First you need to get the data set from [here](https://www.ebi.ac.uk/empiar/EMPIAR-10164/).