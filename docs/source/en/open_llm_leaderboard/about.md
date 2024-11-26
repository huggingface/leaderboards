# About

With the plethora of large language models (LLMs) and chatbots being released week upon week, often with grandiose claims of their performance, it can be hard to filter out the genuine progress that is being made by the open-source community and which model is the current state of the art.

We wrote a release blog [here](https://huggingface.co/spaces/open-llm-leaderboard/blog) to explain why we introduced this leaderboard!

## Tasks

📈 We evaluate models on 6 key benchmarks using the [Eleuther AI Language Model Evaluation Harness](https://github.com/EleutherAI/lm-evaluation-harness) , a unified framework to test generative language models on a large number of different evaluation tasks.

- **IFEval** ([https://arxiv.org/abs/2311.07911](https://arxiv.org/abs/2311.07911)) – IFEval is a dataset designed to test a model's ability to follow explicit instructions, such as "include keyword x" or "use format y." The focus is on the model's adherence to formatting instructions rather than the content generated, allowing for the use of strict and rigorous metrics.
- **BBH (Big Bench Hard)** ([https://arxiv.org/abs/2210.09261](https://arxiv.org/abs/2210.09261)) – A subset of 23 challenging tasks from the BigBench dataset to evaluate language models. The tasks use objective metrics, are highly difficult, and have sufficient sample sizes for statistical significance. They include multistep arithmetic, algorithmic reasoning (e.g., boolean expressions, SVG shapes), language understanding (e.g., sarcasm detection, name disambiguation), and world knowledge. BBH performance correlates well with human preferences, providing valuable insights into model capabilities.
- **MATH** ([https://arxiv.org/abs/2103.03874](https://arxiv.org/abs/2103.03874)) – MATH is a compilation of high-school level competition problems gathered from several sources, formatted consistently using Latex for equations and Asymptote for figures. Generations must fit a very specific output format. We keep only level 5 MATH questions and call it MATH Lvl 5.
- **GPQA (Graduate-Level Google-Proof Q&A Benchmark)** ([https://arxiv.org/abs/2311.12022](https://arxiv.org/abs/2311.12022)) – GPQA is a highly challenging knowledge dataset with questions crafted by PhD-level domain experts in fields like biology, physics, and chemistry. These questions are designed to be difficult for laypersons but relatively easy for experts. The dataset has undergone multiple rounds of validation to ensure both difficulty and factual accuracy. Access to GPQA is restricted through gating mechanisms to minimize the risk of data contamination. Consequently, we do not provide plain text examples from this dataset, as requested by the authors.
- **MuSR (Multistep Soft Reasoning)** ([https://arxiv.org/abs/2310.16049](https://arxiv.org/abs/2310.16049)) – MuSR is a new dataset consisting of algorithmically generated complex problems, each around 1,000 words in length. The problems include murder mysteries, object placement questions, and team allocation optimizations. Solving these problems requires models to integrate reasoning with long-range context parsing. Few models achieve better than random performance on this dataset.
- **MMLU-PRO (Massive Multitask Language Understanding - Professional)** ([https://arxiv.org/abs/2406.01574](https://arxiv.org/abs/2406.01574)) – MMLU-Pro is a refined version of the MMLU dataset, which has been a standard for multiple-choice knowledge assessment. Recent research identified issues with the original MMLU, such as noisy data (some unanswerable questions) and decreasing difficulty due to advances in model capabilities and increased data contamination. MMLU-Pro addresses these issues by presenting models with 10 choices instead of 4, requiring reasoning on more questions, and undergoing expert review to reduce noise. As a result, MMLU-Pro is of higher quality and currently more challenging than the original.

For all these evaluations, a higher score is a better score. We chose these benchmarks as they test a variety of reasoning and general knowledge across a wide variety of fields in 0-shot and few-shot settings.

### Results

You can find:

- Detailed numerical results in the [`results` Hugging Face dataset](https://huggingface.co/datasets/open-llm-leaderboard/results/).
- Details on the input/outputs for the models in the `details` of each model, which you can access by clicking the 📄 emoji after the model name.
- Community queries and running status in the [`requests` Hugging Face dataset](https://huggingface.co/datasets/open-llm-leaderboard/requests).

If a model's name contains "Flagged", this indicates it has been flagged by the community, and should probably be ignored! Clicking the link will redirect you to the discussion about the model.

## Reproducibility

To reproduce our results, you can use our fork of [lm_eval](https://github.com/huggingface/lm-evaluation-harness/tree/main), as our PRs are not all merged in it at the moment.
```
git clone git@github.com:huggingface/lm-evaluation-harness.git
cd lm-evaluation-harness
git checkout main
pip install -e .
lm-eval --model_args="pretrained=<your_model>,revision=<your_model_revision>,dtype=<model_dtype>" --tasks=leaderboard  --batch_size=auto --output_path=<output_path>
```
**Attention:** For instruction models add the `--apply_chat_template` and `fewshot_as_multiturn` option.

**Note:** You can expect results to vary slightly for different batch sizes because of padding.

### **Task Evaluations and Parameters**

**IFEval**:

- Task: "IFEval"
- Measure: Strict Accuracy at Instance and Prompt Levels (`inst_level_strict_acc,none` and `prompt_level_strict_acc,none`)
- Shots: 0-shot for both Instance-Level Strict Accuracy and Prompt-Level Strict Accuracy
- num_choices: 0 for both Strict Accuracy at Instance and Prompt Levels.
  
**Big Bench Hard (BBH)**:

- Overview Task: "BBH"
- Shots: 3-shot for each subtask
- Measure: Normalized Accuracy across all subtasks (`acc_norm,none`)
- List of subtasks with `num_choices`:
    - BBH Sports Understanding, num_choices=2
    - BBH Tracking Shuffled Objects (Three Objects), num_choices=3
    - BBH Navigate, num_choices=2
    - BBH Snarks, num_choices=2
    - BBH Date Understanding, num_choices=6
    - BBH Reasoning about Colored Objects, num_choices=18
    - BBH Object Counting, num_choices=19 (should be 18 but we added a “0” choice)
    - BBH Logical Deduction (Seven Objects), num_choices=7
    - BBH Geometric Shapes, num_choices=11
    - BBH Web of Lies, num_choices=2
    - BBH Movie Recommendation, num_choices=6
    - BBH Logical Deduction (Five Objects), num_choices=5
    - BBH Salient Translation Error Detection, num_choices=6
    - BBH Disambiguation QA, num_choices=3
    - BBH Temporal Sequences, num_choices=4
    - BBH Hyperbaton, num_choices=2
    - BBH Logical Deduction (Three Objects), num_choices=3
    - BBH Causal Judgement, num_choices=2
    - BBH Formal Fallacies, num_choices=2
    - BBH Tracking Shuffled Objects (Seven Objects), num_choices=7
    - BBH Ruin Names, num_choices=6
    - BBH Penguins in a Table, num_choices=5
    - BBH Boolean Expressions, num_choices=2
    - BBH Tracking Shuffled Objects (Five Objects), num_choices=5

**Math Challenges**:

- Task: "Math Level 5"
- Measure: Exact Match (`exact_match,none`)
- Shots: 4-shot
- num_choices: 0

**Generalized Purpose Question Answering (GPQA)**:

- Task: "GPQA"
- Measure: Normalized Accuracy (`acc_norm,none`)
- Shots: 0-shot
- num_choices: 4

**MuSR**:

- Overview Task: "MuSR"
- Measure: Normalized Accuracy across all subtasks (`acc_norm,none`)
- MuSR Murder Mysteries: 0-shot, num_choices: 2
- MuSR Object Placement: 0-shot, num_choices: 5
- MuSR Team Allocation: 0-shot, num_choices: 3

**MMLU-PRO**:

- Task: "MMLU-PRO"
- Measure: Accuracy (`acc,none`)
- Shots: 5-shot
- num_choices: 10

