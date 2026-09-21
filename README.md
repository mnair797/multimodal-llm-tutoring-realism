# When Does Multimodal Context Help LLM Tutors? A Dialogue Move-Level Analysis

Anonymous repository for double-blind review (LAK27 / Journal of Learning Analytics submission).

This repository contains the full data processing, generation, and analysis
pipeline used in the paper. See `MULTIMODAL_EXAMPLES.md` for concrete
examples of the multimodal event signal used throughout the study.

## Pipeline overview

Scripts are numbered in the order they should be run:

| Script | Purpose | Input | Output |
|---|---|---|---|
| `01_build_segments.py` | Segments raw session logs into dialogue units bounded by canvas events | `all_sessions_segments.csv` | `session_level_dialog_segments_final.csv` |
| `02_extract_topics.py` | Extracts per-session math topics via a single LLM pass, for the topic-context prompting strategy | `all_sessions_segments.csv` | `extract_topics_from_transcript_final.csv` |
| `03_generate_llm_responses.py` | Generates tutor utterances under 4 LLMs x 3 prompting strategies x 2 conditions x 2 runs | segments + topics files | `llm_<model_tag>.csv` per model |
| `04_compute_embeddings.py` | Encodes generated and ground-truth utterances into sentence embeddings | `llm_<model_tag>.csv` | `.npy` embedding arrays + metadata CSV |
| `05_run_all_figures_and_tables.py` | Computes the Multimodal Advantage Index (MAI), runs all statistical tests, and produces every figure and table reported in the paper | embeddings + metadata | figures, tables, CSVs (see script header) |

Each script's docstring documents its exact inputs, outputs, and any
assumptions worth checking before running it on new data.

## Requirements

```
pandas
numpy
scipy
scikit-learn
sentence-transformers
matplotlib
seaborn
ollama          # for 02_extract_topics.py and 03_generate_llm_responses.py
tqdm
```

Models used for generation (`03_generate_llm_responses.py`) were served
locally via [Ollama](https://ollama.com): WizardLM-2 7B, Mistral 7B v0.3,
Qwen3-VL 8B, and Gemma3 27B.

## Data

The dataset used in this study consists of de-identified math tutoring
session transcripts and Canvas interaction logs, provided by a U.S.-based
high-dosage tutoring provider under the terms of the provider's data use
agreement. Because of this agreement, the raw and de-identified data
cannot be publicly redistributed alongside this code.

## Reproducing the paper's figures and tables

Once `04_compute_embeddings.py` has produced embeddings for all four
models, run:

```
python 05_run_all_figures_and_tables.py
```

This produces:
- The per-item MAI distribution figure (boxplot, median/IQR/range)
- The marginal MAI by model figure
- The combined MAI-by-strategy and MAI-by-tutor-cluster figure
- The aggregate similarity + MAI table (by model x prompting strategy)
- The MAI-by-tutor-response-cluster table, including Cohen's d effect sizes
- Shapiro-Wilk normality test results supporting the median/IQR reporting choice

See the script's module docstring for the full list of outputs and where
each one is used in the paper.

## Citation

[Anonymized for review]
