# Oscillator 1
- else/bl.vsaw~
- else/bl.square~

## Frequency
- Base Oscillator Frequency
    - 9-octave range
        - NRPN Value: 0-60 (?!)
    - semitone-steps
    - 16Hz - 8kHz

## Sync
- Hard Sync (Slave) to Oscillator 2 (Master)

## Shape
- 0% - 50%: Change width of else/bl.vsaw~ from 0.5 to 1.0
- 50% - 100%: Crossfade between else/bl.vsaw~ and else/bl.square~

## Pulse Width
- Change Pulse width of else/bl.square~ from 0.03 to 0.97


---


# Oscillator 2
- else/bl.vsaw~
- else/bl.square~


## Frequency
- Base Oscillator Frequency
    - 9-octave range
        - NRPN Value: 0-60 (?!)
    - semitone-steps
    - 16Hz - 8kHz

## Fine
- Fine tune control
- Range: Quarter-tone up/down (50 cents = 1/2 semitone)
- Steps: Cents

## Shape
- 0% - 50%: Change width of else/bl.vsaw~ from 0.5 to 1.0
- 50% - 100%: Crossfade between else/bl.vsaw~ and else/bl.square~

## Pulse Width
- Change Pulse width of else/bl.square~ from 0.03 to 0.97

## Low Freq
- Switch from OSC to LFO "mode"
- OSC range: 16Hz - 8kHz
- LFO range: 0.1Hz - 50Hz

## Keyboard
- Switch from free-running to keyboard-controlled OSC


---

# Slop

## Slop Mode
- Randomized detuning of oscillator (per voice)

- One LFO per voice
    - Triangle wave
    - Rate: X.XX Hz
    - Amount: +/- 0 - 0.XX semitones

## Vintage Mode
- As in Slop mode, plus:
- Randomized offset of ADSR stages of filter and amplifier envelopes (per voice)
    - Attack: +/- 0 - 0.XX seconds
    - Decay: +/- 0 - 0.XX seconds
    - Sustain: +/- 0 - 0.XX percent
    - Release: +/- 0 - 0.XX seconds


---


# Mixer

## Oscillator 1
- Oscillator 1 output level

## Oscillator 2
- Oscillator 2 output level

## Noise
- White noise generator output level
    - else/white~

## Sub-Oscillator
- Sub-Oscillator output level
    - Triangle wave
    - Pitched 1 octave below OSC1
    - else/bl.tri~


---


# High-Pass Filter
- Two-pole resonant high-pass filter per voice
- else/highpass~

## Cutoff
- Cutoff frequency of high-pass filter
- Range: 20Hz - 20kHz

## Resonance
- Amount of resonance (Q)
- Range: 0.001 - 100

## Env Amount
- Amount of modulation from filter envelope
- Range: -1 - 1

## Velocity
- Velocity modulation of filter cutoff
- On/Off

## Keyboard
- Keyboard tracking of filter cutoff
- Amount of modulation from keyboard (pitch) to cutoff frequency
- Off/Half/Full


---


# Low-Pass Filter
- Four-pole resonant low-pass filter per voice
- 2x else/lowpass~ (Two-pole each) in series

## Cutoff
- Cutoff frequency of low-pass filter
- Range: 20Hz - 20kHz

## Resonance
- Amount of resonance (Q)
- Range: 0.001 - 100

## Env Amount
- Amount of modulation from filter envelope
- Range: -1 - 1

## Velocity
- Velocity modulation of filter cutoff
- On/Off

## Keyboard
- Keyboard tracking of filter cutoff
- Amount of modulation from keyboard (pitch) to cutoff frequency
- Off/Half/Full


---

# Filter Envelope
- else/adsr~

## Attack
- Attack time: 0 - 10000 ms

## Decay
- Decay time: 0 - 10000 ms

## Sustain
- Susain level: 0 - 1

## Release
- Release time: 0 - 10000 ms


---


# Amplifier Envelope
- else/adsr~

## Env Amount
- Modulation of VCA from amplifier envelope
- Range: 0 - 1

## Velocity
- Velocity modulation of Env Amount

## Attack
- Attack time: 0 - 10000 ms

## Decay
- Decay time: 0 - 10000 ms

## Sustain
- Susain level: 0 - 1

## Release
- Release time: 0 - 10000 ms


---

# Effects
- Adding a few simple FX options instead of the P6 Effects Section
- Dry/Wet control for each effect

