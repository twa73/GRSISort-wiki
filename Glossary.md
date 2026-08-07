# Glossary of terms used in GRSISort

### Addback

Addback is a method for clover detectors where the full energy of a gamma ray can be recovered even if Compton scattering between different crystals of the detector occurs. 
To this end the energies of all crystals within a detector are summed together (with a timing condition applied).

### Build Window

GRSISort sorts hits from different electronic channels into events based on their time information and the build window length settings as well as a flag that determines whether the build window is static or extending.
The former means that the build window length is always measured from the earliest hit in the event, i.e. the maximum time difference between two hits in an event can be the length of the build window.
An extending build window on the other hand will measure its length from the latest hit in the event, i.e. the maximum time difference between two hits in an event can be longer than the length of the build window, and depending on the data rate of the experiment and the build window length used a single event could span a whole run.

### Crystal

We often call a single HPGe crystal a “crystal" to distinguish it from “detector” which refers to the clover detector made up from the four HPGe crystals.

### Event

An event is a collection of hits that are all within a certain time window from each other.
Events are created in GRSISort based on a build window length and a flag that determines whether this build window is static or extending.
The events are written as separate entries to the analysis trees.

### Hit

A hit is a single data set (typically energy and time) from one electronic channel.
Multiple hits can be combined to form events, and some detectors generate multiple coincident hits from one interaction (e.g. DSSD detectors will have a hit from the front and one from the back side of the detector, and TIGRESS detectors will have a signal from the core contact together with one or more segments).

### Matrix

We often refer to histograms with 2 dimensions as matrices.

### Single crystal method

The single crystal method is using the energy of a single HPGe detector, but with the additional condition (compared to the normal singles spectra) that no other crystal in the same clover detector fired at the same time. This essentially turns the other crystals of the same detector into additional Compton suppressors. The easiest way to implement this method is to use addback hits, but reject any where the number of “fragments” in the addback hit is not 1.

### Suppression

The GRIFFIN and TIGRESS HPGe detectors are surrounded by BGO anti-Compton shields that can be used to veto or suppress events where a gamma ray Compton scatters in a HPGe detector and then is detected in the BGO scintillator.
The standard suppression scheme used for GRIFFIN and TIGRESS vetoes any HPGe hits that have a BGO shield of the same detector fire within a certain time frame (default is +- 300 ns).

### Time Random

Time Random means two (or more) hits that are from separate physics events (like two different nuclei decaying or two different beam particles interacting with the target).
The time difference between random physics events forms in general an exponential distribution with a maximum at zero, and extending to infinity with a decay constant to is linked to the average event rate. 
In reality this exponential distribution will quite often appear to the constant if the event rate is sufficiently small compared to the length of the build window.
Since the distribution starts at zero time difference it is impossible to separate time random events from true coincident events by a simple gate on time differences. 
Instead we have to estimate the contribution of time random events to a gate on the prompt peak of the time distribution by comparing events within this prompt gate to a timing gate on purely time random events.
This is also the reason we try to have a build window of at least a microsecond, so that we include enough time random events to achieve decent statistics for the time random histograms.
