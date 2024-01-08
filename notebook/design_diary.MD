# Design & Prototyping Diary
A diary logging the steps in the iterative instrument design and implementation process in Pure data.

## Oct/Nov 2023 - Initial design phase
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

---

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

#### Reflections and considerations
The p6_voice.pd sketch is acting both as the start of a "voice card" and also the overall synthesizer panel. At some point I have to separate those parts, by making a voice card abstraction to be cloned for each of the six voices and the overall GUI panel. I also have to consider which module is part of the voice card, and which is global. Maybe I should keep the Graph-On-Parent abstractions for the GUI, and move or abstract the logic inside to a voice card implementation.

---

## 12/12/2023

- Started working on the GUI layout and send/receive architecture of signals in the p6_voice.pd sketch


### Iteration v0.1 – Oscillators
- p6_voice plays a single voice (monophonic) with the following modules:
    - oscillator_1
    - oscillator_2
- Regular MIDI 'notein' changing the pitch of the oscillators only. No noteoff yet.


- Moved 'bit' selector from individual modules to global p6_voice
- Exchanged else/buttons (used for aesthetic purposes) with vanilla toggles, because of initialization issues.
- Reworked and added Mixer module
- Moved Sub-Oscillator to Oscillator 1 module for simplicity
- Added volume adjustment to mixer (sub-oct too low, noise too high)
- Swapped else/mix4~ with vanilla [*~] since linear volume control was preferred

- Reworked Low-Pass Filter module
    - Implemented bipolar Envelope amount (Range +/- 48 semitones)
        - Implemented Velocity modulation of Envelope amount
    - Implemented Keyboard Tracking (Off/Half/Full) with C4 as center frequency
- Added Low-Pass Filter to p6_voice

- Undid the volume adjustments to mixer after low-pass filter was added. The energy was in the (now tamed) high frequencies.
    - It seems that the preset output volume appear balanced when low-pass filtering is added, except when "open".

#### Reflections and considerations
I need to make a block diagram of the synthesizer design. This could be beneficial to me in the design process as well. At a later stage, a diagram is  needed for the complete instrument as well. Low Resonance takes the energy out of the sound. Look into that. The externals part of the "Pure Data: Electronic Music and Sound Design" book might address and/or solve this.

---

## 13/12/2023
- Added color-coding to inlets (yellow), receives (green) and outlets (purple) in p6_voice.pd
- Removed "hard-wired" inlets to modules, the connection between modules, which should be constant.
    - Only inlets controlling parameters (knobs/buttons) are now used.
- ** Found a bug with the Env_amount range (+/- 4 oct), which fades out the sound(?!) if every knob is at max.
    - Temporarily fix på reducing the range to +/- 3 oct

- Reworked and added the Amplifier Envelope module
    - Implemented Env Amount and Velocity modulation
    - A/D/R with 1-20000 ms range


### Iteration v0.2 – Basic Single Voice
- p6_voice plays a single voice (monophonic) with the following modules:
    - oscillator_1
    - oscillator_2
    - mixer
    - low-pass filter (mo filter_envelope modulation yet)
    - amplifier_envelope
- Regular MIDI 'notein' with 'noteoff' through amp envelope.


- Changed the filter-typ to [else/svfilter~] for better resonance response.
    - 2x 2-pole in series
    - Implementation of Chamberlin's state-variable filter algorithm
- Removed the resonance control of the second filter, it should only control the first in the series.
- Reworked and added Filter Envelope module
    - A/D/R with 1-20000 ms range
- Updated [pd reference] of every module

- Reworked the cutoff logic in the Low-Pass Filter module
- Implemented Filter Envelope modulation to Low-Pass Filter module


### Iteration v0.3 – Single Voice
- p6_voice plays a single voice (monophonic) with the following modules:
    - oscillator_1
    - oscillator_2
    - mixer
    - low-pass filter
    - filter_envelope (updated filter frequency logic and added envelope modulation)
    - amplifier_envelope
- Regular MIDI 'notein' with 'noteoff' through amp envelope.


#### Reflections and considerations
There is pops and glitches in sound if turning knobs when note is playing (specifically amp env amount). Consider adding lines to smooth parameter changes (slew/lag). With iteration v0.3, the synthesizer finally starts to feel expressive, although it is still monophonic, and played with a small MIDI keyboard. The next step is to add polyphony and MPE support.

---

## 14/12/2023
- Added Master Volume module
- Shelved an instance in a 'MIDI' folder, to mark the transition to MPE

- Implemented MPE through the pd_mpe external (still monophonic patch)
    - Reworked and renamed the [mpeTest] abstraction from *pd_mpe* to [mpe_voice]
        - Added copyright notice
    - Added sends for each parameter
- Added pitchbend support, with global PB range selector
- Added [glide2pitchbend] abstraction for glide control of frequency
- Tested 'slide' and 'press' (aftertouch) modulation
    - Slide: [-64, 63]
    - Press: [0, 127]
- Added preliminary modulation input for lpf cutoff frequency (to not override knob, but modulate relative to its position)
    - This has to be reworked for every parameter to be modulated


