# Meeting Transcript & Summary Generator

A Python Jupyter notebook that turns a meeting recording into a clean, shareable HTML report.

**Version:** 1.0.0 · **Date:** 2026-08-06

## What it does

1. **Transcribes** the meeting audio — optimized for **Thai and English** (mixed-language aware) using [`faster-whisper`](https://github.com/SYSTRAN/faster-whisper).
2. **Summarizes** the meeting with Google Gemini (`gemini-2.5-flash`) into an executive summary, key points, decisions, and action items.
3. **Saves** a self-contained HTML report to a folder you pick in a dialog — defaulting to the machine's **real** `Downloads` folder (read from the Windows registry, so a Downloads folder moved to another drive like `D:\` is handled correctly).

## Requirements

- Python 3.10+
- [`ffmpeg`](https://ffmpeg.org/) on your system `PATH` (needed to decode mp3/m4a/etc.)
  - **Windows:** `winget install Gyan.FFmpeg` (then reopen the terminal)
  - **macOS:** `brew install ffmpeg`
  - **Linux:** `sudo apt install ffmpeg`
- A [Google Gemini API key](https://aistudio.google.com/apikey) for the summary step (optional — an offline fallback summary is produced without one).

## Setup

```bash
pip install -r requirements.txt
cp .env.example .env      # then edit .env and paste your GEMINI_API_KEY
```

Setting the key in `.env` is optional: if it isn't set, the notebook shows a **popup window**
to enter it (masked input) when the summary step runs.

## Usage

1. Open `Transcript_Generator.ipynb` in Jupyter / VS Code.
2. Run the cells top to bottom.
3. In **cell 3**, dialogs pop up to choose the **audio file** and the **output folder** (both required).
4. If no `GEMINI_API_KEY` is set, a popup asks for it during the summary step.
5. The finished report lands in the chosen folder as `meeting_summary_<name>_<timestamp>.html`.

## Configuration (cell 2)

| Setting            | Description                                        | Default        |
| ------------------ | -------------------------------------------------- | -------------- |
| `MODEL_SIZE`       | Whisper size: `tiny`…`large-v3`                    | `medium`       |
| `COMPUTE_TYPE`     | `int8` (CPU) or `float16` (GPU)                    | `int8`         |
| `LANGUAGE_HINT`    | `th`, `en`, or `None` (auto-detect)               | `None`         |
| `SUMMARY_MODEL`    | Gemini model (`gemini-flash-latest` / `gemini-pro-latest`); auto-falls back to an available model | `gemini-flash-latest` |
| `SUMMARY_LANGUAGE` | Summary output language: `th` or `en`             | `th`           |

## Privacy

- The `.env` file (your API key) and all audio/HTML files are git-ignored by default.
- Transcription runs locally; only the transcript text is sent to Gemini for summarization.

## License

MIT — see [LICENSE](LICENSE).
