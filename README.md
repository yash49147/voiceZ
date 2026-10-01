# VoiceGuard AI

Production-oriented Android/Compose reference implementation for an AI-powered voice scam/deepfake detection shield.

## Included

- Jetpack Compose dark cyber UI
- 5-second chunk monitor and simulation engine
- waveform + 16x20 telemetry visualization
- 2-class temperature-scaled Softmax confidence dial
- synthetic warning banner + feedback flow
- AudioRecord live microphone analyzer (16 kHz mono)
- AudioTrack harmonic/vocoder audit tone
- Room entities/DAOs for users and call logs
- salted SHA-256 authentication primitives with SecureRandom 16-byte salts
- CallScreeningService metadata hook
- native ACTION_DIAL integration
- Python FastAPI + librosa feature extraction endpoint
- TensorFlow Conv2D + BiLSTM architecture
- active-learning 3.5x hard-negative weighting skeleton

## Android Studio

Open the `VoiceGuardAI` directory in Android Studio and let Gradle sync.

A physical device is recommended for microphone and Telecom testing.

## Backend

```bash
cd backend
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

POST raw 16-bit PCM at 16 kHz mono to `/api/v1/audio/stream-chunk`.

## Important Android call-audio limitation

A normal third-party CallScreeningService is not a raw cellular-call PCM tap. VoiceGuard therefore has two separate paths:

1. **Carrier screening:** Telecom/CallScreeningService handles supported call metadata and screening actions.
2. **Acoustic analysis:** Live Mic mode uses AudioRecord on speakerphone/consented test audio.

Do not market the current prototype as silently intercepting private cellular downlink audio.

## Security notes

For a production deployment, move model inference to a hardened backend or on-device TFLite model, use TLS/certificate pinning, Keystore-backed secrets, encrypted local storage where required, strict retention controls, explicit consent, abuse protection, and model calibration/monitoring.

## Model

The demo simulator is deterministic and is NOT a trained deepfake detector. Replace `PatternSimulator` and the Python mock predictor with a validated dataset/model before making real-world detection claims.
