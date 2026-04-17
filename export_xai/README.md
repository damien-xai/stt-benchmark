# Benchmark Export: xai

This export contains all benchmark data for xai to enable independent verification.

## Contents

- `xai_results.csv` - All results in CSV format
- `xai_results.json` - All results in JSON format with metadata

## Data Fields

| Field | Description |
|-------|-------------|
| `sample_id` | Unique identifier for the audio sample |
| `dataset_index` | Index in the source dataset (pipecat-ai/smart-turn-data-v3.1-train) |
| `audio_duration_seconds` | Duration of the audio sample |
| `ground_truth` | Ground truth transcription (generated via Gemini with human review) |
| `transcription` | Transcription returned by xai |
| `normalized_reference` | Normalized ground truth (for WER calculation) |
| `normalized_hypothesis` | Normalized transcription (for WER calculation) |
| `wer` | Semantic Word Error Rate (0.0 = perfect, 1.0 = 100% errors) |
| `substitutions` | Number of word substitutions |
| `deletions` | Number of word deletions |
| `insertions` | Number of word insertions |
| `reference_words` | Total words in normalized reference |
| `ttfb_seconds` | Time to first byte (latency) |

## Semantic WER Methodology

We use **Semantic WER**, which only counts errors that would impact how an LLM agent understands the user's intent.

**Counted as errors:**
- Word substitutions that change meaning
- Nonsense/hallucinated words
- Missing words that change intent
- Wrong names, numbers, negations

**NOT counted as errors:**
- Punctuation and capitalization differences
- Contractions ("don't" → "do not")
- Singular/plural variations
- Filler words ("um", "uh")
- Number format differences ("5" vs "five")

## Verification Steps

1. **Verify transcriptions**: Compare the `transcription` field against your service's logs
2. **Verify ground truth**: Listen to samples and verify `ground_truth` is accurate
3. **Recalculate WER**: Use your own WER calculation on `normalized_reference` vs `normalized_hypothesis`
4. **Identify disputes**: Note any `sample_id` values you want to discuss

## Audio Access

Audio samples are from the public dataset: `pipecat-ai/smart-turn-data-v3.1-train`

You can access them via HuggingFace:
```python
from datasets import load_dataset
ds = load_dataset("pipecat-ai/smart-turn-data-v3.1-train")
# Use dataset_index to find specific samples
```

## Sample Count

This export contains **100** samples.

## Questions?

If you have questions about specific samples or methodology, please reference the `sample_id` when discussing.
