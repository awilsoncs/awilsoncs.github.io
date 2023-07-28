---
tags: music production synthesis harmonics fl-studio tutorials level:beginner
---

Welcome to the journey of sound design using FL Studio's 3x Osc and stock effects! This tutorial is designed for those with basic understanding of FL Studio and its interface, aiming to take the novice to an intermediate level.

You should be comfortable with the interface of FL Studio, FL Parametric EQ 2. If you don't know your way around 3x Osc, refer to the [FL Studio manual](https://www.image-line.com/fl-studio-learning/fl-studio-online-manual/html/plugins/3x%20Osc.htm) for any questions.

This guide offers an exploration into the art of synthesizing a multitude of sounds. Our focus will range from creating percussive elements like kicks, snares, and hi-hats, to essential bass sounds such as sub and the Reese bass, mid-range instruments including pads and plucks, and a broad spectrum of sound effects (FX), such as risers and impacts.

Throughout this journey, we will delve into key sound design principles, including understanding different oscillator waveforms, the use of ADSR envelopes to shape sounds, applying filters to control the harmonic content of your sounds, and using effects processing to introduce depth, space, character, and complexity to your creations.

Mastering sound design is invaluable for any music producer or composer. Here are some reasons why:

1. **Creative Control**: Synthesis provides the power to shape unique sounds and craft a distinctive musical identity. Imagine being able to create a signature sound that sets you apart from everyone else!
2. **Problem Solving**: Sound design proficiency allows you to troubleshoot and resolve mix issues. For instance, if a synth lead fails to cut through the mix, manipulating its harmonics can help it stand out.
3. **Efficiency**: While presets have their place, reliance on them can limit creativity and workflow. Knowledge of crafting sounds will significantly speed up your production process.
4. **Communication**: Familiarity with synthesis language enables better communication with other producers, sound designers, and engineers.

With this guide, you'll learn sound design from first principles and gain a deep understanding of the building blocks of synthesis. The knowledge you'll gain here will empower you to create and manipulate sounds with precision and creativity.

The tutorial will walk you through creating a variety of synth sounds using FL Studio's 3x Osc and stock effects. You'll end up with a set of essential patches that could be used to write an entire song, covering a range of percussive sounds, bass sounds, mid-range instruments, lead sounds, and sound effects. In mastering these sounds, you'll establish a solid foundation in synthesis and sound design, applicable to any synthesizer.

### Terminology, Jargon, and Definitions

Refer back to this section as needed. If you're comfortable exploring 3x Osc and know your way around stock plugins, you can safely skip this.

- **Oscillator**: The component in a synthesizer that generates a basic waveform like sine, saw, square, and triangle. The selection of waveform, pitch, phase offset, and volume settings in 3x Osc shapes the character of your sound.
- **Filter**: Used to sculpt the sound by removing or boosting certain frequencies. The most common types are low-pass, high-pass, and band-pass.
- **Envelope**: A tool for shaping a sound over time, defined by its Attack, Decay, Sustain, and Release (ADSR). For example, a piano note has a quick attack and decay, a low sustain, and a slow release.
- **Reverb**: An effect simulating the sound reflections in a physical space, adding depth and space to your sounds.
- **Delay**: An effect creating echoes of a sound, adding complexity and width to your sounds.
- **Detune**: The process of slightly altering the pitch of oscillators in relation to each other to create a thicker, "chorused" (or layered) sound.
- **White Noise**: A type of noise that has equal intensity at different frequencies, similar to the sound of a waterfall or TV static. Often used for percussive sounds and sound effects.
- **Pitch**: The perceived frequency of a sound, determining how "high" or "low" a sound is. Pitch can be manipulated for creative effect.
- **Cutoff Frequency**: In the context of a filter, the cutoff frequency is the point where the filter starts to affect the sound. Think of it as the filter's "threshold."
- **Resonance**: A boost at the cutoff frequency, making a filter sound more pronounced or "resonant."

### Understanding ADSR Envelopes

ADSR stands for Attack, Decay, Sustain, and Release - these are the four stages that make up an ADSR envelope, which is a fundamental component in sound synthesis.

