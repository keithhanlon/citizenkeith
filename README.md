# JSFX

These are JSFX plugins for Cockos REAPER.

These effects were written with the help of Claude AI. What started out as a simple learning exercise turned into madness! I'm using these in my own projects and very happy with the results.

### Install via Reapack: 
https://raw.githubusercontent.com/keithhanlon/citizenkeith/master/index.xml

### Learn How To Install JS plugins:
https://reaper.blog/2015/06/quick-tip-how-to-install-js-plugins/

# Plugin Features

Quick links:
- [ReaFripp Tape Loop Delay](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#reafripp-tape-loop-delay)
- [Tycho Delay TMA-1](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#tycho-delay-tma-1)
- [Stargate Ambient Reverb](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#stargate-ambient-reverb)
- [SynthiAKS Analog Modular Synthesizer](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#synthiaks-analog-modular-synthesizer)
- [TapeEmu - Professional Tape Machine Emulation](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#tapeemu---professional-tape-machine-emulation)
- [Probabilistic MIDI Note Generator](https://github.com/keithhanlon/citizenkeith/blob/main/README.md#probabilistic-midi-note-generator)

# Delays
## ReaFripp Tape Loop Delay
![ReaFripp Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/ReaFripp.png?raw=true)

This plugin is inspired by Frippertronics, Robert Fripp's tape delay technique that utilized two Revox tape machines to create repeated delay. It includes optimizations like:

- Interpolation for smooth playback at fractional delay times
- Fast sine approximation for wow/flutter calculations
- Pre-calculated filter coefficients for efficiency

This creates a vintage tape delay experience with the characteristic imperfections and warmth of analog tape loops.

### Controls:
**Loop Length** (0.1-10 seconds)

1. Sets the duration of the tape loop delay
2. When tempo sync is off, this directly controls loop time in seconds
3. When tempo sync is on, this value is overridden by the tempo-synced calculation

**Feedback** (0-1)

1. Controls how much of the delayed signal is fed back into the loop
2. Higher values create longer, more sustained echoes
3. At maximum (1.0), the loop will sustain indefinitely

**Wet/Dry Mix** (0-1)

1. Blends between the original dry signal (0) and the delayed wet signal (1)
2. 0.5 gives equal parts dry and wet signal

**High-Frequency Damping** (0-1)

1. Simulates tape's natural high-frequency roll-off over time
2. Higher values progressively filter out high frequencies from the loop
3. Creates a warmer, more vintage tape sound as the loop repeats

**Saturation** (0-1)

1. Adds harmonic distortion to simulate tape saturation
2. Higher values create more pronounced distortion effects

**Saturation Mode** (0-1, toggle)

1. 0 (Soft Clipping): Symmetric soft clipping distortion
2. 1 (Tape Saturation): Asymmetric tape-style saturation that's more musical

**Wow/Flutter** (0-1)

1. Simulates tape speed variations that occur in real tape machines
2. Wow: Slow speed variations (0.1-6 Hz)
3. Flutter: Fast speed variations (>6 Hz)
4. Creates subtle pitch modulation for authentic tape character

**Tempo Sync** (0-1, toggle)

1. 0 (Off): Uses manual loop length from Slider 1
2. 1 (On): Synchronizes loop length to host tempo using time divisions

**Time Division** (0-6)

1. Only active when Tempo Sync is enabled
2. Sets the musical note value for the loop length:

  - 0: 1/4 note (1 beat)
  - 1: 1/2 note (2 beats)
  - 2: 1 bar (4 beats)
  - 3: 2 bars (8 beats)
  - 4: 4 bars (16 beats)
  - 5: 8 bars (32 beats)
  - 6: 16 bars (64 beats)

## Tycho Delay TMA-1
![Tycho Delay TMA-1 Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/TykoDelay.png?raw=true)

Tycho Delay is a tape delay emulation that goes beyond basic delay functionality to provide vintage tape characteristics with modern optimization and flexibility.

### Controls:

**Delay** (1-80,000 ms)

1. Sets the delay time in milliseconds
2. When tempo sync is off, this directly controls the delay length
3. When tempo sync is on, this value is automatically calculated based on the sync division

**Delay (sync)** (Off, 1/32 to 16 bars)

1. Tempo synchronization with 28 different musical divisions:
* 0: Off (manual delay time)
* 1-28: Various note values including Standard notes (1/32, 1/16, 1/8, 1/4, 1/2, 1, 2, 4, 8, 16 (bars)), Triplets (T) (1/16T, 1/8T, 1/4T, etc. (2/3 of normal length)) and Dotted (D) (1/32D, 1/16D, 1/8D, etc. (1.5x normal length))

**Mix** (0-1)

1. Blends between dry signal (0) and wet delayed signal (1)
2.v0.5 gives equal parts dry and wet

**Input Gain** (-90 to +15 dB)

1. Adjusts the level of signal going into the delay buffer
2. Higher values can drive the tape saturation harder

**Feedback** (-90 to +15 dB)

1. Controls how much delayed signal is fed back into the delay line
2. Higher values create longer, more sustained echoes
3. Negative dB values prevent runaway feedback

**Volume** (-30 to +30 dB)

1. Master output level control for the entire effect

**Dry Out** (-90 to +15 dB)

1. Independent level control for the dry (unprocessed) signal

**Wet Out** (-90 to +15 dB)

1. Independent level control for the wet (delayed) signal

**Delay Loop Lowpass** (1,000-15,000 Hz)

1. High-frequency filter applied within the delay feedback loop
2. Simulates tape's natural high-frequency roll-off over repeated passes
3. Lower values create warmer, more vintage tape sounds

**Delay Loop Saturation** (1-100%)

1. Tape-style saturation applied within the delay feedback loop
2. Creates harmonic distortion that builds up with each repeat
3. Simulates tape compression and saturation characteristics

**Output Saturation** (1-100%)

1. Saturation applied to the final mixed output
2. Adds overall harmonic coloration to the entire effect

# Reverb

## Stargate Ambient Reverb
![targate Ambient Reverb Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/Stargate.png?raw=true)

Stargate Ambient Reverb is a dual-stage algorithmic reverb designed for creating lush, expansive ambient spaces. It features a main diffuse reverb engine that can morph from clear delays to dense reverb textures, plus an independent tail extender for infinite, blooming decays. Perfect for ambient music, cinematic soundscapes, and adding otherworldly depth to any source.
### Controls:

**REVERB SECTION**

**Regen** (0-95%)

Controls the feedback amount in the main reverb, determining how long the reverb tail sustains. Higher values create longer, more sustained reverberations.

**Bright** (0-100$%)

High-frequency damping control. Lower values create darker, more natural reverb tails by filtering high frequencies. Higher values preserve brightness for shimmering, ethereal textures.

**Shimmer** (0-100%)

Adds subtle pitch modulation to the reverb, creating a chorus-like shimmer effect. Adds movement and animation to the reverb tail without detuning.

**Size** (10-100%) 

Adjusts the length of the delay lines, scaling the reverb from tight, room-like spaces to vast, cathedral-sized environments.

**Pitch** (-12 to +12 semitones) 

Pitch shifts the reverb feedback, creating shimmer reverb effects. Positive values shift up (classic shimmer), negative values shift down for darker, more ominous tails.

**Diffusion** (0-100%)

The key morphing control. At 0%, you hear discrete delays. As you increase it, the delays diffuse into smooth, dense reverb. Low values = rhythmic delays, high values = lush reverb wash. WATCH OUT: The feedback can get out of control easily.

**Dry/Wet** (0-100%)

Standard mix control. 0% = completely dry signal, 100% = completely wet reverb signal.

**TAIL EXTENDER SECTION**

**Decay** (0-100%)

1. High-frequency filterControls the feedback amount in the secondary reverb stage. This extends the main reverb's tail, allowing for extremely long, infinite-style decays without pushing the main reverb into instability.

**Mix** (0-100%)

Blends the tail extender output back into the main signal. At 0%, the tail extender is off. Increase to add progressively longer, smoother tails for massive ambient textures.

# Synths

## SynthiAKS Analog Modular Synthesizer
![SynthiAKS Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/SynthiAKS.png?raw=true)

SynthiAKS (pronounced sinth ee aks) is an emulation of the classic EMS Synthi AKS modular synthesizer, known for its unique pin matrix patching system and distinctive British analog sound used by artists like Pink Floyd and Brian Eno.

### Features:
1. 4-pole Synthi-style filter with characteristic resonance
2. Pin matrix modulation system mimicking the original's patch pin connections
3. Analog drift simulation for authentic vintage instability
4. Sample & Hold circuit for random stepped modulation
5. Ring modulator for metallic/bell-like timbres
6. Noise generator as modulation source
7. Complete ADSR envelope generator

### Controls:
**External Input Level** (0-1)

1. Controls the level of external audio input being fed into the synthesizer
2. Allows processing external signals through the synth's filters and effects

**Oscillator Frequency** (20-2000 Hz)

1. Sets the base pitch/frequency of the internal oscillator
2. Can be modulated by Sample & Hold via the pin matrix

**Oscillator Waveform** (0-3)

1. 0: Sawtooth wave (bright, buzzy sound)
2. 1: Triangle wave (warmer than sawtooth)
3. 2: Square wave (hollow, woody sound)
4. 3: Sine wave (pure, smooth tone)

**Filter Cutoff** (0-1)

1. Sets the base cutoff frequency of the 4-pole lowpass filter
2. Can be modulated by envelope and Sample & Hold via pin matrix

**Filter Resonance** (0-8)

1. Controls filter feedback/resonance amount
2. Higher values create more prominent filter peaks and can self-oscillate

**Oscillator Drift** (0-0.1)

1. Simulates analog oscillator instability with slow frequency drift
2. Adds authentic vintage analog character

### Pin Matrix (Modular Connections):

**Pin: Osc → Filter Cutoff** (-1 to 1)

1. Routes oscillator signal to modulate filter cutoff frequency
2. Negative values invert the modulation

**Pin: Envelope → Filter Cutoff** (-1 to 1)

1. Routes envelope generator to modulate filter cutoff
2. Classic filter sweep effect when positive

**Pin: Envelope → VCA (Amplitude)** (0-1)

1. Routes envelope to control overall amplitude
2, Essential for creating note attacks and releases

**Pin: External → Ring Modulator** (0-1)

1. Routes external input to the ring modulator for processing

**Pin: Osc → Ring Modulator** (0-1)

1. Routes internal oscillator to the ring modulator

**Ring Mod Frequency** (5-200 Hz)

1. Sets the carrier frequency for the ring modulator
2. Can be modulated by Sample & Hold

**Ring Mod Mix** (0-1)

1. Blends between dry signal and ring modulated signal

### Sample & Hold System:

**Noise Level** (0-1)

1. Controls the amplitude of the internal noise generator
2. Noise is the source for the Sample & Hold circuit

**S&H Clock Rate** (0.1-10 Hz)

1. Sets how fast the Sample & Hold circuit samples new values
2. Higher rates create faster, more chaotic modulation

**Pin: S&H → Filter Cutoff** (-1 to 1)

1. Routes Sample & Hold output to modulate filter cutoff
2. Creates stepped, random filter sweeps

**Pin: S&H → Osc Frequency** (-1 to 1)

1. Routes Sample & Hold to modulate oscillator frequency
2. Creates random pitch sequences

**Pin: S&H → Ring Freq** (-1 to 1)

1. Routes Sample & Hold to modulate ring modulator frequency
2. Creates random timbral changes

### Envelope Generator:

**Envelope Attack** (0.001-2 seconds)

1. Sets how quickly the envelope rises from 0 to maximum
2. Shorter = sharper attack, longer = gradual fade-in

**Envelope Decay** (0.001-2 seconds)

1. Sets how quickly the envelope falls from peak to sustain level
2. Controls the initial brightness decay

**Envelope Sustain** (0-1)

1. Sets the level the envelope holds while a note is sustained
2. 0 = no sustain, 1 = full level sustain

**Envelope Release** (0.001-5 seconds)

1. Sets how quickly the envelope falls to zero when note is released
2. Controls the tail/fade-out time

**Envelope Trigger** (0-1)

1. Manual trigger to start the envelope
2. Acts like pressing a key on the synthesizer

# MIDI
## Probabilistic MIDI Note Generator
![Note Generator Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/NoteGenerator.png?raw=true)

The Probabilistic MIDI Generator creates evolving, generative MIDI patterns using probability-based algorithms. Perfect for ambient music, experimental compositions, modular-style sequencing, and creative improvisation.
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

# Tape Emulation
## TapeEmu - Professional Tape Machine Emulation
![TapeEmu Screenshot](https://github.com/keithhanlon/citizenkeith/blob/main/images/TapeEmu.png?raw=true)

TapeEmu is a tape machine emulation plugin designed to add the warmth, character, and sonic artifacts of classic analog tape recording to digital audio. Features include harmonic saturation, tape hiss, low-frequency head bump, stereo width control, and tape aging effects like dropouts and wow & flutter.

The plugin offers five different tape machine models (Studer A800, Ampex ATR102, Sony APR5000, Otari MTR90, and Basic Tape), each with their own unique saturation curves, compression characteristics, and tonal coloring.

### Controls:

**Input/Saturation** (0-178%)

1. Controls the amount of tape saturation/harmonic distortion
2. Higher values add more warmth and harmonics to simulate tape saturation
3. Each tape machine type applies different harmonic characteristics

**Output** (-50 to 0 dB)

1. Master output level control
2. Adjusts the final volume of the processed signal

**Noise** (-105 to -60 dB)

1. Controls the level of tape hiss/noise floor
2. Simulates the inherent noise present in analog tape recordings
3. Each machine type has slightly different noise characteristics

**Head Bump** (-18 to +12 dB)

1. Simulates the low-frequency boost that occurs in tape machines
2. This is a frequency-dependent effect that enhances bass frequencies
3. Uses FFT processing to apply the boost in specific frequency bands

**Tape Age** (0-100%)

1. Simulates the effects of aged/worn tape
2. Adds random dropouts (brief signal reductions)
3. Increases noise levels and adds modulation flutter
4. Higher values create more audible artifacts

**Stereo Width** (0-200%)

1. Controls the stereo image width using mid/side processing
2. 100% = normal width, <100% = narrower, >100% = wider
3. Some machine types apply subtle frequency coloring to the stereo image

**Tape Machine** (5 options)

1. Selects different tape machine emulations with unique characteristics

**Wow & Flutter** (0-100%)

1. Simulates tape speed variations
2. Wow = slow variations (0.5 Hz), Flutter = fast variations (12 Hz)
3. Uses a delay buffer with modulated read position to create pitch variations

### Tape Machine Options
**0: Studer A800**

1. Warm, musical character with even harmonics
2. Gentle compression (1:2.5 ratio)
3. Slightly enhanced mids in stereo processing

**1: Ampex ATR102**

1. Punchy, colored sound with odd harmonics and asymmetry
2. More aggressive compression (1:1.5 ratio)
3. Enhanced presence in stereo image

**2: Sony APR5000**

1. Clean, modern sound with minimal distortion
2. Very gentle compression (1:4.0 ratio)
3. Accurate, uncolored processing

**3: Otari MTR90**

1. Balanced, professional sound with mixed harmonics
2. Moderate compression (1:2.0 ratio)
3. Slight warmth added

**4: Basic Tape**

1. Generic tape emulation using manual slider settings
2. Uses original saturation formula
3. No preset characteristics applied

Each machine type also has different preset values for head bump, noise floor, and compression characteristics that override the manual settings when selected (except for "Basic Tape" which uses the manual slider values).

