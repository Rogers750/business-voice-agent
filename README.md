# Voice Agent (Lightweight 8 GB Mac Setup)

This project is a small, focused voice agent designed to run on an 8 GB Mac without turning the machine into a space heater.

Step 1 is **architecture only** – no installs, no services, no background daemons. This README captures the initial plan so we don’t blindly install our way into regret.

## High-Level Goal

Build a conversational voice agent that:
- Listens to the microphone
- Transcribes speech locally
- Sends text to a remote LLM for reasoning
- Speaks the response back locally

All of this should run comfortably on a modest Mac.

## Chosen v1 Architecture

Pipeline:

`Mic input → faster-whisper (local STT) → DeepSeek API (LLM) → Piper TTS (local) → speaker output`

- **Brain (LLM):** DeepSeek API
  - Used for conversation and agent logic.
  - Runs remotely, so it doesn’t eat local RAM/VRAM.
- **Speech-to-Text (STT):** `faster-whisper` (local)
  - Lightweight Whisper implementation, good performance on consumer machines.
  - Keeps raw audio data local.
- **Text-to-Speech (TTS):** `Piper` (local)
  - Fast local TTS designed for desktop/edge use.
  - No reliance on cloud TTS for basic usage.

## Why This Split?

- The LLM is the most memory-hungry piece; keeping it remote (DeepSeek API) avoids trying to run a research lab on an 8 GB Mac.
- STT and TTS are kept local:
  - Lower latency for audio I/O.
  - Audio doesn't have to leave the machine.
- This stack is intentionally minimal but real:
  - It’s enough to have a voice loop that feels like an agent.
  - It avoids premature complexity like full duplex, barge-in, or streaming end-to-end speech models.

## Constraints

- Target machine: Mac with **8 GB RAM**.
- Priority:
  1. Don’t cook the laptop.
  2. Keep the code understandable.
  3. Prefer local components where they are light enough.

## Project Status

- ✅ Architecture decision
  - Pipeline and components chosen.
- ⏳ Implementation
  - No code, no environment setup, no installs yet.
- 🔒 Commitment for now
  - No commands or scripts that change system state until explicitly added in a later step.

## Next Steps (Not Yet Executed)

These are **planned**, not performed:

1. Define exact packages and tools:
   - Python environment and core libraries.
   - `faster-whisper` install options.
   - `Piper` install and voice selection.
   - DeepSeek API client and configuration.
2. Sketch a minimal loop:
   - Capture audio from the mic.
   - Transcribe with `faster-whisper`.
   - Send text to DeepSeek and stream back text.
   - Feed text into Piper and play audio.
3. Add guardrails for resource usage:
   - Choose model sizes that fit in memory.
   - Ensure we don’t spawn unnecessary background processes.

Implementation details and install commands will be added in later steps, after confirming this architecture is still the right trade-off for your machine and usage.

