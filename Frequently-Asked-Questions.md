Here you can find frequently asked questions which appear from time to time when you process data with tomoBEAR:

1. What can I do if tomoBEAR outputs "out of memory" during execution of the module DynamoTiltSeriesAlignment?
* If you are running the module in parallel execution mode set the parameter `execution_method` in the `DynamoTiltSeriesAlignment` block to `sequential`. Then the execution will be slower because only one tilt stack will be processed at a time but the memory consumption will be less.