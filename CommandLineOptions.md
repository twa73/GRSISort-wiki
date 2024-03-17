# Command-Line Options

## GRSISort

| Flag and Alternatives | Explanation |
|-----------------------|-------------|
| -h -? or --help       | Prints a help message with all command-line options and short explanations. |
| -v --version          | Show the version of GRSISort. This is based on the latest "tag" created on GitHub, depending on how long ago that was created your version could differ quite a lot from the tagged version. For get a better idea of how new your version of GRSISort is, use the ```git log -n 1``` command inside the ```$GRSISYS``` directory. |
| --build-window arg    | Set the size of the build window using the argument in ns. This determines the maximum time difference between hits to be put into one event. Depending on the ```--static-window``` flag this is either the time difference between the first and the last hit in the event (static window), or between two subsequent hits in the event (moving window). This should be set large enough to get a nice time-random background but if this is too large it can slow down the sorting because the multiplicities of the events get too large (especially with the moving window and a high data rate). |                                                    
| --build-events-by-timestamp | Build events by timestamp w/o using CFD. |
| --addback-window  arg | Addback window, time in ns. This is the maximum time for two hits two be considered to be added together for Addback. |
| --suppression-window  arg | BGO suppression window, time in ns. If a BGO that fulfills the other conditions fires within this time of a HPGe, the hit from that HPGe is suppressed. |
| --suppression-energy  arg | Minimum BGO energy for a BGO hit to be considered for suppression. |
| --static-window       | Use static window for event building. See the explanation of ```--build-window``` for the difference between a static and a moving build window. |
| --waveform-fitting    | Fit waveforms using SFU algorithms. |
| --is-correcting-cross-talk | Correct cross-talk. This causes any cross-talk coefficents from the calibration file to be applied to HPGe hits in the same detector. |
| --recommended         | Use recommended flags. |
| --output-fragment-tree  arg | Filename of output fragment tree. |
| --output-analysis-tree  arg | Filename of output analysis tree. |
| --output-fragment-hists  arg | Filename of output fragment hists. |
| --output-analysis-hists  arg | Filename of output analysis hists. |
| -a                    | Make the analysis tree. |
| -H --histos           | Attempt to run events through MakeHisto lib. This is pretty much depreceated, grsiframe is the recommended way to create histograms. |
| -g --start-gui        | Start the gui at program start. |
| -b --batch            | Run in batch mode. |
| --sort-depth  arg     | Number of events to hold when sorting by time/trigger_id. This is the size of the container used to hold all hits (sorted by either trigger ID or time/timestamp). Once the container is full, every time we add a new hit to the container, we remove the first one and either add it to the existing event (if it fits the build condition), or push the existing event to the output queue and create a new event with this hit in it. If the hit we removed from the queue is earlier than the earliest hit in the event, it is determined to be out of order and put into a separate tree (if that option is enabled). This also triggers a warning at the very end of the sort, displaying the largest time difference between this hit and the first one in the event, plus a suggestion to increase the size of the container using this flag. The suggested sizee if very conservative (i.e. large) and can actually cause the program to use too much memory. So instead of just copying that value, some evaluation is advised. |
| -q --quit             | Quit after completing the sort or after running a .C-script provided on the command line. |
| -l --no-logo          | Inhibit the startup logo. |
| -w --extract-waves    | Extract wave forms to data class when available. |
| -d --debug            | Write debug information to output/file, e.g. enables writing of TDescantDebug at analysis stage. |
| --write-diagnostics   | Write Parsing/SortingDiagnostics to root-file. |
| --word-count-offset  arg | Offset to the word count in the GRIFFIN header word, default is -1 (disabled). Unless you are sorting very old data or know exactly what you are doing this should remain disabled. |
| --log-errors          | This causes the data parser to print errors (unless ```--suppress-errors``` is used) to a file called "error.log". |
| --reading-material    | Opens a random wikipedia article for you to read. |
| --write-fragment-tree --write-frag-tree | Write fragment tree. |
| --bad-frags --write-bad-frags --bad-fragments --write-bad-fragments | Write fragments that failed parsing to a separate "BadFragmentTree" in fragment file. |
| --separate-out-of-order | Write out-of-order fragments to a separate tree "OutOfOrderTree" at the sorting stage to the analysis file. |
| --ignore-odb          | Ignore any and all information from the ODB. |
| --ignore-odb-channels | Ignore the channel information from the ODB (but use all other information like the cycle used). |
| --downscaling  arg    | Downscaling factor for raw events to be processed. Only intended for online analysis when the data rate is high, though in most cases it makes more sense to sort a limited number of subruns instead of using the downscaling factor. |
| --ignore-epics        | Ignore the data from epics channels in the midas file. |
| --ignore-scaler       | Ignore the data from scalers in the midas file. |
| --suppress-error --suppress-errors --suppress_error --suppress_errors | Suppress error output from parsing. If this flag isn't used a lot of output will be written to the terminal, so redirecting the output to file is advised (append ``` 2>&1 > logfile.log``` to write to a file called "logfile.log"). |
| --reconstruct-timestamp --reconstruct-time-stamp | Reconstruct missing high bits of timestamp. Sometimes the word with the high bits of the timestamp is dropped during the transmission of the data from the digitizer to the file. This flag allows GRSISort to reconstruct the high bits based on the high bits of the previous timestamps for this channel. This is only done if a missing high timestamp word is the only error encountered for this fragment. |
| --ignore-missing-channel | Ignore missing channels completely (not written to fragment or analysis tree). Hits from channels missing from the .cal-file are never written to the analysis file (since the information which detector this channel represents is missing), but this flags causes these hits also to not be written to the fragment file! |
| --skip-input-sort     | Skip sorting fragments before building events (default is false). Can be used for datasets where the data coming from the digitizers is already sorted in time. |
| --fragment-size  arg  | Size of fragment write queue. This is the size of the queue used to store fragments that are waiting to be written to file. Optimizing this can speed up the sorting in some cases. |
| --analysis-size  arg  | Size of analysis write queue. This is the size of the queue used to store events that are waiting to be written to file. Optimizing this can speed up the sorting in some cases. |
| --column-width  arg   | Width of each column of the status output. |
| --status-width  arg   | Number of characters to be used for status output. Any columns that do not fit within this width are simply not shown. |
| --status-interval  arg| Seconds between each detailed status output (each a new line), non-positive numbers mean no detailed status. |
| --write-clients  arg  | Number of clients used to write analysis tree (deprecated). Older versions of GRSISort had the option to use multiple threads writing events to a TMemFile that would then be merged into one single file. This was found to lead to issued with single events being written thousands of times, resulting in spikes in energy or timing spectra. |
| --max-events  arg     | Maximum number of (midas, lst, rlmd, or tdr) events read. This can be used to debug or test things. |

