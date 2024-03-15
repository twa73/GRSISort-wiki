# Command-Line Options

## GRSISort

| Flag and Alternatives | Explanation |
|-----------------------|-------------|
| -h -? or --help       | Prints a help message with all command-line options and short explanations. |
| -v --version          | Show the version of GRSISort. This is based on the latest "tag" created on GitHub, depending on how long ago that was created your version could differ quite a lot from the tagged version. For get a better idea of how new your version of GRSISort is, use the ```git log -n 1``` command inside the ```$GRSISYS``` directory. |
| --build-window arg    | Set the size of the build window using the argument in ns. This determines the maximum time difference between hits to be put into one event. Depending on the ```--static-window``` flag this is either the time difference between the first and the last hit in the event (static window), or between two subsequent hits in the event (moving window). This should be set large enough to get a nice time-random background but if this is too large it can slow down the sorting because the multiplicities of the events get too large (especially with the moving window and a high data rate). |                                                    
| --build-events-by-timestamp | Build events by timestamp w/o using CFD. |
| --addback-window  arg | Addback window, time in ns. |
| --suppression-window  arg | BGO suppression window, time in ns. |
| --suppression-energy  arg | Minimum BGO energy for suppression. |
| --static-window       | Use static window for event building. |
| --waveform-fitting    | Fit waveforms using SFU algorithms. |
| --is-correcting-cross-talk | Correct cross-talk. |
| --recommended         | Use recommended flags. |
| --output-fragment-tree  arg | Filename of output fragment tree. |
| --output-analysis-tree  arg | Filename of output analysis tree. |
| --output-fragment-hists  arg | Filename of output fragment hists. |
| --output-analysis-hists  arg | Filename of output analysis hists. |
| -a                    | Make the analysis tree. |
| -H --histos           | Attempt to run events through MakeHisto lib. |
| -g --start-gui        | Start the gui at program start. |
| -b --batch            | Run in batch mode. |
| --sort-depth  arg     | Number of events to hold when sorting by time/trigger_id. |
| -q --quit             | Quit after completing the sort. |
| -l --no-logo          | Inhibit the startup logo. |
| -w --extract-waves    | Extract wave forms to data class when available. |
| -d --debug            | Write debug information to output/file, e.g. enables writing of TDescantDebug at analysis stage. |
| --write-diagnostics   | Write Parsing/SortingDiagnostics to root-file. |
| --word-count-offset  arg | Offset to the word count in the GRIFFIN header word, default is -1 (disabled). |
| --log-errors          | |
| --reading-material    | |
| --write-fragment-tree --write-frag-tree | Write fragment tree. |
| --bad-frags --write-bad-frags --bad-fragments --write-bad-fragments | Write fragments that failed parsing to BadFragmentTree. |
| --separate-out-of-order | Write out-of-order fragments to a separate tree at the sorting stage. |
| --ignore-odb          | |
| --ignore-odb-channels | |
| --downscaling  arg    | Downscaling factor for raw events to be processed. |
| --ignore-epics        | |
| --ignore-scaler       | |
| --suppress-error --suppress-errors --suppress_error --suppress_errors | Suppress error output from parsing. |
| --reconstruct-timestamp --reconstruct-time-stamp | Reconstruct missing high bits of timestamp. |
| --ignore-missing-channel | Ignore missing channels completely (not written to fragment or analysis tree). |
| --skip-input-sort     | Skip sorting fragments before building events (default is false). |
| --fragment-size  arg  | Size of fragment write queue. |
| --analysis-size  arg  | Size of analysis write queue. |
| --column-width  arg   | Width of one column of status. |
| --status-width  arg   | Number of characters to be used for status output. |
| --status-interval  arg| Seconds between each detailed status output (each a new line), non-positive numbers mean no detailed status. |
| --write-clients  arg  | Number of clients used to write analysis tree. |
| --max-events  arg     | Maximum number of (midas, lst, rlmd, or tdr) events read. |