1. **Attack**: This is the time it takes for the sound to reach its maximum level after it's triggered. A short attack time will create a sound that starts quickly, like a snare drum or plucked string. A longer attack time creates a sound that gradually fades in, like a soft pad or string section.
2. **Decay**: After the attack phase, the sound begins to drop in level. The time it takes to fall to the level set by the sustain is the decay phase. Short decay times can create percussive sounds, while longer decay times are often used for more sustained sounds.
3. **Sustain**: This is not a time parameter but a level parameter. It sets the level at which the sound will continue after the decay phase, as long as the note is held (or the gate is open). If the sustain level is set to maximum, the sound level will not drop after the attack phase, no matter how long the note is held. 
4. **Release**: This is the time it takes for the sound to drop from the sustain level to silence after the note is released (or the gate is closed). Short release times can create abrupt, staccato-like sounds, while longer release times create sounds that fade out slowly.

In 3x Osc (as well as many other synthesizers), you can set an ADSR envelope for the volume, pitch, and filter cutoff frequency. These allow you to shape the amplitude, pitch, and tonal content of a sound over time, providing the fundamental tools for sound design.

## The Patches

### Overview the Patches

We'll explore a variety of basic synthesizer patches throughout this exercise, including:

1. **Percussive Sounds**:
   - Kick: A foundational element in almost all genres of music, particularly in rhythm-focused genres like electronic music and hip-hop.
   - Snare: Another rhythmic element that provides the backbeat in many genres.
   - Closed Hi-Hat (HH): A high-frequency percussive element that often provides a steady rhythmic pulse or embellishments.
   - Open Hi-Hat: Similar to the closed hi-hat but with a longer sustain, often used for accents or transitions.

2. **Bass Sounds**:
   - Sub Bass: A deep, low-frequency sound that provides the bottom end in many genres, particularly in electronic music.
   - Moog Bass: A classic analog-style bass sound known for its rich, warm tone.

3. **Mid-Range Instruments**:
   - Atmospheric Pad: A long, sustained sound often used for creating atmosphere and filling out the mix.
   - Harmonic Pad: Another type of pad sound, often used for harmonic support or layering.
   - Plucks: Short, percussive sounds often used for melodies or arpeggios.

4. **Sound Effects (FX)**:
   - Riser: A sound that increases in pitch and/or volume over time, used for building tension.
   - Impact: A high-energy sound used to emphasize the downbeat of a new section of a track.

5. **Lead Sounds**:
   - Saw Lead: A bright, rich lead sound perfect for energetic, uplifting melodies.
   - Square Lead: A warm, slightly hollow-sounding lead great for more laid-back or retro-flavored melodies.

These patches serve as an introduction to sound design in FL Studio, and understanding how to create them will give you a solid foundation to start creating your own unique sounds.

### Percussive Sounds

#### Kick

1. **Waveform Selection**: Start by setting all oscillators in 3x Osc to sine waves, which are perfect for creating the smooth, low-frequency content of a kick.

2. **Tuning and Mixing**: Tune the oscillators to different octaves to add more harmonic content to the sound. For example, OSC 2 to -12 semitones, and OSC 3 to -24 semitones.

3. **Volume Envelope**: Set the volume envelope with a fast attack, short decay, no sustain, and no release. This will give the sound a sharp, percussive quality.

4. **Effects**: In the Mixer, add a Fruity Parametric EQ 2 to shape the sound. Boost the low frequencies to emphasize the "boom" of the kick, and cut the high frequencies to remove any harshness.

#### Snare

1. **Waveform Selection**: In 3x Osc, set OSC 1 to a sine wave for the body of the snare, and OSC 2 to white noise for the snare's "snap". Turn off OSC 3.

2. **Tuning**: Tune OSC 1 to the pitch you want for your snare's body. Leave OSC 2 at the default setting.

3. **Volume Envelope**: Set the volume envelope with a fast attack, short decay, no sustain, and no release. This will give the sound a sharp, percussive quality.

4. **Effects**: In the Mixer, add a Fruity Parametric EQ 2 to shape the sound. Boost the mid frequencies for the body, and the high frequencies for the snap.

#### Closed Hi-Hat (HH)

1. **Waveform Selection**: Set all oscillators in 3x Osc to white noise. This will provide the high-frequency content that's characteristic of hi-hats.

2. **Filtering**: Use a high-pass filter to remove any unwanted low-frequency content.

3. **Volume Envelope**: Set the volume envelope with a fast attack, short decay, no sustain, and no release. This will make the sound short and percussive, like a real hi-hat.

4. **Effects**: In the Mixer, add some reverb to give the hi-hat a sense of space. Boost high mids slightly in EQ to similate the characteristic resonance of a real high-hat.

#### Open Hi-Hat

1. **Waveform Selection**: Set all oscillators in 3x Osc to white noise.