### Iteration v0.4 – MPE
- p6_voice is MPE-enabled and plays a single voice (monophonic) with the following modules:
    - oscillator_1
    - oscillator_2
    - mixer
    - low-pass filter
    - filter_envelope
    - amplifier_envelope
    - master_volume
- Tested with a ROLI Seaboard Block
- Glide simulates pitchbend
- Slide modulates Osc1 Pulse Width
    - Overrides knob
- Press modulates lpf cutoff frequency
    - Modulates relative to knob (not really, but relative to the modules inlet)


#### Reflections and considerations
I need to implement MPE objects into my synthesizer patch, as in map the individual MPE voices' output to my synthesizer parameters. First up is the 'glide', analogous to pitch-bend, which is a core functionality in MPE. The 'noteoff' architecture needs to adapt top the MPE object as well.

Pitchbend control is added to make use of the MPE 'glide' parameter. To enable for 'glide' modulation of other parameters as well, a [glide2pitchbend] abstraction was added. The other MPE control parameters 'slide' and 'press' (aftertouch) is also tested. Cutoff modulation works fine, Pulse Width modulation might have som artifacts. Still considering a [line] object to smooth parameter changes.

NB! The latest pd_mpe git activity was committed on Apr 29, 2019. The current build is for win/mac 'i386;x86_64' CPU architectures. This build does not support the new Apple M1/M2/M3 chips, which is in the UiO MCT Synthroom. Considering making a build for the 'arm64e' CPU architecture.

pd error log:
/Users/kristian/Documents/Pd/externals/mpe/mpe.pd_darwin:dlopen(/Users/kristian/Documents/Pd/externals/mpe/mpe.pd_darwin, 0x000A): tried: '/Users/kristian/Documents/Pd/externals/mpe/mpe.pd_darwin' (mach-o file, but is an incompatible architecture (have (x86_64), need (arm64e)))
/Users/kristian/Documents/Pd/externals/mpe/mpe.pd_darwin:dlopen(/Users/kristian/Documents/Pd/externals/mpe/mpe.pd_darwin, 0x000A): tried: '/Users/kristian/Documents/Pd/externals/mpe/mpe.pd_darwin' (mach-o file, but is an incompatible architecture (have (x86_64), need (arm64e)))
mpe: can't load library

---

## 19/12/2023
- Bugfix: Connecting [r~ osc2] to sync input of Oscillator 1
- Bugfix: MPE glide / pb_range typo
- Restructuring Oscillator 1 + Oscillator 2


### Iteration v0.4.1 – MPE (+ sync)
- Bugfix to Iteration v0.4
    - Sync input of Oscillator 1 is now connected to [r~ osc2]
- Restructured the Oscillator 1 + 2 modules with abstractions per function


- Started the process of separating the GUI from the logic of modules
    - Added a _GUI folder containing the GUI patches with the logic removed.
    - The old patches, is now without GUI and graph-on-parent, with just the logic.
    - The input scaling is happening in the GUI patch.
    - The GUI module patches are "global", on the layout of the main synthesizer patch.
    - The logic module patches are "local" per voice, to be called in each clone of the voicecard patch.
- Reworked Oscillator 1 (separated GUI + logic)
- Started with a [pd voicecard] subpatch, which later will be saved as a separate file.
- Reworked Oscillator 2 (separated GUI + logic)
- Reworked Mixer (separated GUI + logic)

#### Reflections and considerations
There has been a bug in the previous iterations, where the [r~ osc2] inlet was not connected to the sync-input of the oscillator on Oscillator 1. Because the Oscillator 2 is halfway up in the mix by default, I didn't notice the bug. Considered fixing for iteration 0.3 and 0.4, but decided on not to temper with previous iterations after they were finished. The bug will be fixed in iteration 0.5.
- Should Oscillator 2 level attenuate the signal to Oscillator 1 when in Sync mode?

---

## 20/12/2023
- Reworked Low-Pass Filter (separated GUI + logic)
- Reworked Filter Envelope (separated GUI + logic)
- Reworked Amplifier Envelope (separated GUI + logic)

- Because of the local $0-prefix to send/receive objects, I have to incorporate the modules logic to the voicecard abstraction.

- Reworked the voicecard abstraction to not call the synthesizer module logic abstractions, but copy/pasted this logic into subpatches instead.
- Added polyphonic support to the synthesizer!


### Iteration v0.5 – Polyphony (global modulation)
- The synthesizer now works with polyphonic note input
- Every voice currently modulates every other voice (except pitch glide)
    - Polyphonic synthesizer with "monophonic modulation"
    - The modulation is currently done in the GUI module abstractions, and needs to be moved to the logic subpatches of the voicecard abstraction.


#### Reflections and considerations
The synthesizer is now polyphonic, which brings up another consideration regarding the modulation. I've thought about that it is not necessary to have visual feedback of the knob of a parameters modulation. But now I see that it is not wanted, as the modulation needs to be individual per voice. There might be wish for global modulation through a future LFO, but that will be reflected upon at a later stage. For now, I have to move the modulation of parameters to the synthesizer module logic inside the voicecard.pd abstraction. This also means that there will be need for a separate scaling of the modulation input range, as the scaling is now happening in the GUI patch to make the knobs work in higher resolution than the MIDI 7-bit range. (This was already an conscious choice in making the synthesizer more future-proof and compatible with higher resolution MIDI 2.0, OSC or 14-bit CC.) The scaled modulation input signal will be added/subtracted to the "knob" control signal. There should possibly also be a "depth" control per modulation source or destination.

