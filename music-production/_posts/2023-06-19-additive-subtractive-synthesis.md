---
tags: music production synthesis harmonics fl-studio tutorials level:intermediate
---

By doing this exercise, you will be manually building a complex waveform through additive synthesis (using multiple sine waves at different pitches to create a complex harmonic structure) and then applying subtractive synthesis principles (using a filter to subtract frequencies from this complex waveform). This simulates the signal flow of an additive-subtractive synthesizer like Harmless.

This exercise assumes you know about and understand harmonics. If you don't, please work through [Understanding Harmonics](Understanding-Harmonics.md) first. You should also be comfortable using the following plugins:

- 3x Osc
- Fruit Parametric EQ2
- Wave Candy

As well, I assume you are generally familiar with FL Studio and have a careful ear.

## Checking Your Work

I found it very helpful to create a send with a saw wave and send it to a mixer with a spectrum analyzer. Since saw waves have both even and odd harmonics, this allowed me to compare my DIY synth to a saw wave and see how close I was getting. I used Fruity Notepad as a ruler to check my harmonic peaks against the saw wave.

## Additive Synthesis

1. Setting up the fundamental: 
    - In the channel rack, create an instance of 3x Osc.
    - Set OSC 2 and OSC 3 volume to 0.
    - Enable HQ mode.
    - Send the channel to your Parametric EQ2 and Wave Candy visualizers.
    - In the piano role for this channel, create a note at C3 (the duration is up to you).
    - Observe the pure sin wave.
    - Turn up OSC 2 and 3 at +12 and +24 semitones, respectively. This will create octaves of the harmonics.
2. Creating the 2nd harmonic:
    - In the channel rack, clone 3x Osc.
    - In the piano roll for this channel, create a note at C4 (12 semitones higher).
    - Again, observe the waveform and spectrum.
3. Creating the 3rd harmonic:
    - In the channel rack, clone 3x Osc gain
    - In the piano roll for this channel, create a note at G4 (7 semitones higher).
    - Again, observe the waveform and spectrum.
4. Repeat this process for the next nine harmonics:
    - 4th: C5 (5 semitones higher)
    - 5th: E5 (4 semitones higher)
    - 6th: G5 (3 semitones higher)
    - 7th: Bb5 (2 semitones higher)
    - 8th: C6 (1 semitones higher)
    - 9th: D6 (2 semitones higher)
    - 10th: E6 (3 semitones higher)
    - 11th: F#6 (4 semitones higher)
    - 12th: G6 (5 semitones higher)
    
_Why are some of the notes missing? A few of the notes aren't found until much higher in the harmonic series. See [the Harmonic Series wikipedia article](https://en.wikipedia.org/wiki/Harmonic_series_(music)) if you want to finish the scale_

You may also notice that our sound doesn't have as many harmonics as the saw wave. Since the saw wave is a mathematical construct, it can have "infinite" oscillators to establish harmonics, whereas we're limited to manual input.

We've created a simple set of harmonics. Now, let's take a brief aside to talk about equal temperament.

### Equal Temperament

Equal temperament is a system of tuning that divides the octave into 12 equal parts. This is the system that is used in modern music. The reason for this is that it allows for the most flexibility in terms of key changes. For example, if you are playing in the key of C, you can easily change to the key of G by simply moving up 7 semitones. If you were using a different tuning system, this would not be possible.

However, this system is not perfect. The problem is that the intervals between the notes are not exactly equal. For example, the interval between C and C# is not the same as the interval between C# and D. This is because the ratio between the frequencies of the notes is not exactly the same. This is a problem because it means that the harmonics of a note will not line up exactly with the harmonics of the note an octave above it.

### Tuning our synth to just intonation

In order to get our additive synthesizer in tune, we need to detune the harmonics slightly using Fine Tune in 3x Osc. This is called _just intonation_. Use your comparison saw wave, a ruler, and (especially) your ears to tune the harmonics.

You're doing pretty good if you can get within 2 cents of just tuning. However, this is pretty challenging, so heres a cheat sheet:

