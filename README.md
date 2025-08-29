# Call Quality Analyzer — Colab Notebook

This Colab notebook analyzes a sales call and outputs:
- talk-time ratio (per speaker)
- number of questions
- longest monologue (sec)
- call sentiment (positive/neutral/negative)
- one actionable insight
- (bonus) heuristic sales rep vs customer label

## How to run
1. Open this notebook in Google Colab.
2. Runtime → Run all (no inputs required; the notebook downloads the test audio and runs).
3. Results are printed and saved as `call_quality_results.json`.

## 200-word approach
We build a lightweight call-analysis pipeline optimized for the free Colab tier. Audio is fetched from YouTube with `yt-dlp`, normalized to 16kHz mono WAV via `ffmpeg` for ASR robustness. Transcription uses `faster-whisper (tiny.en, int8)` with VAD for speed and resilience; this yields timestamped segments. For speaker separation, we compute 1.5s overlapping window embeddings using `resemblyzer` and cluster them with `KMeans(2)`. Merging adjacent windows produces approximate speaker turns, from which we compute talk-time ratios and the longest monologue. Questions are counted by combining literal `?` detection and a heuristic for interrogative sentence starts to handle missing punctuation. Sentiment is derived via VADER (NLTK), mapped to positive/neutral/negative. The notebook produces a rule-based actionable insight (balance talk-time, increase questions, or address objections) and saves results to JSON. The code is commented and tuned to run quickly (<30s) on the free Colab tier.
