SynthiAKS Analog Modular Synthesizer

SynthiAKS (rhymes with "maniacs") is an emulation of the classic EMS Synthi AKS modular synthesizer, known for its unique pin matrix patching system and distinctive British analog sound used by artists like Pink Floyd and Brian Eno.

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