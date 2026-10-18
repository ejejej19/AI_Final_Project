# Towards Better Evaluation: A Multidimensional Framework for Assessing LLM-Generated Recipes
## Introduction
This repository documents our journey for the AI final project.
While our final research direction focuses on evaluating LLM-generated recipes beyond traditional text metrics, the entire process included multiple explorations, trial and errors -- all of which are preserved here as part of our learning record.

## Project Overview
In this project, we collected recipes for three popular Taiwanese dishes: braised pork rice, Taiwanese fried chicken, and stinky tofu from the iCook website. For each dish, we curated five recipe samples, and extracted their representative images.<br><br>
These images were then send to three large language models: ChatGPT, Gemini, and Claude with the prompts "根據圖片，請問這道料理的名稱？請列出這道料理需要的原料與烹飪步驟。" to generate full recipes (ingredients + instructions).<br><br>
To evaluate the accuracy and quality of the generated recipes, we applied both traditional and custom-designed evaluation methods:<br>
- **Text similarity metrics:** ROUGE, bert-base-chinese, and BERTScore.

- **Our proposed metric — Recipe-F1:** Inspired by the F1-score, this metric measures the overlap between generated and reference recipe content, considering both precision and recall.

- **User Study:** We designed a form-based survey to gather user feedback with the LLM-generated recipes.

This multidimensional evaluation approach helps us better understand the real-world applicability and limitations of recipe generation by LLMs.

## Early Exploration
We kept our earlier attempts as a record of how the project evolved:

- **BLIP visual question answering** (`previous_work/myBLIP`, `0512_try_blip_vqa_base_with_UI`): tried BLIP VQA with a Gradio UI to identify dishes and answer questions about them, and looked into fine-tuning.
- **iCook crawler** (`0520_icook_crawler`): scraped recipes and photos to build the dataset.
- **First metric experiments** (`0521_F1_BERTscore`): ingredient F1 and Jaccard, BLEU/ROUGE/BERTScore for steps, and a test of how ingredient order affects BERT similarity, on braised pork rice.
- **Meeting notes** (`MD_files`): proposal, discussions and setup notes.

## Results

For each of the 45 generated recipes, we checked which metric's score was closest to the user-survey score, and compared each metric's average with the survey average.

**Ingredients.** BERTScore was the closest metric for most samples (32 of 45). On average, BERTScore and Recipe-F1 were nearly tied as the closest to the survey.

| Metric | Closest to survey score (samples) | Average score |
| --- | --- | --- |
| BERTScore (F1) | 32 / 45 | 0.764 |
| Recipe-F1 (manual) | 9 / 45 | 0.767 |
| ROUGE-L | 4 / 45 | 0.251 |
| bert-base-chinese similarity | 0 / 45 | 0.921 |
| *User survey (reference)* | — | *0.655* |

**Cooking instructions.** BERTScore was again the closest metric for most samples (33 of 45), but Recipe-F1's average was by far the closest to the survey average.

| Metric | Closest to survey score (samples) | Average score |
| --- | --- | --- |
| BERTScore (F1) | 33 / 45 | 0.732 |
| Recipe-F1 (manual) | 8 / 45 | 0.684 |
| ROUGE-L | 4 / 45 | 0.162 |
| bert-base-chinese similarity | 0 / 45 | 0.903 |
| *User survey (reference)* | — | *0.675* |

In both cases, plain BERT similarity rated recipes far higher than people did, and ROUGE-L far lower.

## Repository structure

| Path | Contents |
| --- | --- |
| `src/` | Evaluation notebooks: ROUGE, `bert-base-chinese` similarity, BERTScore |
| `results/` | Metric scores for ingredients and instructions, Recipe-F1 scoring sheets, and anonymized survey responses |
| `previous_work/` | Earlier explorations (see above) |

## Run the evaluation

The notebooks read `ground_truth.json` and `response_gpt.json`, `response_gemini.json`, `response_claude.json` from the working directory.

```bash
pip install rouge-score bert-score transformers torch scikit-learn
jupyter notebook src/
```

## Team
| Member | Contributions |
| --- | --- |
| 李尹瑄 | BLIP-VQA exploration, crawler and data collection, Recipe-F1 and BERT evaluation, stinky tofu ingredient evaluation, user study, video recording |
| 陳奕潔 | BERT evaluation, fried chicken data collection, ChatGPT responses generate, construct ingredient confusion table, fried chicken ingredient and instruction evaluation, stinky tofu user study, video recording |
| 陳璽安 | BERTScore evaluation, braised pork rice data collection, Claude and ChatGPT responses generate, construct cooking-instruction confusion table, braised pork rice ingredient and instruction evaluation, user study, video recording | 
| 劉維凡 | Stinky tofu data collection, Gemini responses generate, human labeling, Google Forms scripts, video editing |

Final presentation: [slides](https://docs.google.com/presentation/d/1vSrX-mGXMiXG0kDOw2nF9EXQY7VtYguvXYy0gnwYNiI/edit?usp=sharing)