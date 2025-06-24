# Exploring Multi-Touch Instruments With MPE and MIDI 2.0
A repository for my thesis in the Master's program "Music, Communication and Technology" at the University of Oslo.

## Code
 Contains **P-6**, my Pure data synthesizer inspired by the [Sequential Prophet-6](https://www.sequential.com/product/prophet-6/).

Dependencies:
 - [Pure Data](https://puredata.info/) (Version 0.54-1 or newer)
 - The [ELSE](https://github.com/porres/pd-else/) library for Pure Data (Version 1.0-0 RC-10 or newer)
 - The [pd_mpe](https://github.com/DanielRudrich/pd_mpe) Pure Data external for MIDI Polyphonic Expression (MPE) support

The synthesizer contains a modified version of a Pure data abstraction for parsing MPE data found in the pd_mpe external by [DanielRudrich](https://github.com/DanielRudrich/), released under the GPL-3.0 license.

## Video Demonstration
Video demonstrations are found in the associated [OSF repository](https://doi.org/10.17605/OSF.IO/JFMCV).

## Known Issues
As the last build of the [pd_mpe](https://github.com/DanielRudrich/pd_mpe) external is from April 2019, it will not run on Apple silicon (M-series) processors. Please reach out if you could help with compiling this external for the arm64 architecture.
