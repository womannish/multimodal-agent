
# AI Guard Agent

## Overview
The **AI Guard Agent** is an integrated multimodal safety system that combines face verification, voice/command recognition, and LLM-based reasoning to create a secure interactive assistant. The agent observes, interprets, and responds to user commands while enforcing identity and intent checks.

## Features
- **Face recognition**: Build and store trusted face embeddings, verify faces against trusted identities.
- **Command recognition**: Lightweight speech-to-intent classifier with confidence scores.
- **LLM interface**: Wrapper around a large language model API to generate contextual responses.
- **State graph / flow control**: A graph-based state machine that controls the end-to-end flow (Idle → Capture → Verify → Listen → Classify → Reason → Speak).
- **Audio & media utilities**: Microphone recording, webcam capture, text-to-speech for real-time interaction.
- **Visualization**: Graphviz visualization of the state graph for easier debugging and presentation.

## Repository Structure
```
AI_Guard_Agent.ipynb
Known_Faces/                # Directory containing trusted user images
face_embeddings/            # Auto-generated embeddings
guard_state_graph.png       # Graph visualization (optional)
README.md                   # This file
```

## Getting Started

### Requirements
Install the typical Python packages used in the notebook:
```bash
pip install torch torchvision torchaudio numpy pillow graphviz pydub matplotlib
# plus your LLM client library, e.g., openai or other provider client
```

If you run in Google Colab, ensure you allow camera and microphone permissions.

### Prepare trusted faces
1. Create a folder called `Known_Faces/`.
2. Add one or more clear frontal photos for each trusted person.

Run the embedding extraction to create a trusted embeddings database:
```python
from your_module import save_image_embeddings
save_image_embeddings("Known_Faces", output_dir="face_embeddings")
```

### Run the guard system
Execute the notebook's main loop:
```python
from your_module import run_guard_system
run_guard_system()
```
The loop typically performs:
1. Idle waiting
2. Camera capture and face verification
3. Microphone recording and intent classification
4. LLM query and response generation
5. Text-to-speech output
6. Return to Idle

## Module Summary

### FaceRecognition
- Extract embeddings from face images.
- Compare input embeddings to trusted embeddings using cosine similarity.
- `FaceRecognition.verify_face(image, threshold=0.7)` returns True/False.

### CommandRecognition
- Converts recorded audio into an "intent" label with a confidence score.
- Integrates with the state graph to drive transitions.

### LLM
- Lightweight wrapper that sends prompts and receives responses.
- Handles API errors and returns model output to the agent.

### State Graph
- Nodes define discrete states (Idle, Capture, Verify, Listen, Classify, Reason, Speak).
- Edges define transitions, optionally with success/failure conditions.
- Visualized using Graphviz.

## Tips & Best Practices
- Use multiple images per trusted person to improve embedding robustness.
- Tune verification threshold based on empirical matches (0.6–0.85 typical ranges).
- Add anti-spoofing (liveness detection) for production security.
- Expand the intent classifier dataset for better command coverage and fewer false positives.
- Log decisions (face similarity, intent confidence, LLM outputs) for auditability and debugging.

## Potential Improvements
- Integrate a liveness/spoof-detection module.
- Support multi-user sessions and profile switching.
- Add role-based permissions (e.g., admin vs. guest intents).
- Use an off-the-shelf speech recognition and intent model for higher accuracy.
- Deploy as a standalone service with a lightweight web UI.

## Example Usage
```python
# Build embeddings (one-time)
save_image_embeddings("Known_Faces", output_dir="face_embeddings")

# Load trusted embeddings (startup)
trusted = load_trusted_embeddings("face_embeddings")

# Start the guard
run_guard_system()
```

## Acknowledgements
This project combines techniques from:
- Computer Vision (face embeddings, similarity)
- Speech Processing (audio capture, intent classification)
- Natural Language Processing (LLM-based reasoning)
- Systems Design (state machines and real-time interaction)

---
