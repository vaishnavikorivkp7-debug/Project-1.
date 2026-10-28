# Multimodal Attendance & Engagement Agent

Offline AI agent that estimates **attendance** and **engagement** from a webcam/video
(face presence, eye openness, head pose) and optional audio (speech activity), and shows
everything in a **Streamlit** dashboard with recommendations.

## Architecture (maps to your EPICs)

| EPIC | Module |
|------|--------|
| 1. Requirements & environment | `requirements.txt`, `config.py` |
| 2. Vision module | `vision/face_analyzer.py` (OpenCV + MediaPipe FaceMesh) |
| 3. Audio module | `audio/audio_analyzer.py` (Librosa / NumPy) |
| 4. AI implementation | `agent/engagement_agent.py` (score fusion + attendance decision) |
| 5. Analytics & recommendations | `analytics/recommender.py` |
| 6. Interface, testing, deployment | `app.py`, `tests/` |

## Setup (Python 3.9+)

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Run

```bash
streamlit run app.py
pytest -q
```

## How scoring works

- **Presence** = fraction of frames with a face detected.
- **Attention** = eyes open (EAR >= 0.20) and head facing screen (|yaw| <= 25, |pitch| <= 20).
- **Audio activity** = speech detected in the window (energy-based VAD).
- **Engagement** = 0.6 * attention + 0.4 * audio (vision only if no audio).
- **Attendance** = Present if face seen in >= 60% of the session.

All thresholds live in `config.py`.

## Notes

- Fully offline; no data leaves your machine.
- Live microphone input is optional (`sounddevice`); file input is the default.