Considering making a matrix for modulation routing!

---

## 21/12/2023
- Moved the modulation logic from the main patch into the voicecard.pd abstraction to make the modulation polyphonic (individual modulation per voice)
- Added a small modulation matrix
    - Sources: Slide / Press
    - Destinations: Off / Osc1 PW / LPF Cutoff
    - Added depth control for each modulation (source)

### Iteration v1 – It's alive!
- A 6-voice polyphonic synthesizer with MPE and per-voice modulation!
- There is a small modulation matrix for flexible modulation routing between a few sources and destinations
- Synth modules:
    - oscillator_1
    - oscillator_2
    - mixer
    - low-pass filter
    - filter_envelope
    - amplifier_envelope

---

## 04/01/2024

#### Reflections and considerations
The first "official" v1-iteration is tailored to the ROLI Blocks. This explains the Slide/Press naming of modulation sources, but which easily can be interchanged with the y-axis control and surface area control parameters of the TouchKeys. *The naming convention should probably be changed in a later iteration, to more "neutral" controller-agnostic labels.*

The Modulation Matrix is great for testing individual parameters, both sources and destinations for modulation. It is also "performer-friendly" in that it is easy to route/map the modulation sources to a synthesizer parameter and set its depth. However, this negates the possibility of having one modulation source affect multiple destinations. *The modulation matrix affords One-To-One and Many-to-One mapping strategies, but not One-to-Many*. One solution to this could be to have two or more instances of each source (with individual depth-knobs). This could work with only a few modulation sources (or DoF of the instrument) in use, but I suspect it would become messy when adding more modulation sources. This limitation furthermore hinders the making of a higher-level intermediate abstract mapping layer. Such an intermediate layer would be beneficial for the designer--performer, enabling custom instrument iterations where a saved pd patch instance of the synthesizer could be the equivalent of a synthesizer sound patch/preset. Such an intermediate layer would be "messier" to implement, aesthetically speaking, than a transparent and clean modulation matrix, and should at least be placed in a dedicated sub-patch "environment". A modulation matrix is as mentioned a great tool when designing and prototyping new features. As for the performer-friendliness, I think this would make the instrument more available to a wide selection of performer--users, enabling instant, although limited, expressivity, by affording the possibility to set the modulation matrix to their individual preferences. It is also easy to test different ideas out, in a "one-dimensional" sense, with the restriction of using a source only once, while any destination parameter potentially could be used for every source in use.

---

## 08/01/2024

#### Reflections and considerations
The modulation logic needs to be changed so that the response is either unipolar or bipolar according to the design of the control source parameter. *As of now, the slide control modulation (which by default is routed to Oscillator 1 PW) is unipolar, i.e., only adding positive values to the modulation.* The slide parameter feels on the other hand to be inherently bipolar, centering around a middle value (64), with the possibility to slide values up or down. As of now, where the slide control only adds positive values to the PW modulation, this only affords a full modulation range if the Osc1 PW knob is set to 0. (Which in turn would "jump" the PW modulation to around the middle value for each time a note is pressed, if the depth is set to 100%.) Instead, the slide control parameter signal should be transformed to center around 0 [-64, 63], to be a bipolar modulation source. *The press modulation on the other hand (which by default "opens" up the low-pass filter by increasing the cutoff frequency), is inherently unipolar and feels right as it currently is set up.* The default value of a press is 0, and pressing harder (using the polyphonic aftertouch CC) adds to this value.

**Care and consideration has to be taken in setting up the different control parameters and treating their data before routed to their modulation destination, either unipolar or bipolar.** To implement this, I'm making sub-patches to transform the bipolar control sources from [0, 127] to [-64, 63]. The unipolar control sources are used as is, as adding the value of the knob controlling the destination synthesizer parameter with the control source value was already the design choice used. This addition will (when depth is set to 100%) in effect range from subtracting 50% to adding 50% for bipolar modulation, and range from adding 0% to 100% for unipolar modulation.

Regarding the actual range of the modulation, every synthesizer parameter value gets clipped to a certain range depending on the type of parameter. This means that, e.g., having a low-pass filter frequency already set to max before being modulated by the press source would not add more or otherwise affect the frequency. In a future bipolar modulation design, the Osc1 PW knob set to the minimum value before being modulated negatively by the slide source would similarly not affect the PW --- only and upwards slide adding a positive value would. 

I could consider adding the depth ranging from [-1, 1] instead of [0, 1] to make the unipolar modulation be negative as well. But on the other hand, this negative-to-positive modulation possibility is already a hard-coded design feature of the ENV AMOUNT ot the filter(s) and amplifier envelope. Maybe keeping the unipolar positive, and otherwise only letting the bipolar sources modulate "negatively" would more than suffice.

---