## GRSIFrame

| Flag and Alternatives | Explanation |
|-----------------------|-------------|
| -h -? or --help       | Prints a help message with all command-line options and short explanations. |
| -v --version          | Show the version of GRSISort. This is based on the latest "tag" created on GitHub, depending on how long ago that was created your version could differ quite a lot from the tagged version. For get a better idea of how new your version of GRSISort is, use the ```git log -n 1``` command inside the ```$GRSISYS``` directory. |
| --build-window arg    | Set the size of the build window using the argument in ns. This determines the maximum time difference between hits to be put into one event. Depending on the ```--static-window``` flag this is either the time difference between the first and the last hit in the event (static window), or between two subsequent hits in the event (moving window). This should be set large enough to get a nice time-random background but if this is too large it can slow down the sorting because the multiplicities of the events get too large (especially with the moving window and a high data rate). |                                                    
| --build-events-by-timestamp | Build events by timestamp w/o using CFD. |
| --addback-window  arg | Addback window, time in ns. This is the maximum time for two hits two be considered to be added together for Addback. |
| --suppression-window  arg | BGO suppression window, time in ns. If a BGO that fulfills the other conditions fires within this time of a HPGe, the hit from that HPGe is suppressed. |
| --suppression-energy  arg | Minimum BGO energy for a BGO hit to be considered for suppression. |
| --static-window       | Use static window for event building. See the explanation of ```--build-window``` for the difference between a static and a moving build window. |
| --waveform-fitting    | Fit waveforms using SFU algorithms. |
| --is-correcting-cross-talk | Correct cross-talk. This causes any cross-talk coefficents from the calibration file to be applied to HPGe hits in the same detector. |
| --max-workers arg | Maximum number of threads used when running a grsiframge session. |
| --tree-name arg | Name of the input tree to be used, default is empty, i.e. FragmentTree, and AnalysisTree are tried as input trees. If neither is found (or no tree with the given name is found), an error message is generated and grsiframe exits. |
| -d --debug | Increases verbosity of the underlying RDataFrame and also turns off the progress bar. |

## GRSIProof - DEPRECATED!

| Flag and Alternatives | Explanation |
|-----------------------|-------------|
| -h -? or --help       | Prints a help message with all command-line options and short explanations. |
| -v --version          | Show the version of GRSISort. This is based on the latest "tag" created on GitHub, depending on how long ago that was created your version could differ quite a lot from the tagged version. For get a better idea of how new your version of GRSISort is, use the ```git log -n 1``` command inside the ```$GRSISYS``` directory. |
| --build-window arg    | Set the size of the build window using the argument in ns. This determines the maximum time difference between hits to be put into one event. Depending on the ```--static-window``` flag this is either the time difference between the first and the last hit in the event (static window), or between two subsequent hits in the event (moving window). This should be set large enough to get a nice time-random background but if this is too large it can slow down the sorting because the multiplicities of the events get too large (especially with the moving window and a high data rate). |                                                    
| --build-events-by-timestamp | Build events by timestamp w/o using CFD. |
| --addback-window  arg | Addback window, time in ns. This is the maximum time for two hits two be considered to be added together for Addback. |
| --suppression-window  arg | BGO suppression window, time in ns. If a BGO that fulfills the other conditions fires within this time of a HPGe, the hit from that HPGe is suppressed. |
| --suppression-energy  arg | Minimum BGO energy for a BGO hit to be considered for suppression. |
| --static-window       | Use static window for event building. See the explanation of ```--build-window``` for the difference between a static and a moving build window. |
| --waveform-fitting    | Fit waveforms using SFU algorithms. |
| --is-correcting-cross-talk | Correct cross-talk. This causes any cross-talk coefficents from the calibration file to be applied to HPGe hits in the same detector. |
| --max-workers arg     | Maximum number of threads used when running a grsiframge session. |
| --selector-only       | Turns off PROOF to run a selector on the main thread. |
| --log-file  arg       | Name of the file logs from grsiproof are written to. |
| --tree-name  arg      | Name of tree to be proofed, default is empty, i.e. FragmentTree, AnalysisTree, and Lst2RootTree are checked. |
| --average-rate        | Use average rate instead of current rate for status bar. |
| --parallel-unzip      | Use parallel unzipping of input files. |
| --cache-size  arg     | Set tree cache size (default = -1 = off). |
| --sub-mergers  arg    | Use sub mergers to merge result from workers (default = -1 = off, 0 = automatic number of mergers). |
| --proof-stats         | Enable PROOF stats. |
| --max-events  arg     | Maximum number of (midas, lst, rlmd, or tdr) events read. |

