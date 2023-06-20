---
tags: music production synthesis harmonics fl-studio tutorials level:intermediate
---

This sequence of exercises will help you to understand the role of harmonics in a sound.

## Harmonics

In the realm of sound design and synthesis, harmonics play a pivotal role in defining the sonic character or timbre of a sound. Every musical sound we hear is typically made up of more than just one frequency; it is a complex combination of a fundamental frequency and a series of overtones, which are also called harmonics.

The fundamental frequency is the lowest, loudest frequency, and it's what determines the pitch that we perceive. The harmonics are frequencies that occur at integer multiples of the fundamental. While they are usually less loud, they significantly influence the color and character of the sound.

Understanding harmonics is vital for sound designers and music producers. It equips them with the knowledge to predict how different waveform shapes will sound, and how filters, envelopes, and other sound-shaping tools will affect the overall timbre.

For example, a square wave contains only odd harmonics, while a sawtooth wave contains both odd and even harmonics. Understanding this helps in selecting the right waveform to produce the desired sound. Similarly, knowing that a low-pass filter removes high-frequency harmonics helps in shaping the sound's brightness and sharpness.

Through this chapter, we delve deeper into the realm of harmonics, exploring various exercises and techniques that enhance our understanding and application of harmonics in the process of sound design. This forms an integral part of mastering synthesis and creating unique, appealing sounds.

## Basic Exercises

For these exercises, we will use one or more instances of 3x Osc and Fruity Parametric EQ 2 to view the shape of the sound. You may optionally use Wave Candy to view the waveform.

_A note about the HQ setting in 3x Osc: The HQ setting in 3x Osc is a high-quality mode that uses more CPU. It is recommended to use this setting when you are designing sounds, but you can turn it off when you are done. With this setting off, you may notice upper harmonics that shouldn't otherwise exist- these are computational artifacts and not truly parts of the sound._

### Exploring Harmonic Content of Basic Waveforms

Using 3x Osc and a spectrum analyzer (like the one in EQ plugins), disable OSC 1 and 2, then observe how the harmonic content changes with different waveforms (sine, sawtooth, square, triangle, etc.). Pay attention to how the harmonics are spread and their intensity levels.

- Sin: A single peak. This corresponds to the frequency of the sin wave.
- Square: Odd harmonics.
- Triangle: Odd harmonics, with progressively quieter highs.
- Sawtooth: Even and odd harmonics. A sawtooth is the richest of the basic waveforms.

### Creating Harmonics with Distortion
Apply distortion or overdrive to a simple sine wave and observe the new harmonics that are created. Try different types of distortion and observe how they affect the harmonic content.

- Use Fast Dist set to max THRESH, MIX, and POST and observe the waveform and spectrum. Compare to a Square Wave with no effects. This similarity is due to the distortion applying an amplitude change to the sin wave.
- Set 3x Osc to a square wave, then Fast Dist to PRE 50%, THRESH 100%, MIX 0%, and POST 100%. Slowly increase mix and observe change in the X-Y graph. Note the change in amplitude in the waveform and corresponding increase in harmonic content in the spectrum.

### Layering with Multiple Oscillators
This time, use multiple oscillators set at different waveforms, pitches, and levels. Experiment with adding slightly detuned copies of your main oscillator and observe how this creates a richer sound with more harmonics.

### Harmonic Shaping with Filters
Create a basic patch with a rich waveform like a sawtooth. Then, use a filter to cut or boost specific harmonic regions. Use a low pass, high pass, band pass, and notch filters and observe how they affect the sound.

## Optional Math: Fourier Transforms

The Fourier Transform is a mathematical technique that is used to transform a function of time, a signal, into a function of frequency. In the context of audio and music production, it's the mathematical process used to analyze the frequencies contained within a complex waveform or sound.

When you play a note on a synthesizer, you are hearing a complex wave, which is composed of a fundamental frequency (the note you played) and a series of overtones, also called harmonics. Each harmonic is a whole number multiple of the fundamental frequency and contributes to the overall timbre or tone color of the sound.

To visualize these frequencies and their relative amplitudes, we use a spectrum analyzer, a tool that performs a Fourier Transform on the audio signal in real time. The spectrum analyzer displays the individual frequencies (or closely grouped sets of frequencies) as vertical lines along a horizontal frequency axis. The height of each line represents the amplitude of that frequency component.

The harmonic content of a sound is often referred to as its "spectrum" because it describes the range of frequencies present in the sound and their relative levels. Different sounds have different spectrums, and this is what gives them their unique character.

When using synthesizers like 3x Osc, you're manually adding or subtracting harmonics with the waveforms and filters. When viewing your sound on a spectrum analyzer, you're seeing the results of the Fourier Transform, showing you what frequencies are present in your sound. By understanding this, you can make more informed decisions about what waveforms and filters to use to shape your sound.