2. **Filtering**: Use a high-pass filter to remove any unwanted low-frequency content.

3. **Volume Envelope**: Set the volume envelope with a fast attack, moderate decay, slight sustain, and short release.

4. **Effects**: In the Mixer, add some reverb to give the hi-hat a sense of space. You can use a longer reverb decay time than you did for the closed hi-hat to make the sound more sustained. Boost high mids slightly in EQ to similate the characteristic resonance of a real high-hat.

### Bass Sounds

#### Sub Bass

1. **Waveform Selection**: In 3x Osc, set all three oscillators to sine waves. Sine waves are ideal for sub bass because they produce a pure tone with no harmonic overtones.

2. **Tuning and Mixing**: You can experiment with tuning the oscillators at different octaves to add some subtle harmonic content. For example, you might set OSC 1 to +0 semitones, OSC 2 to -12 semitones, and OSC 3 to -24 semitones. Mix the oscillators to taste, but the lower octave oscillator should be the loudest to maintain a deep, subby sound.

3. **Volume Envelope**: For the volume envelope, set a fast attack, high sustain, and medium release. This will make the sound continuous when a note is held, and prevent clicks or pops when the note stops.

4. **Effects**: For a sub bass, it's best to keep effects minimal to maintain a clean low end. However, you can use a Fruity Parametric EQ 2 in the Mixer to make any necessary adjustments to the sound. For example, you might boost the very low frequencies or cut some of the higher frequencies.

#### Reese Bass

1. **Waveform Selection**: Set all three oscillators in 3x Osc to saw waves. This will give the Moog bass its characteristic rich, buzzy sound.

2. **Tuning and Mixing**: Detune the oscillators slightly from each other to create a thicker, more interesting sound. For example, you might set OSC 1 to -12 semitones, OSC 2 to -12 semitones and +7 cents, and OSC 3 to -12 semitones and -7 cents. Mix the oscillators to taste.

3. **Filtering**: Use a low-pass filter to remove some of the harsh high frequencies and create a warmer sound. You can also add some filter envelope with a short decay and no sustain to create a plucky sound.

4. **Volume Envelope**: For the volume envelope, set a fast attack, high sustain, and medium release. 

5. **Effects**: In the Mixer, add a Fruity Parametric EQ 2 to make any necessary adjustments to the sound. You might also add some light distortion or saturation to give the sound a bit of grit, and some slight chorus or phaser to add movement and depth.

### Mid-Range Instruments

#### Atmospheric Pad

1. **Waveform Selection**: In 3x Osc, set all three oscillators to saw waves. This will give the pad a rich, full sound.

2. **Tuning and Mixing**: Detune the oscillators slightly from each other to create a thicker, more interesting sound. Consider duplicating this instrument to add even more voices at other tunings.

3. **Filtering**: Use a low-pass filter to remove some of the harsh high frequencies and create a warmer sound. Set a slight filter envelope with a long attack and release to add some movement to the sound.

4. **Volume Envelope**: Set the volume envelope with a slow attack, high sustain, and slow release. This will make the sound evolve slowly over time, which is characteristic of pads.

5. **Effects**: In the Mixer, add some reverb and delay to create a sense of space and depth. You might also add some chorus to thicken the sound further.

#### Harmonic Pad

1. **Waveform Selection**: Set all three oscillators in 3x Osc to square waves. This will give the pad a softer, more mellow sound.

2. **Tuning and Mixing**: Detune the oscillators slightly from each other to create a thicker, more interesting sound.

3. **Filtering**: Use a low-pass filter to remove some of the harsh high frequencies. Set a slight filter envelope with a long attack and release to add some movement to the sound.

4. **Volume Envelope**: Set the volume envelope with a slow attack, high sustain, and slow release.

5. **Effects**: In the Mixer, add some reverb and delay to create a sense of space and depth. You might also add some chorus to thicken the sound further.

#### Pluck

1. **Waveform Selection**: Set all three oscillators in 3x Osc to saw waves. This will give the pluck a bright, rich sound.

2. **Tuning and Mixing**: Detune the oscillators slightly from each other to create a thicker, more interesting sound.

3. **Filtering**: Use a low-pass filter with a filter envelope. Set the envelope with a fast attack, short decay, low sustain, and short release. This will make the sound start bright and then quickly become darker, which is characteristic of plucks.

4. **Volume Envelope**: Set the volume envelope with a fast attack, short decay, no sustain, and short release. This will make the sound short and percussive.

