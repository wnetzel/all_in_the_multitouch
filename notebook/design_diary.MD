# Design & Prototyping Diary
A diary logging the steps in the iterative synthesizer design and implementation process in Pure data.

## Oct/Nov 2023 - Initial design
- Started the process of designing the synthesizer in Pd, inspired by the Sequential Prophet-6
- Chose which modules to implement and which to leave out
- Begun thinking about how to implement features in Pd
- Prototyped abstractions of modules from the Prophet 6 with GUIs:
    - oscillator_1
    - oscillator_2
    - mixer
    - low-pass filter
    - high-pass filter
    - filter_envelope
    - amplifier_envelope
- Oscillator 1 + 2 makes sound with features resembling the Prophet 6

## 11/12/2023
- Started a p6_voice.pd sketch, making a GUI with the module abstractions
- Reworked Oscillator 1 + 2
    - Added Graph-On-Parent GUI feature
    - Reworked the logic and input rescaling of parameters
    - Added a semi-global "bit" selector for future use with MIDI 2.0 or testing/comparing with 14-bit CC or OSC
        - Reworked scaling objects to receive "bit" variable input
- Tested with a Akai LPK25 MIDI keyboard
    - Mockup MPE through the TouchKeys software works with Pd
    - We have sound in the oscillator 1 + 2 modules. 

Reflections and considerations:
The p6_voice.pd sketch is acting both as the start of a "voice card" and also the overall synthesizer panel. At some point I have to separate those parts, by making a voice card abstraction to be cloned for each of the six voices and the overall GUI panel. I also have to consider which module is part of the voice card, and which is global. Maybe I should keep the Graph-On-Parent abstractions for the GUI, and move or abstract the logic inside to a voice card implementation.

## 12/12/2023

- Started working on the GUI layout and send/receive architecture of signals in the p6_voice.pd sketch

### Iteration v0.1
- p6_voice plays a single voice with the following modules:
    - oscillator_1
    - oscillator_2
- Regular MIDI 'notein' changing the pitch of the oscillators only. No noteoff.


