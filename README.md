# Stable Audio 3 — ComfyUI Workflow

A ComfyUI workflow for AI-powered music and audio stem generation using **Stable Audio 3**, featuring structured text prompting for precise control over genre, instruments, mood, and BPM.

> Workflow assembled by **Marco Capelli** · [@adamdived1](https://x.com/adamdived1)

---

## Features

- Full-track audio generation up to **120 seconds**
- Structured prompt system following Stable Audio 3's official format
- Support for **full band tracks** and **individual stems** (e.g. drums-only)
- Positive and negative prompt conditioning for fine-grained control
- Fast generation via **LCM sampler** (distilled model, ~10 steps)
- Built-in **AudioWaveformVisualizer** and **SaveAudio** output nodes
- Clean graph using **Set/Get nodes** (KJNodes) for easy readability

---

## Required Models

Download and place these files in the correct directories:

| File | Destination |
|------|-------------|
| [`stable_audio_3_medium.safetensors`](https://huggingface.co/Comfy-Org/stable-audio-3/resolve/main/checkpoints/stable_audio_3_medium.safetensors?download=true) | `models/checkpoints/` |
| [`t5gemma_b_b_ul2.safetensors`](https://huggingface.co/Comfy-Org/stable-audio-3/resolve/main/text_encoders/t5gemma_b_b_ul2.safetensors?download=true) | `models/text_encoders/` |

---

## Required Custom Nodes

- [KJNodes](https://github.com/kijai/ComfyUI-KJNodes) — for Set/Get routing nodes
- [ComfyUI-Pixaroma](https://github.com/pixaroma/ComfyUI-Pixaroma) — for the embedded documentation note
- [audio-waveform-visualizer](https://github.com/eigenpunk/ComfyUI-audio) — for waveform preview

---

## Sampler Settings

| Model Type | Distilled | Steps | CFG |
|------------|-----------|-------|-----|
| Base | No | 50+ | 7+ |
| Non-base (medium) | Yes | 8+ | 1 |

**Defaults used in this workflow:**
- Sampler: `LCM`
- Scheduler: `Simple`
- Steps: `10`
- CFG: `1`
- Duration: `120 seconds`

---

## Prompt Structure

Stable Audio 3 responds best to a **structured prompt format**. The order of elements matters — always start with `Format` and `Genre`.

```
Format: Band | Genre: Rock | Subgenre: Dark Rock, Psychedelic Rock | Instruments: heavily reverbed distorted electric guitar with wah-wah and tremolo, droning fuzz bass, slow pounding live drum kit with tribal feel, haunting organ swells, eerie theremin-like synth lead | Mood: Hypnotic, Dark, Ritualistic, Trance-Inducing | Style: 1970s Psychedelic Underground, Desert Rock, Occult Doom | BPM: 108
```

### Prompt Template

```
Format: [Band / Electronic / Drum Stem / etc.] | Genre: [...] | Subgenre: [...] | Instruments: [...] | Mood: [...] | Style: [...] | BPM: [number]
```

### Example Prompts

**Occult Psychedelic Rock:**
```
Format: Band | Genre: Rock | Subgenre: Dark Rock, Psychedelic Rock, Occult Rock | Instruments: heavily reverbed distorted electric guitar with wah-wah and tremolo, droning fuzz bass, slow pounding live drum kit with tribal feel, haunting organ swells, eerie theremin-like synth lead, layered feedback textures, ritualistic percussion | Mood: Hypnotic, Hallucinogenic, Dark, Ritualistic, Unsettling, Trance-Inducing, Oppressive | Style: 1970s Psychedelic Underground, Desert Rock, Occult Doom, Stoner Rock | BPM: 70
```

**Detroit Techno:**
```
Format: Electronic | Genre: Techno | Subgenre: Detroit Techno, Dark Techno, Industrial | Instruments: pounding 909 kick drum, distorted acid bassline, cold metallic hi-hats, eerie pitch-shifted synth pads, hypnotic looping arpeggiator, industrial noise textures, gated reverb snare | Mood: Dark, Hypnotic, Relentless, Cold, Dystopian, Trance-Inducing | Style: 1990s Detroit Underground, Warehouse Club, Industrial Electronics | BPM: 135
```

**Commercial Pop-Rock (for negative prompting reference):**
```
Format: Band | Genre: Rock | Subgenre: Easy Rock, Commercial Pop-Rock | Instruments: bright electric guitars with light chorus, punchy snare drum kit, driving bass guitar, uplifting piano stabs, subtle synth pad | Mood: Upbeat, Energizing, Optimistic, Feel-Good | Style: 2020s, Advertising, Film Instrumental | BPM: 120
```

---

## Stem Generation

To generate a **single instrument stem** (e.g. drums only), focus the entire positive prompt on that instrument and use the negative prompt to exclude everything else.

**Positive prompt example (drums only):**
```
Format: Drum Stem | Genre: Synthwave, Retrowave | Subgenre: Electro, Outrun | Instruments: electronic drum machine, LinnDrum, TR-808 kick drum, gated reverb snare, punchy rimshot, tight closed hi-hats, open hi-hat, claps, electronic tom fills, sidechained kick transients | Mood: Driving, Punchy, Mechanical, Energetic | Style: 1980s Drum Machine, Electronic Percussion, Giorgio Moroder | BPM: 110
```

**Negative prompt example (drums only):**
```
synth, bass, chords, pads, melody, strings, piano, arpeggio, guitar, harmony, lead, atmosphere, reverb tails, ambient
```

---

## Workflow Structure

The graph is organized into three groups:

| Group | Contents |
|-------|----------|
| **Load Models** | CheckpointLoader, CLIPLoader, ModelSamplingAuraFlow patch, Set nodes for MODEL / CLIP / VAE |
| **Prompts** | CLIPTextEncode (positive + negative), Set nodes for conditioning |
| **Sampler** | Get nodes, KSampler, EmptyLatentAudio, VAEDecodeAudio, Visualizer, SaveAudio |

---

## 📄 License

This workflow configuration is released for free use and modification. The underlying models are subject to their respective licenses on Hugging Face.