5. **Effects**: In the Mixer, add some reverb and delay to give the sound a sense of space. You might also add some light distortion or saturation to give the sound some grit.

### Lead Sounds

#### Saw Lead

1. **Waveform Selection**: Set all three oscillators in 3x Osc to saw waves. This will give the lead a bright, rich sound.

2. **Tuning and Mixing**: Detune the oscillators slightly from each other to create a thicker, more interesting sound.

3. **Filtering**: Use a low-pass filter to remove any harsh high frequencies. Depending on the style of music, you might want to keep the filter cutoff quite high.

4. **Volume Envelope**: Set the volume envelope with a fast attack, high sustain, and medium release.

5. **Effects**: In the Mixer, add some reverb and delay to give the sound a sense of space. You might also add some light distortion or saturation to give the sound some grit, and some slight chorus or phaser to add movement and depth.

#### Square Lead

1. **Waveform Selection**: Set all three oscillators in 3x Osc to square waves. This will give the lead a warm, slightly hollow sound.

2. **Tuning and Mixing**: Detune the oscillators slightly from each other to create a thicker, more interesting sound.

3. **Filtering**: Use a low-pass filter to remove any harsh high frequencies. Depending on the style of music, you might want to keep the filter cutoff quite high.

4. **Volume Envelope**: Set the volume envelope with a fast attack, high sustain, and medium release.

5. **Effects**: In the Mixer, add some reverb and delay to give the sound a sense of space. You might also add some light distortion or saturation to give the sound some grit, and some slight chorus or phaser to add movement and depth.

### Sound Effects (FX)

#### Riser

1. **Waveform Selection**: In 3x Osc, set all three oscillators to saw waves. This will give the riser a bright, rich sound.

2. **Tuning and Mixing**: Detune the oscillators slightly from each other to create a thicker, more interesting sound.

3. **Pitch Envelope**: Set a pitch envelope with a slow attack, no decay, high sustain, and no release. This will make the pitch of the sound rise over time.

4. **Volume Envelope**: Set the volume envelope with a slow attack, high sustain, and medium release.

5. **Effects**: In the Mixer, add some reverb and delay to give the sound a sense of space. You might also add some light distortion or saturation to give the sound some grit, and some phaser or flanger to add movement and depth.

#### Impact

1. **Waveform Selection**: In 3x Osc, set OSC 1 to a sine wave and OSC 2 and 3 to white noise. This will give the impact a deep "boom" and a high-frequency "crash".

2. **Tuning and Mixing**: Tune OSC 1 to a very low pitch to create the "boom". Mix the white noise oscillators to taste to create the "crash".

3. **Volume Envelope**: Set the volume envelope with a fast attack, short decay, no sustain, and short release.

4. **Effects**: In the Mixer, add some reverb to give the sound a sense of space. You might also add some EQ to boost the low frequencies and shape the high frequencies.

## Recap

Great job on your first steps into the world of sound design! Here's a recap of some of the principles and techniques you've explored:

Understanding Oscillators: You've learned how different oscillator waveforms (sine, saw, square, and triangle) can be used to create different tones and textures. You've seen how these waveforms can be combined and detuned to create complex sounds.

Envelopes: You've used ADSR (Attack, Decay, Sustain, Release) envelopes to shape the volume and timbre of your sounds over time. You've seen how this can drastically affect the character of a sound, making it short and plucky or long and evolving.

Filters: You've used different types of filters (low-pass, high-pass, band-pass) to shape the harmonic content of your sounds. You've seen how automating the cutoff frequency can add movement and interest to a sound.

Effects Processing: You've used effects like reverb, delay, distortion, and chorus to further shape your sounds and place them in a mix. You've seen how these effects can add depth, space, character, and complexity to your sounds.

Sound Types: You've learned how to create different types of sounds (kicks, snares, hi-hats, basses, pads, leads, plucks, and FX) that are commonly used in electronic music production.

Tweaking and Experimentation: Perhaps most importantly, you've seen how sound design is a process of experimentation. Every sound can be tweaked and adjusted in countless ways, and small changes can sometimes have a big impact on the final result.

Now that you're comfortable with these basics, you can start to delve deeper into each area. You might explore more complex oscillator types (like wavetable or FM synthesis), more advanced modulation techniques (like LFOs or envelopes modulating pitch, filter, or effects parameters), or more advanced effects processing (like multiband compression or convolution reverb). You might also start to design your own unique sounds from scratch, rather than following guides or tutorials. Happy sound designing!
