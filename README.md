# ClassVision AI — Visualizing every explanation, live.

> **ClassVision AI** is an AI teaching assistant that runs alongside PowerPoint. It captures the currently displayed slide, listens to the teacher's recent explanation, detects the cursor position, sends this context to an AI model, generates a useful educational diagram, and displays the diagram in a professional always-on-top overlay.

```text
┌───────────────────────────────────────┬─────────────────────────────────────┐
│                                       │                                     │
│       POWERPOINT PRESENTATION         │           CLASSVISION AI            │
│                                       │                                     │
│       Current Slide                   │           On-The-Spot               │
│       (e.g., TCP Handshake)           │           SVG Educational           │
│                                       │           Diagram Overlay           │
│                                       │                                     │
└───────────────────────────────────────┴─────────────────────────────────────┘
```

---

## 1. System Requirements

* **OS:** Windows 10 / 11 (also runs on macOS & Linux)
* **Python:** 3.10 or 3.11+
* **Microphone:** Built-in laptop or external USB microphone
* **Internet Connection:** Required for Google Gemini multimodal API diagram generation

---

## 2. Installation

1. **Clone repository:**
   ```bash
   git clone https://github.com/your-username/classvision-ai.git
   cd classvision-ai
   ```

2. **Create and activate virtual environment:**
   ```bash
   python -m venv .venv
   # Windows PowerShell:
   .venv\Scripts\Activate.ps1
   # Windows Command Prompt:
   .venv\Scripts\activate.bat
   # Linux / macOS:
   source .venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure Environment Variables:**
   ```bash
   copy .env.example .env
   ```
   Add your Google Gemini API key:
   ```env
   AI_API_KEY="your_gemini_api_key_here"
   AI_MODEL="gemini-2.5-flash"
   WHISPER_MODEL="base"
   TRANSCRIPT_DURATION=45.0
   ```

---

## 3. Quickstart & Running

Run the desktop application:
```bash
python app.py
```

### The MVP Teaching Flow:
1. Click **Start Session** (ClassVision enters "● Listening" state).
2. Open your PowerPoint presentation on screen.
3. Teach normally — speech is transcribed locally via `faster-whisper` and kept in a rolling ~45-second buffer.
4. Point your cursor to the element you are explaining.
5. Press **F9** (or say *"visualize this"*).
6. ClassVision AI captures the slide, retrieves the ~45s explanation and cursor hint, asks Gemini for the structured educational diagram specification, renders clean SVG, and pops up the always-on-top overlay beside PowerPoint!
7. Click **↻ Regenerate** to request an alternative perspective or close the overlay anytime.

---

## 4. Architecture

```text
                 ┌─────────────────────┐
                 │   PowerPoint Slide  │
                 └──────────┬──────────┘
                            │
                            ▼
                    Screen Capture
                         mss
                            │
                            ▼
┌───────────────┐    ┌───────────────┐
│  Microphone   │───►│ Speech Engine │
│               │    │ faster-whisper│
└───────────────┘    └───────┬───────┘
                             │
                             ▼
                    Recent Transcript
                       ~45 seconds
                             │
                             ▼
┌───────────────┐    ┌────────────────┐
│ Cursor (Hint) │───►│ AI Orchestrator│
│ pyautogui     │    │                │
└───────────────┘    └───────┬────────┘
                             │
                             ▼
                     Google Gemini API
                             │
                             ▼
                   Structured Diagram JSON
                             │
                             ▼
                     Local SVG Renderer
                             │
                             ▼
                   Always-On-Top Overlay
```

---

## 5. Supported Diagram Types

ClassVision AI enforces a strict JSON schema and supports 10 pedagogical visualization types:
* `flowchart`: Sequential logical branching
* `process`: Step-by-step pipeline
* `architecture`: System components and interactions
* `timeline`: Chronological progression
* `cycle`: Circular recurring loops (e.g. Calvin cycle, OS scheduling)
* `comparison`: Side-by-side contrast (e.g. Monolith vs Microservices)
* `hierarchy`: Tree-like branching (e.g. Binary Search Trees, DOM)
* `concept_map`: Core hub-and-spoke conceptual relationships
* `sequence`: Inter-entity message passing (e.g. TCP Handshake, HTTP)
* `labeled_diagram`: Visual callouts and annotations

---

## 6. Voice Triggers & F9 Fallback

* **Voice triggers:**
  - *"visualize this"*
  - *"visualize this line"*
  - *"visualize this concept"*
  - *"visualize the slide"*
  - *"visualize the whole page"*
  - *"show me a diagram"*
  - *"draw this"*
* **F9 Hotkey:** The primary, dependable fallback. F9 always works even when the microphone is muted or Whisper is unavailable.

---

## 7. Privacy Architecture

* **Local Speech:** Audio is transcribed entirely on-device using local `faster-whisper`. Raw microphone audio is never uploaded to the cloud.
* **API Payload:** Only the slide screenshot, recent text transcript, cursor hint, and visualization instruction are sent to the AI API when requested.
* **Ephemeral Storage:** Screenshots are deleted from the local disk immediately after rendering.

---

## 8. Demo Mode (Offline / Hackathon Ready)

If you don't have a live microphone or PowerPoint presentation handy during a presentation or hackathon:
1. Click **💡 Load Demo Mode** in the main window.
2. A sample TCP Handshake slide and teacher explanation are instantly simulated.
3. The system generates and renders the diagram without microphone dependencies.

---

## 9. Running Tests

Run the full automated test suite (19 unit tests covering cursor calculations, transcript buffer, trigger detector, JSON schema validation, and SVG generation):
```bash
python classvision-ai/tests/run_tests.py
```
