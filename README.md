# medi-LLaMA

`medi-LLaMA` is a medical-domain language model fine-tuning project built around supervised instruction tuning and preference optimization. The project uses a small LLaMA base model and MedCRAFT medical datasets to evaluate whether fine-tuning improves medical question-answering quality, factual relevance, and response alignment.

## Project Overview

The repository contains a complete fine-tuning and evaluation pipeline for a small medical QA assistant. It includes dataset preparation, baseline evaluation, supervised fine-tuning, preference fine-tuning, and comparison of model outputs using automatic evaluation metrics.

The project uses:

- Base model: `TinyLlama/TinyLlama_v1.1`

- SFT dataset: `sherry0213/MedCRAFT` SFT split

- DPO dataset: `sherry0213/MedCRAFT` DPO split

- Evaluation set: 10 manually selected medical prompts with reference answers

- Metrics: BLEU and BERTScore

## Pipeline

The project is organized into four main stages:

1. **Data Preparation and Baseline Evaluation**

   Loads and preprocesses MedCRAFT SFT/DPO data, prepares the manual test set, generates baseline responses from `TinyLlama/TinyLlama_v1.1`, and records BLEU/BERTScore.

2. **Supervised Fine-Tuning**

   Runs multiple LoRA-based SFT trials using different hyperparameter configurations. Each trial is evaluated on the same 10 medical prompts.

3. **Preference Fine-Tuning**

   Uses the best SFT model as the starting point and applies DPO training using MedCRAFT preference pairs.

4. **Final Comparison**

   Compares baseline, SFT, and DPO models using BLEU, BERTScore, validation loss, and qualitative response examples.

## Datasets

### MedCRAFT SFT

The SFT split contains medical instructions and reference responses. It is used to teach the base model how to follow medical instructions and produce clear, domain-specific answers.

Main columns:

```
instruction
response
diff
```

### MedCRAFT DPO

The DPO split contains medical prompts with preferred and rejected responses. It is used to improve response preference alignment after SFT.

Main columns:

```
prompt
chosen
rejected
```

## Evaluation

All models are evaluated on the same 10 manually selected medical prompts. Each prompt has a reference answer generated separately and stored in `test_prompts.json`.

The main evaluation metrics are:

* **BLEU:** measures lexical overlap with the reference answer.

* **BERTScore:** measures semantic similarity with the reference answer.

Final model selection is based on BLEU and BERTScore, with validation loss used as a secondary criterion when scores are close.

## Notes

This project is for educational experimentation with medical-domain language model fine-tuning. Model outputs should not be treated as professional medical advice.