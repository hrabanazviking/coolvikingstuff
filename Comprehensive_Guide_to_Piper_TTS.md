# Comprehensive Guide to Piper TTS

**Compiled from conversation on May 11, 2026** 
 **Focus:** Piper TTS usage, especially on Raspberry Pi, for Hermes Agent and local/offline setups.

---

## What is Piper TTS?

**Piper TTS** is a fast, fully offline, neural text-to-speech engine developed under the Open Home Foundation, originally by Rhasspy. It is based on the VITS model architecture and is specifically optimized for low-resource devices like the Raspberry Pi.

### Key Strengths

- Excellent real-time performance on CPU-only hardware
- Natural-sounding voices with good prosody
- Very low memory and storage footprint
- Broad language support, with 35+ languages
- Hundreds of community and official voices
- Completely free and open-source

---

## Model Quality Tiers

Piper models come in four main quality tiers:

| Quality Tier | Sample Rate | Approx. Parameters | Typical ONNX Model Size | Speed and Use Case | Audio Quality |
|---|---:|---:|---:|---|---|
| **x_low** | 16 kHz | 5 to 7 million | 20 to 35 MB | Fastest, Pi Zero / ultra-low power | Basic, somewhat robotic |
| **low** | 16 kHz | 15 to 20 million | 30 to 50 MB | Very fast | Decent |
| **medium** | 22.05 kHz | 15 to 20 million | 50 to 70 MB, typically around 60 to 65 MB | Best balance, real-time on Pi 4/5 | Natural, recommended default |
| **high** | 22.05 kHz | 28 to 32 million | 100 to 120 MB | Still real-time on Pi 5 | Richest detail and intonation |

Each voice package consists of:

- `.onnx`  
  The main model file

- `.onnx.json`  
  The config file, usually only a few KB

### Recommendation for Raspberry Pi

| Device | Recommended Quality |
|---|---|
| **Pi 4** | Medium |
| **Pi 5** | Medium or High |
| **Pi Zero / Pi 3** | Low or x_low |

---

## Performance on Raspberry Pi

| Raspberry Pi Model | Typical RTF, Medium Voice | Short Sentence Latency | Notes |
|---|---:|---:|---|
| **Pi 3** | 0.8 to 1.2 | 1 to 4 seconds | Usable with lighter models |
| **Pi 4** | 0.4 to 0.7 | 1 to 2 seconds | Excellent balance |
| **Pi 5** | 0.2 to 0.5 | Less than 1 second | Handles high-quality models easily |

All tiers run on CPU with no GPU required.

---

## British / UK English Female Voices

### Official Voices

- `en_GB-aru-medium`, female
- Various Sonia variants
- Alba and other community fine-tunes

### Popular Third-Party Female Voices

#### Bryce Beattie’s Collection

Excellent quality, public domain or permissive voices:

- **Cori**  
  UK English female, medium and high quality. Clear RP-style British voice.

- **Jenny / Dioco**  
  UK English with Irish influence and a warm tone.

- **Kristin**  
  US English female, very polished.

### Other Notable Sources

- AgentVibes collection on Hugging Face
- Community multi-speaker models with many selectable female UK voices

### Where to Find Voices

- Official Piper voices:  
  <https://huggingface.co/rhasspy/piper-voices>

- Bryce Beattie voices:  
  <https://brycebeattie.com/files/tts/>

- Hugging Face search:  
  Search for `piper en_GB female`

---

## Installation on Raspberry Pi

### 1. Binary Installation

Recommended method:

```bash
mkdir -p ~/piper
cd ~/piper

wget https://github.com/rhasspy/piper/releases/download/v1.2.0/piper_linux_aarch64.tar.gz
tar -xzf piper_linux_aarch64.tar.gz

cd piper
```

### 2. Python Package

```bash
pip install piper-tts
```

### 3. Docker

Docker is optional but can be useful for cleaner deployments.

---

## Basic Usage

### Command Line

```bash
echo "Hello, this is Piper running on my Raspberry Pi." | ./piper \
  --model en_GB-aru-medium.onnx \
  --output_file hello.wav

aplay hello.wav
```

### Python

```python
from piper import PiperVoice
import wave

voice = PiperVoice.load("/path/to/en_GB-aru-medium.onnx")

with wave.open("output.wav", "wb") as wav_file:
    voice.synthesize("Hello from Piper on Pi!", wav_file)
```

---

## Integration with Hermes Agent

Example Hermes configuration in:

```text
~/.hermes/config.yaml
```

```yaml
tts:
  provider: piper
  piper:
    model_path: /home/pi/piper/en_GB-cori-medium.onnx
    speaker: 0          # Only for multi-speaker models
    length_scale: 1.0   # Speed control
```

---

## Pros and Cons on Raspberry Pi

### Pros

- Outstanding speed/quality ratio
- Excellent British female voices available
- Fully offline and private
- Mature, stable project
- Easy to combine with Whisper.cpp for a full local voice assistant

### Cons

- Quality is very good, but not quite at ElevenLabs/OpenAI level
- Manual model management
- Occasional audio output quirks on Pi 5
- May require raw output plus `aplay` or `paplay` depending on the audio setup

---

## Comparison with Other TTS Options

| TTS | Offline | British Female Voices | Size/Speed on Pi | Best For |
|---|---|---|---|---|
| **Piper** | Yes | Excellent, many available | Very good | Local, accent-accurate TTS |
| **KittenTTS** | Yes | None dedicated | Excellent | Ultra-small footprint |
| **Edge TTS** | No | Excellent, Microsoft voices | N/A | Free high-quality cloud TTS |

---

## Tips and Best Practices

- Use medium models for most Raspberry Pi setups.
- Use high models on Pi 5 when quality matters more than minimum latency.
- Prefer PipeWire with `paplay` for better Bluetooth audio on Pi 5.
- Download models once and keep them in a central folder.
- Test voices with short samples before committing to one in Hermes.
- You can train your own voices with Piper tools if needed.
- Monitor CPU temperature during heavy concurrent use.
- Keep several voices available so Hermes/Runa can switch between them if desired.

---

## Useful Links

- Official Piper GitHub:  
  <https://github.com/rhasspy/piper>

- Official Piper Voices:  
  <https://huggingface.co/rhasspy/piper-voices>

- Bryce Beattie Voices:  
  <https://brycebeattie.com/files/tts/>

---

## Last Updated

**May 11, 2026**

### Sources

- Official Piper repository
- Community collections, including Bryce Beattie and AgentVibes
- Hermes Agent integration docs
- Raspberry Pi benchmarks

---


