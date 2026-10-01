# 🎭 Emotion Match

A browser game where your webcam reads your facial expression in real time and you
must match the emoji on screen before the timer runs out.
Built for the *Design of AI & Products* project. Single file: [`index.html`](index.html).

## How to run (step by step)

1. Download `index.html` (keep it anywhere, e.g. the Desktop).
2. Make sure you are **online** the first time (the AI library and model weights load from a CDN, then the browser caches them).
3. Open the file in **Google Chrome** or **Microsoft Edge** (double-click, or right-click → Open with).
   - If your browser blocks the camera on `file://`, run a tiny local server instead:
     `python -m http.server 8000` in the folder, then open <http://localhost:8000>.
4. Pick a difficulty (Easy 7 s / Medium 5 s / Hard 3 s) and press **Start Game**.
5. Click **Allow** when the browser asks for camera access.
6. After a 3-2-1 countdown, copy each emoji and hold it for 1 second. 10 rounds, +10 points per match.
7. See your final score and high score. **Play again** or change difficulty.

**Demo tips:** sit facing a light source (not a window behind you), exaggerate expressions, and
use full-screen (F11). If the camera fails, the game offers a keyboard fallback (keys 1–5 = 😀 😢 😠 😲 😐).

## The AI in one page

- **Library:** [face-api.js](https://github.com/vladmandic/face-api) (maintained fork) on **TensorFlow.js**, running in the browser via WebGL.
- **Model 1 – TinyFaceDetector:** a small convolutional network that finds the face bounding box in each frame (fast enough for ~10–15 fps on a laptop).
- **Model 2 – FaceExpressionNet:** a CNN that takes the cropped face and outputs a **probability for 7 expressions** (neutral, happy, sad, angry, fearful, disgusted, surprised) summing to 1. It was trained on labelled facial-expression photo datasets.
- **Game logic:** probabilities are smoothed with a moving average; a round is won when the target emotion is the top prediction (≥45 %) continuously for 1 s. Fearful + disgusted are grouped as "Other" in the confidence bars.
- **Privacy:** frames are analysed in memory and discarded. No backend, no upload, no image storage. Only the high score (one number) is kept in `localStorage`.

## Ethics & limitations

- **Expression ≠ emotion.** The model detects facial patterns, not inner feelings; people fake, mask or hide emotions.
- **Bias.** Training data under-represents some ages, skin tones, genders and cultures, so accuracy varies between people; expression norms are also culture-dependent.
- **Lighting & hardware.** Dim/back-lit rooms, low-res webcams, glasses, masks, beards and head tilt reduce accuracy.
- **Confusable classes.** Neutral/sad/angry are often mixed up; subtle expressions are unreliable.
- **Responsible use.** Fine for a game; unsuitable for hiring, grading, policing or other high-stakes decisions, and any real product needs informed consent.
