# Museum Audio Description with LLaVA-1.5-7B

### A pilot comparison of zero-shot and Snyder-guided prompting

This repository accompanies a pilot study of **AI-generated museum audio description (AD)**. It investigates whether providing a small open vision–language model with detailed instructions adapted from Joel Snyder's objective AD guidelines changes the linguistic characteristics and image-grounded accuracy of its descriptions.

The experiment compares **two prompts applied to the same 12 artworks** using **LLaVA-1.5-7B**, without fine-tuning or external artwork metadata. The generated texts were evaluated using automatic linguistic measures, blind-coded sentence-level manual annotation by one researcher, and qualitative close reading.


## Research questions

1. How does a Snyder-based prompt (B) affect the linguistic features of VLM-generated ADs compared to a minimal zero-shot prompt (A)?
2. To what extent does the model follow Snyder’s guidelines when explicitly instructed to do so, and which aspects of objective AD remain problematic?

## Experimental design

| | Condition A | Condition B |
|---|---|---|
| Identifier | `A_zero_shot` | `B_snyder_prompt` |
| Prompt | `Describe this artwork. Be objective.` | Detailed role and 11 rules adapted from Snyder's AD guidance |
| Model | LLaVA-1.5-7B | Same model |
| Artworks | Same 12 images | Same 12 images |

The exact prompts, including the image placeholder and assistant formatting, are preserved in [`prompts.json`](prompts.json).

### Generation environment

The recorded run configuration (`run_config.json`) specifies:

- Model: `llava-hf/llava-1.5-7b-hf`
- GPU: NVIDIA Tesla T4
- Inference: 4-bit quantisation; `bf16: false`
- PyTorch: `2.14.1+cu130`; Transformers: `5.23.0`
- Seed: `42`; greedy decoding; `max_new_tokens: 320`
- Prompt B requests approximately 150 words, with a maximum of 200; this is a **prompt instruction**, not a guarantee about output length.

The 12 public-domain artworks were obtained from the Art Institute of Chicago's open-access collection. Credits and source links are recorded in `image_credits.csv`.

## Dataset and annotation

- **12 artworks**, each described under both conditions
- **24 generated descriptions** (12 per condition)
- **109 manually annotated sentences**: 54 in A and 55 in B
- **914 words in A** and **909 words in B**, according to the manual-annotation analysis's word-count convention

Each sentence was coded `1` (present) or `0` (absent) for eight **non-mutually-exclusive** categories:

| Code | Meaning |
|---|---|
| `objective` | Visible, verifiable content |
| `interpretive` | Inferred emotion, mood, intention, symbolism, or meaning |
| `evaluative` | Value judgment or praise |
| `spatial` | Location or orientation within the image |
| `colour` | Specific colour naming |
| `comparison` | Comparison or simile |
| `error` | Image-inconsistent or invented detail |
| `selection` | Content judged essential to understanding the artwork |


Automatic metrics, which use different lexical rules from manual annotation, are reported separately in `metrics_by_condition.csv` and `metrics_per_image.csv`. In particular, their spatial and colour measures should **not** be conflated with manual sentence-level rates.

## Repository contents

```text
.
├── README.md
├── run_config.json                  # Recorded generation configuration
├── prompts.json                     # Exact prompts for conditions A and B
├── images/                          # 12 artwork images, img_00–img_11
├── gallery.png                      # Contact sheet of artworks
├── image_credits.csv                # Artwork metadata and source URLs
├── image_credits.txt                # Human-readable image credits
├── ai_outputs.jsonl                 # Generated descriptions, long format
├── all_outputs.csv                  # Generated descriptions, wide format
├── metrics_by_condition.csv         # Aggregated automatic measures
├── metrics_per_image.csv            # Automatic measures by image
├── Objective AD_BRIDGE.py           # Python script 
├── annotation_sheet.csv             # Sentence-level annotation template
├── annotation_sheet_completed.csv   # Completed manual annotation
├── annotation_guide.txt             # Code definitions and instructions
├── annotation_key.json              # Mapping of blind tags to conditions
├── annotation_analysis_by_sentence.csv  # Annotation and checker analysis
└── table2.csv                       # Summary of manual annotation counts
```

## How to inspect the results

1. Read `run_config.json` and `prompts.json` to establish the experimental settings.
2. Match the artworks in `images/` to their credits in `image_credits.csv`.
3. Inspect the 24 model outputs in `ai_outputs.jsonl` or `all_outputs.csv`.
4. Consult `annotation_guide.txt`, then `annotation_sheet_completed.csv` for the manually assigned codes and notes.
5. Use `annotation_key.json` to identify conditions associated with the blinded text tags.
6. Consult `table2.csv` for the manual counts and `metrics_by_condition.csv` for the separate automated measures.


## Artwork rights and reuse

Artwork source information and rights statements are provided in `image_credits.csv`. The source images are recorded as public domain / CC0 through the Art Institute of Chicago Open Access programme. Users should consult the linked source records for provenance and reuse information. The repository's own code, text, and data are **not automatically assigned a licence** by the artworks' public-domain status; add a separate `LICENSE` file if you wish to specify reuse permissions.