- 1st, 2nd, 4th, 8th (C): +0c(ents)
- 3rd, 6th, 12th (G): +2c
- 5th, 10th (E): -14c
- 7th (Bb): -31c
- 9th (D): +4c
- 11th (F#): -49c

Make sure to tune OSC 2 and 3 for each instance.

_Note: You probably won't be able to hear much difference on the 3rd, 6th, and 12th harmonics._

## (Optional) Add Subharmonics

Subharmonics are lower harmonics that are not part of the harmonic series, but some synths artificially create them by adding additional sine waves at lower frequencies. By doing so, the synth adds low-end weight to the sound, giving it more presence.

We'll emulate Harmless's design for subharmonics. Per the Harmless manual:

> The first slider Sub 1 is an octave below the root note. The remaining two sub sliders are harmonics of Sub 1. Sub 3 is the 3rd harmonic and Sub 4 is the 4th harmonic

_Note: It's not clear whether the subharmonics are replicated at multiples of 12 like the harmonics. I'm assuming they are not._

Let's add this feature to our DIY synth.

1. In the channel rack, clone the fundamental 3x Osc three times. Have the clones play the following notes:

- Sub 1: C2 (12 semitones below the fundamental)
- Sub 3: G2 (7 semitones below the fundamental)
- Sub 4: C3 (12 semitones above the fundamental)

We skip Sub 2, because this would just be the fundamental again.

## Subtractive Synthesis

Subtractive synthesis, as the name suggests, involves 'subtracting' or filtering out certain frequencies from a sound. This is typically accomplished using filters. Filters are tools that allow certain frequencies to pass while reducing the volume of others, effectively shaping the harmonic content of a sound.

In the context of our exercise, we'll be using a low-pass filter.

### Low-Pass Filters

A low-pass filter, or LPF, is a type of filter that allows frequencies below a certain point (called the cutoff frequency) to pass through, while reducing the volume of frequencies above this point. The cutoff frequency is adjustable and its manipulation is a key aspect of subtractive synthesis.

1. In your mixer, create a new instance of Fruity Filter.
2. You'll notice a Cutoff Freq knob, this adjusts the cutoff frequency. A lower setting will allow fewer high frequencies to pass through.
3. Experiment with moving the cutoff frequency and observing the effect on the waveform and spectrum. As you lower the cutoff frequency, you should notice the higher frequencies being attenuated or completely removed from the sound.
4. Also experiment with the resonance knob, which boosts the frequencies around the cutoff point. This can make the cutoff point more pronounced in the final sound.

It's also worth understanding that the steepness of the filter's roll-off (how quickly it reduces the volume of frequencies above the cutoff point) is also crucial. This is usually measured in decibels per octave (dB/Oct). A higher dB/Oct value means a steeper roll-off.

Subtractive synthesis is about more than just filtering out unwanted frequencies. By shaping the harmonic content of a sound, you're effectively sculpting its timbre - the characteristic that allows us to distinguish a piano from a guitar when they play the same note at the same volume. This makes subtractive synthesis a powerful tool for sound design.

## Summary

In this exercise, we created a complex waveform through additive synthesis and then used subtractive synthesis to remove frequencies from it. 

## Jargon and Definitions

- **Additive Synthesis**: A sound synthesis technique that creates timbre by adding sine waves together. The different sine waves, or harmonics, are manipulated to define a particular timbre.
- **Subtractive Synthesis**: A method of sound synthesis in which partials of an audio signal (often one rich in harmonics) are attenuated by a filter to alter the timbre of the sound.
- **Harmonics**: Overtones, or additional sine waves, that exist above a fundamental frequency. They are integer multiples of the fundamental frequency. 
- **Fundamental Frequency**: The lowest frequency of a periodic waveform. In music, it is the primary pitch of a musical note that the human ear identifies.
- **OSC or Oscillator**: A device for generating oscillating electronic signals. In the context of synthesizers, an oscillator generates a tone that is then shaped and manipulated to create a desired sound.
- **Equal Temperament**: A system of tuning in which every pair of adjacent notes has an identical frequency ratio. It is the standard tuning system for western musical instruments.
- **Just Intonation**: A system of tuning in which the frequencies of notes are related by ratios of small whole numbers. It creates pure intervals, but lacks the flexibility of key modulation offered by equal temperament.
- **Cents**: A logarithmic unit of measure used for musical intervals. One cent is one hundredth of an equal-tempered semitone.
- **Low-Pass Filter**: A filter that passes signals with a frequency lower than a certain cutoff frequency and attenuates signals with frequencies higher than the cutoff frequency.
- **Cutoff Frequency**: The frequency at which a filter begins to reduce the amplitude of a signal.
- **Resonance**: In the context of a filter, it refers to the boost of frequencies around the cutoff frequency. 
- **Roll-off**: The rate at which a filter attenuates the signal beyond the cutoff frequency. It's often measured in decibels per octave (dB/octave).
- **Timbre**: The quality of a musical sound or voice as distinct from its pitch and intensity. It is what makes a particular musical sound have a different sound from another, even when they have the same pitch and loudness.
