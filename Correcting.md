#Correcting Data
##Removing dumped GRIF-16 events <a name="bufferclear" />
The GRIF-16's add extraneous events to the start of every run. To remove these, the **bufferclean** utility can be used. This script looks for the first time a digitizer has a Channel Trigger Id < some threshold and removes all events from this digitizer that come before that. The threshold is currently set to 50. To use **bufferclean** type: 
```
bufferclean name_of_midas_file.mid name_of_output.mid
```
The output midas file should have the events properly cleared.

##Fixing GRIF-16 Timestamp Offsets <a name="offsetfix" />
Sometimes, the GRIF-16 digitizers do not properly reset their timestamps at the start of a new run. To fix the timestamps, you must first make sure you have cleaned out the [extra buffer events](#bufferclear) at the start of the Midas File. The fix the offsets, use **offsetfix**.
```
offsetfix name_of_midas_file.mid name_of_output.mid
```
The script works by using the [offsetfind](#offsetfind) algorithm, and shifting all of the timestamps by the found time shifts. It then outputs a corrected Midas file. To check that the finding script found the correct timestamp shift, you can look at **time_diffs*runnumber*.root**. The histograms labeled *timediff* are the final peak found after employing the [offsetfind](#offsetfind) algorithm. It should be noted that this also shifts the current offset between 4G's and GRIF-16's from their current 800 ns shift to 0 ns.

##Finding GRIF-16 Timestamp Offsets <a name="offsetfind" />
Sometimes, the GRIF-16 digitizers do not properly reset their timestamps at the start of a new run. To find the timestamp offset from each address, the **offsetfind** algorithm can be used. This is a script in it's own right, but is mostly useful for checking that the [offsetfix](#offsetfix) script worked properly.