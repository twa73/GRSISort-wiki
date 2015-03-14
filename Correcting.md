#Correcting Data
##Removing dumped GRIF-16 events
The GRIF-16's add extraneous events to the start of every run. To remove these, the **bufferclean** utility can be used. This script looks for the first time a digitizer has a Channel Trigger Id < some threshold and removes all events from this digitizer that come before that. The threshold is currently set to 50. To use **bufferclean** type: 
```
bufferclean name_of_midas_file.mid name_of_output.mid
```
The output midas file should have the events properly cleared.