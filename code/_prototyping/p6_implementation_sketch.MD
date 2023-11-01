# Oscillator 1

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
- OSC range 16Hz - 8kHz
- LFO range 0.1Hz - 50Hz

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
- else/lowpass~ x2

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