## Chorus
- else/chorus~
- Control:
    - Rate
    - Depth
    - Dry/Wet

## Delay
- else/filterdelay~
- Control:
    - Time (ms)
    - Feedback
    - Dry/Wet

## Reverb
- else/free.rev~
-   Control:
    - Decay
    - Width
    - Dry/Wet


---


# Low Frequency Oscillator
- else/lfo (Triangle/Sawtooth/RevSawtooth/Square)
- else/stepnoise (Random)

- Reset when a key is pressed (but not if a note is held)
- Control rate LFO/LFN

- Modulation amount controlled with Mod Wheel (0 - 100%)

## Frequency
- LFO Range: 0.1Hz - 50Hz

## LFO Sync (dropped)
- LFO sync to MIDI clock

## Shape
5 shapes + 1 hidden (random shape with frequency maxed)

### Triangle
- Bipolar
- message: "vsaw 0.5"

### Sawtooth
- Unipolar
- message: "vsaw 1"

### Reverse Sawtooth
- Unipolar
- message: "vsaw 0"

### Square
- Unipolar
- message: "square 0.5"

### Random
- Bipolar

### Noise (Hidden)
- Bipolar
- Rate: 100Hz (max control rate frequency)

## Initial Amount
- Modulation amount (0 - 100%)
    - 0%: Mod Wheel applies modulation

## Freq 1
- Mod destination: Frequency of Oscillator 1

## Freq 2
- Mod destination: Frequency of Oscillator 2

## PW 1+2
- Mod destination: Pulse Width of Oscillator 1 and 2 (if shape = square)

## Amp
- Mod destination: Amplitude level

## LP Filter
- Mod destination: Cutoff frequency of Low-Pass Filter

## HP Filter
- Mod destination: Cutoff frequency of High-Pass Filter


---


# Poly Mod
- Modulation matrix

## Modulation Sources

### Filter Env
- Range: -100% - 100%

### Osc 2
- Range: -100% - 100%

## Modulation Destinations

### Freq 1
- Mod destination: Frequency of Oscillator 1

### Shape 1
- Mod destination: Shape of Oscillator 1

### PW 1
- Mod destination: Pulse Width of Oscillator 1 (if shape = square)

### Filter
- Off/LP/HP/LP+HP

#### LP
- Mod destination: Cutoff frequency of Low-Pass Filter

#### HP
- Mod destination: Cutoff frequency of High-Pass Filter


---


# Clock / Arpeggiator / Sequencer (dropped)


---


# Master Volume
- else/out~
- Master volume control (VCA)


---


# Misc Parameters

## Pan Spread
- Pan audio in stereo field individually per voice
- Range: 0 - 100%

## Key Mode (dropped)

## P Whl Range
- Pitch Wheel Range
- Range: 0 - 12 semitones

## Prgm Vol (dropped)


---


# Distortion
- else/drive~

- Stereo distortion effect


---


# Transpose (dropped)


---


# Hold (dropped)


---


# Glide
- else/slew~~
- else/glide~

- 4 modes:
    - Fixed Rate:
        - Fixed speed
        - Longer intervals = slower time
    - Fixed Rate A
        - Same as Fixed Rate
        - Only applied when playing legato
            - (Turns glide on and off from the keyboard)
    - Fixed Time
        - Fixed time (ms)
        - Longer intervals = faster speed
    - Fixed Time A
        - Same as Fixed Time
        - Only applied when playing legato

## Glide Rate
- Mode dependent

### Fixed Rate
- else/slew~~
- Controls: Speed limit (amplitude per second)
- Range: XXX - XXXX

### Fixed Time
- else/glide~
- Controls: Glide time (ms)
- Range: 1 - XXXX

## Glide (On/Off)
- On/Off


---


# Unison (dropped)


---


# Aftertouch

## Amount
- Amount of modulation from aftertouch
- Range: -1 - 1

## Freq 1
- Mod destination: Frequency of Oscillator 1
- Pitch up/down

## Freq 2
- Mod destination: Frequency of Oscillator 2
- Pitch up/down

## LFO Amt
- Mod destination: Amount of modulation from LFO
- Increase/decrease amount

## Amp
- Mod destination: Amplitude level
- Increase/decrease volume

## Filter
- Off/LP/HP/LP+HP

### LP
- Mod destination: Cutoff frequency of Low-Pass Filter

### HP
- Mod destination: Cutoff frequency of High-Pass Filter
