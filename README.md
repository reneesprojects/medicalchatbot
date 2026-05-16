# Medical Chatbot
my final year project!!!!!

- This research project investigates the feasibility of deploying Small Language Models in a fully offline voice conversational system for common medical knowledge exchange.

A fully offline, voice-enabled conversational bot for common medical knowledge exchange. 
The system runs entirely on local hardware with no internet connection, no cloud APIs, and no data sent externally — making it suitable for privacy-sensitive healthcare environments.

## What It Does

The bot listens to the user through a microphone, transcribes their speech, generates a medically-informed response using a local language model, and speaks the response back using text-to-speech. The entire pipeline runs offline on a standard laptop.

**Pipeline:** Microphone → Voice Activity Detection → Speech-to-Text → Language Model → Text-to-Speech → Speaker


## System Architecture

| Component | Technology | Role |
|---|---|---|
| Speech-to-Text | OpenAI Whisper Small | Transcribes user speech in real time |
| Voice Activity Detection | Silero VAD | Detects when the user has finished speaking |
| Language Model | Microsoft Phi-3 Mini (GGUF 4-bit) | Generates medical responses locally |
| Text-to-Speech | pyttsx3 + macOS Joelle voice | Speaks the response aloud |
| Orchestration | Python (multi-threaded) | Manages the full pipeline |


## Requirements

- macOS 
- Python 3.12+
- Anaconda / Conda

### Model File

Download the Phi-3 Mini GGUF model and place it in the project folder:

> The model file is not included in this repository due to its size (2.28 GB).

---

## File Structure

```
medical-bot/
├── stt_module.py        # Audio engine: VAD, Whisper ASR, pipeline orchestration
├── slm.py               # Phi-3 Mini language model wrapper
├── tts_module.py        # pyttsx3 TTS wrapper with interruptible speak loop
├── run_fullbot.py       # Entry point: loads all modules and starts the pipeline
└── MedicalBot.command   # Double-click launcher for macOS
```

| Parameter | Default | Description |
|---|---|---|
| `speech_prob_threshold` | 0.5 | VAD sensitivity (lower = more sensitive) |
| `silence_duration_before_transcribe_s` | 1.2 | Seconds of silence before transcription triggers |
| `post_tts_ignore_seconds` | 1.0 | Silence window after bot speaks |
| `interrupt_speech_prob_threshold` | 0.95 | Confidence required to interrupt the bot |
| `duplicate_transcript_window_seconds` | 0.5 | Prevents same phrase being processed twice |


## Known Limitations

- **Response latency:** Full pipeline averages 15–20 seconds on CPU. STT + SLM alone averages under 5 seconds; TTS adds the remaining time.
- **Echo feedback:** Without earphones, the microphone may pick up the bot's own voice. Earphones are recommended for best performance.
- **TTS instability:** pyttsx3 occasionally skips audio output on rapid consecutive queries due to macOS audio session threading limitations.
- **Static knowledge:** The system cannot auto-update medical information as it runs fully offline.
- **Developed on macOS:** pyttsx3 with the Joelle voice and Apple Metal GPU acceleration are macOS-specific. Deployment on other operating systems would require adjustments.

**Key finding:** A privacy-preserving voice medical assistant is technically achievable on consumer hardware. It is slower than cloud systems but shares no data externally.


## Disclaimer

This system is for informational purposes only. It does not diagnose conditions or prescribe medications. Always consult a qualified healthcare professional for medical advice.

