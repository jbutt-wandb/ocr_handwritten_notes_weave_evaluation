# OCR Handwritten Notes - Evaluation

OCR evaluation harness for **likho** ([jbutt-wandb/ocr_handwritten_notes](https://github.com/jbutt-wandb/ocr_handwritten_notes)), the handwritten-notes-to-Markdown app. This repo builds a ground-truth eval dataset from real handwritten note pages, then benchmarks the app's vision models against it — using the app's exact OCR prompt and predict path — with [W&B Weave](https://weave-docs.wandb.ai/).

## What's here

| Path | Purpose |
|---|---|
| `images/` | Photos of handwritten note pages (the eval inputs) |
| `text/` | Hand-corrected Markdown transcriptions, paired to images by filename stem (`page_1.jpeg` ↔ `page_1.md`) — the ground truth |
| `create_eval_dataset.ipynb` | Pairs `images/` with `text/`, publishes a Weave `Dataset` named `ocr-eval`, and verifies the round-trip |
| `run_ocr_eval.ipynb` | Runs a Weave `Evaluation` over `ocr-eval` for each model in the app's registry, scores with jiwer, and publishes a leaderboard |
| `scratchpad.ipynb` | Experiments / scratch work |

## Relationship to likho

The eval is deliberately faithful to the app so results transfer:

- **Prompt** — vendored verbatim from likho's [`backend/prompts/ocr_prompts.py`](https://github.com/jbutt-wandb/ocr_handwritten_notes/blob/main/backend/prompts/ocr_prompts.py), assembled with `build_ocr_prompt(contains_latex=True, contains_diagrams=True)` and held constant across all models.
- **Models** — the app's registry of vision models served on [W&B Inference](https://docs.wandb.ai/guides/inference/) (Kimi K2.6 — the app default — plus Gemma 4 31B, Qwen 3.6 35B A3B, DeepSeek V4.1-Flash).
- **Predict path** — mirrors the app's `OCRModel.predict`: images downscaled to a max dimension of 1600 px, strict JSON-schema response format (`{"markdown": ...}`), `max_tokens=32768`, and the app's `clean_markdown` post-processing.

The only variable between eval runs is the model, so the Weave compare view is a direct answer to "which model should likho default to?"

## Scoring

[jiwer](https://jitsi.github.io/jiwer/) OCR metrics — WER, MER, WIL, WIP, CER — computed twice per row:

- **raw** — exact text, LaTeX and Markdown formatting included
- **normalized** — lowercased, punctuation stripped, whitespace collapsed (content accuracy without formatting noise)

Lower is better for WER/MER/WIL/CER; higher is better for WIP.

## Setup

Requires Python ≥ 3.14 and [uv](https://docs.astral.sh/uv/):

```bash
uv sync
```

Create a `.env` in the repo root:

```bash
ENTITY=<your-wandb-entity>
PROJECT=<your-wandb-project>
WANDB_API_KEY=<your-wandb-api-key>   # also used for W&B Inference
```

## Usage

1. Run `create_eval_dataset.ipynb` to publish the `ocr-eval` dataset to your Weave project. Every image in `images/` must have a matching `.md` in `text/` (the notebook asserts this).
2. Run `run_ocr_eval.ipynb` to evaluate each model against the dataset and publish the leaderboard.
3. Compare results at `https://wandb.ai/<ENTITY>/<PROJECT>/weave/evaluations`.

To grow the dataset, drop a new page photo in `images/`, add its corrected transcription with the same stem in `text/`, and re-run the dataset notebook.
