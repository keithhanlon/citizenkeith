# JSFX

These are JSFX plugins for Cockos REAPER.

These effects were written with the help of Claude AI. What started out as a simple learning exercise turned into madness! I'm using these in my own projects and very happy with the results.

## Installation

### Via ReaPack (Recommended)
1. Install ReaPack if you haven't already
2. Add this repository: https://raw.githubusercontent.com/keithhanlon/citizenkeith/master/index.xml
3. Install plugins - all dependencies installed automatically

### Manual Installation
All plugins require the shared UI library:
1. Download `citizenkeith-ui-lib/citizenkeith-ui-lib.jsfx-inc`
2. Download the plugin(s) you want
3. Place all files in your REAPER Effects folder (same directory)

### Learn How To Install JS plugins:
https://reaper.blog/2015/06/quick-tip-how-to-install-js-plugins/

# Plugin Features

Quick links:
- [Fidelio Channel Strip](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#fidelio-channel-strip)
- [Garden Hose Delay](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#garden-hose-delay)
- [ReaFripp Tape Loop Delay](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#reafripp-tape-loop-delay)
- [Tycho Delay TMA-1](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#tycho-delay-tma-1)
- [Watkins Fuzz Pedal](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#watkins-fuzz-pedal)
- [Analog Warmth Compressor](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#analog-warmth-compressor)
- [FM Radio Compressor](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#fm-radio-compressor)
- [Transparent Bus Compressor](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#transparent-bus-compressor)
- [Probabilistic MIDI Note Generator](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#probabilistic-midi-note-generator)
- [Tempo Tremolo](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#tempo-tremolo)
- [SynthiAKS Analog Modular Synthesizer](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#synthiaks-analog-modular-synthesizer)
- [Cloudbusting Reverb](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#cloudbusting-reverb)
- [Cocteau Verb](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#cocteau-verb)
- [TapeEmu - Professional Tape Machine Emulation](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#tapeemu---professional-tape-machine-emulation)

# Channel Strips
## Fidelio Channel Strip
![Fidelio Channel Strip Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/fideliov2.png?raw=true)

Fidelio is a channel strip plugin combining transformer-style harmonic saturation, a Neve-inspired high shelf EQ, a Trident-style high-pass filter, two parametric mid bands, and a dynamics section with compressor, expander, and gate modes. 

# Delays

## Garden Hose Delay
![Garden Hose Delay Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/fideliov2.png?raw=true)
An emulation of the Cooper Time Cube, a classic electromechanical delay unit that created its characteristic doubling effect by sending audio through a coiled garden hose as a transmission line. 

## ReaFripp Tape Loop Delay
![ReaFripp Tape Loop Delay Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/reafrippv2.png?raw=true)
This plugin is inspired by Frippertronics, Robert Fripp's tape delay technique that utilized two Revox tape machines to create repeated delay.

## Tycho Delay TMA-1
![Tycho Delay TMA-1 Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/tychov2.png?raw=true)
Tycho Delay is a tape delay emulation that provides vintage tape characteristics. Version 2 includes a complete GUI overhaul, DSP fixes, a dedicated tempo sync on/off toggle, ping pong stereo, and an optimized memory.

# Distortion

## Watkins Fuzz Pedal
![Watkins Fuzz Pedal Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/watkinsv2.png?raw=true)
Emulation of the Watkins WEM Project V Fuzz pedal, a sophisticated 8-transistor British fuzz from the late 1960s.

# Dynamics

## Analog Warmth Compressor
![Analog Warmth Compressor Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/AWCv2.png?raw=true)
VintageWarmer-inspired compressor with optional 2x oversampling via a 63-tap halfband FIR filter. 

## FM Radio Compressor
![FM Radio Compressor Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/FMradio.png?raw=true)
Upward compressor inspired by analog FM radio processing. Boosts quiet passages while leaving loud peaks untouched.

## Transparent Bus Compressor
![Transparent Bus Compressor Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/TBCv2.png?raw=true)
SSL/Sonitus-inspired glue compression for mix bus duty.

# MIDI

## Probabilistic MIDI Note Generator
![Note Generator Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/probmidiv2.png?raw=true)
The Probabilistic MIDI Generator creates evolving, generative MIDI patterns using probability-based algorithms. Perfect for ambient music, experimental compositions, modular-style sequencing, and improvisation.
Insert it before any MIDI instrument in REAPER, and watch as it generates note sequences that never repeat exactly the same way twice.

### Quick Start Guide

1. Insert the plugin on an empty track before a MIDI instrument
2. Choose your key with the ROOT knob (60 = C4)
3. Select a scale using the large SCALE knob
4. Set pattern density with the TRIGGER knob (70% is a great starting point)
5. Adjust pattern variation with REPEAT (50% balances repetition and change)
6. Hit play and hear your generative sequence come to life!

From there, experiment with:

1. VEL RND and DUR RND for more organic, human-like expression
2. STEPS to change rhythmic subdivision (4 = sixteenth notes, 8 = thirty-second notes)
3. LENGTH to extend or shorten your pattern phrases
4. OCTAVES to expand or narrow the melodic range

# Modulation

## Tempo Tremolo
![Tempo Tremolo Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/tempotremv2.png?raw=true)
Tempo-synced stereo tremolo with morphing wave shapes.

# Reverb

## Cloudbusting Reverb
![Cloudbusting Reverb Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/cloudbustingv2.png?raw=true)
Cloudbusting Reverb is an algorithmic reverb inspired by the Cloud Seed Reverb, using parallel comb filters and cascaded allpass diffusion to create rich, modulated spaces ranging from tight rooms to vast ambiences.

## Cocteau Verb
![Cocteau Verb Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/cocteauv2.png?raw=true)
Stereo widener, BBD chorus, ping pong delay and three reverb choices, for a Robin Guthrie-inspired tone. 

# Synths

## SynthiAKS Analog Modular Synthesizer
![SynthiAKS Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/synthiaksv2.png?raw=true)
SynthiAKS (rhymes with "maniacs") is an emulation of the EMS Synthi AKS modular synthesizer, known for its unique pin matrix patching system and distinctive analog sound, used by artists like Pink Floyd and Brian Eno.


# Tape Emulation
## TapeEmu - Professional Tape Machine Emulation
![TapeEmu Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/TapeEmuV2.png?raw=true)
TapeEmu is a tape machine emulation plugin designed to add the warmth, character, and sonic artifacts of classic analog tape recording to digital audio. Features include harmonic saturation, tape hiss, low-frequency head bump, stereo width control, and tape aging effects like dropouts and wow & flutter.

