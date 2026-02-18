<p align="center">
  <img src="assets/EACL2026_alt-logo_blue.png" alt="EACL 2026 Logo" height="80"/>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="assets/mbzuai-logo.webp" alt="MBZUAI Logo" height="80"/>
</p>

# Do LLMs Model Human Linguistic Variation? A Case Study in Hindi-English Verb Code-Mixing

**Mukund Choudhary\***, **Madhur Jindal\***, Gaurja Aeron, Monojit Choudhury

---

## 📄 Paper

**Published at EACL 2026:** [paper, poster, and video!](https://drive.google.com/drive/folders/1UiipNERjKUvjC3ho4cw1HXXmTvZpDZxN?usp=drive_link)

---

## 🔍 Overview

We ask: **do LLMs model human linguistic variation?** and test this on Hindi-English (*Hinglish*) verb code-mixing. It is interesting because native speakers can replace a Hindi verb with an English noun followed by the light verb *karna* ('do') and keep the rest of the sentence intact and grammatically accurate. However, they show unexplained, systematic preferences about which verbs to mix and which ones are awkward to. We compare native Hinglish-speaking bilinguals' acceptability ratings for 30 verb pairs to LLM perplexity ratios from 7 open-weight models (across sizes, families, and pre-training mixes) and find that current LLMs do not reliably model native speaker preferences and variation.

---

## 📁 Repository Contents

This repository releases the experimental data described in the paper. All files are in `main_data/`.

### `verb_reliability_metrics.csv`
**§ 4.2.2 · § 5.1** — *Per-verb inter-rater reliability for the 30 human-study verb pairs.*

30 rows (one per verb pair). Contains the mean rating, standard deviation, per-rater difficulty scores, rater IDs, percent exact agreement with the mode, and a Mann-Whitney U test p-value (`mt_pval`) for rating reliability. These are the 30 Hindi-English verb pairs selected for the human experiment via k-means stratification (§ 4.2.2) and used to define preference classes UN / HIP / ENP / MP (§ 5.1).

| Column | Description |
|---|---|
| `verb` | Hindi verb (Devanagari) |
| `verb_en` | English translation |
| `n_raters` | Number of ratings collected |
| `mean_rating` | Mean 7-point preference score (1=English preferred, 7=Hindi preferred) |
| `std_dev` / `mean_abs_dev` | Rating spread |
| `difficulties` | Per-rater scores |
| `raters` | Anonymised rater IDs |
| `pct_exact_match_with_mode` | % raters agreeing with modal response |
| `mt_pval` | Mann-Whitney U test p-value against uniform distribution |

---

### `sentence_reliability_metrics.csv`
**§ 4.2.3 · § 5.1** — *Per-sentence reliability for the 100 human-rated sentence pairs.*

100 rows (one per sentence pair; 30 verb pairs × up to 5 frames, subsetted). Contains item-level rating statistics and two reliability p-values: `sent_mt_pval` (sentence-level Mann-Whitney against uniform) and `verb_mt_pval` (verb-level reliability collapsed across frames). These support the inter-rater reliability analysis (ordinal Krippendorff's α = 0.365) and Monte Carlo uniformity test reported in § 5.1.

| Column | Description |
|---|---|
| `sentence_col1` / `sentence_col2` | Code-mixed and formal Hindi sentence pair |
| `n_raters`, `mean_rating`, `std_dev`, `mean_abs_dev` | Rating statistics |
| `difficulties`, `raters` | Per-rater raw scores and IDs |
| `pct_exact_match_with_mode` | Modal agreement rate |
| `verb`, `verb_en`, `verb_casual` | Verb in Devanagari, English, and casual Latin |
| `frame_en`, `frame_en_id` | English gloss of the frame and frame index (1–5) |
| `sent_mt_pval` | Sentence-level Mann-Whitney p-value |
| `verb_mt_pval` | Verb-level Mann-Whitney p-value |

---

### `merged_human_llm_data_with_sentence_reliability.csv`
**§ 4.1.2 · § 4.2.3 · § 5** — *Main analysis table: human ratings merged with LLM perplexity ratios.*

300 rows (100 sentence pairs × 3 orthographic forms: Devanagari, Casual Latin, Formal Latin). This is the primary dataset used in all three experiments (§ 5.2–5.4). Each row links one sentence pair in one form to its human preference statistics (from `sentence_reliability_metrics`) and the perplexity ratio PR_θ(x_hi, x_en) from each of the 7 LLMs. Also includes preference class labels (DistClass / DistClassName: UN, HIP, ENP, MP) derived from the Monte Carlo test (§ 5.1).

| Column | Description |
|---|---|
| `sentence_col1` / `sentence_col2` | Sentence pair |
| `n_raters`, `mean_rating`, `std_dev`, etc. | Human rating statistics |
| `verb`, `verb_en`, `verb_casual` | Verb forms |
| `frame_en`, `frame_en_id` | Frame gloss and index |
| `sent_mt_pval`, `verb_mt_pval` | Reliability p-values |
| `DistClass`, `DistClassName` | Numeric and named preference class (§ 5.1) |
| `translit` | Orthographic form (`devanagari` / `casual` / `formal`) |
| `google_gemma-3-{1,4,12}b-pt` | PR_θ for Gemma-3 variants |
| `meta-llama_Llama-3.1-{8,70}B` | PR_θ for Llama-3.1 variants |
| `Qwen_Qwen3-32B` | PR_θ for Qwen3-32B |
| `sarvamai_sarvam-1` | PR_θ for Sarvam-1 |

---

### `verbmap_with_perplexities_ratios_full_expanded.csv`
**§ 3.1 · § 4.1.2** — *Full-scale perplexity ratios for all 4,279 verb pairs across 7 models × 15 conditions.*

4,207 rows (one per verb pair, after preprocessing; cf. 4,279 unique pairs from Indowordnet described in § 3.1). Each row contains a Hindi-English verb pair in all three transliterations, plus — for each of the 7 LLMs — the full array of 15 perplexity ratios (5 frames × 3 forms) and per-model summary statistics (min, max, mean, median, std, range). These 18-dimensional per-model feature vectors were used for k-means clustering to stratify the 30-verb human study sample (§ 4.2.2), and the 15-condition PR_θ values serve as features in Experiments 1, 2(a), and 2(b) (§ 5.2–5.4).

| Column | Description |
|---|---|
| `hindi_verb` | Hindi verb (Devanagari) |
| `casual_transcription` | Casual Latin romanisation |
| `formal_transcription` | Formal (rule-based) Latin romanisation |
| `english translation` | English verb |
| `{model}_perplexity_{0..14}` | PR_θ for each of the 15 conditions (5 frames × 3 forms) |
| `{model}_min/max/mean/median/std/range` | Summary statistics across 15 conditions |

Models: `google_gemma-3-1b-pt`, `google_gemma-3-4b-pt`, `google_gemma-3-12b-pt`, `meta-llama_Llama-3.1-8B`, `meta-llama_Llama-3.1-70B`, `Qwen_Qwen3-32B`, `sarvamai_sarvam-1`

---

## 🤝 Contact

Mukund Choudhary — [mukund.choudhary@mbzuai.ac.ae](mailto:mukund.choudhary@mbzuai.ac.ae)  
Madhur Jindal — [madhur.jindal@mbzuai.ac.ae](mailto:madhur.jindal@mbzuai.ac.ae)

Questions about the data or methodology are welcome. If you use this dataset or build on this work, we'd love to hear from you.

---

## ✍️ Citation

```bibtex
@inproceedings{choudhary2025llms,
  title     = {Do {LLM}s Model Human Linguistic Variation? {A} Case Study in {H}indi-{E}nglish Verb Code-Mixing},
  author    = {Choudhary, Mukund and Jindal, Madhur and Aeron, Gaurja and Choudhury, Monojit},
  booktitle = {Proceedings of the 17th Conference of the European Chapter of the Association for Computational Linguistics (EACL)},
  year      = {2026},
}
```

---

## Acknowledgements

This research was supported in part by the Microsoft Azure Foundation Model Research (AFMR) Grant. We thank all participants in the human preference study.
