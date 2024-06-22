# About

With the plethora of large language models (LLMs) and chatbots being released week upon week, often with grandiose claims of their performance, it can be hard to filter out the genuine progress that is being made by the open-source community and which model is the current state of the art.
🤗 Submit a model for automated evaluation on the 🤗 GPU cluster on the "Submit" page!
The leaderboard's backend runs the great [Eleuther AI Language Model Evaluation Harness](https://github.com/EleutherAI/lm-evaluation-harness) - read more details below!

## Tasks

📈 We evaluate models on 6 key benchmarks using the <a href="https://github.com/EleutherAI/lm-evaluation-harness" target="_blank">  Eleuther AI Language Model Evaluation Harness </a>, a unified framework to test generative language models on a large number of different evaluation tasks.
- <a href="https://arxiv.org/abs/1803.05457" target="_blank">  AI2 Reasoning Challenge </a> (25-shot) - a set of grade-school science questions.
- <a href="https://arxiv.org/abs/1905.07830" target="_blank">  HellaSwag </a> (10-shot) - a test of commonsense inference, which is easy for humans (~95%) but challenging for SOTA models.
- <a href="https://arxiv.org/abs/2009.03300" target="_blank">  MMLU </a>  (5-shot) - a test to measure a text model's multitask accuracy. The test covers 57 tasks including elementary mathematics, US history, computer science, law, and more.
- <a href="https://arxiv.org/abs/2109.07958" target="_blank">  TruthfulQA </a> (0-shot) - a test to measure a model's propensity to reproduce falsehoods commonly found online. Note: TruthfulQA is technically a 6-shot task in the Harness because each example is prepended with 6 Q/A pairs, even in the 0-shot setting.
- <a href="https://arxiv.org/abs/1907.10641" target="_blank">  Winogrande </a> (5-shot) - an adversarial and difficult Winograd benchmark at scale, for commonsense reasoning.
- <a href="https://arxiv.org/abs/2110.14168" target="_blank">  GSM8k </a> (5-shot) - diverse grade school math word problems to measure a model's ability to solve multi-step mathematical reasoning problems.
For all these evaluations, a higher score is a better score.
We chose these benchmarks as they test a variety of reasoning and general knowledge across a wide variety of fields in 0-shot and few-shot settings.

## Results

You can find:
- detailed numerical results in the `results` Hugging Face dataset: https://huggingface.co/datasets/open-llm-leaderboard/results
- details on the input/outputs for the models in the `details` of each model, which you can access by clicking the 📄 emoji after the model name
- community queries and running status in the `requests` Hugging Face dataset: https://huggingface.co/datasets/open-llm-leaderboard/requests
If a model's name contains "Flagged", this indicates it has been flagged by the community, and should probably be ignored! Clicking the link will redirect you to the discussion about the model.

## Reproducablity
To reproduce our results, here are the commands you can run, using [this version](https://github.com/EleutherAI/lm-evaluation-harness/tree/b281b0921b636bc36ad05c0b0b0763bd6dd43463) of the Eleuther AI Harness:

```bash
python main.py --model=hf-causal-experimental \
    --model_args="pretrained=<your_model>,use_accelerate=True,revision=<your_model_revision>" \
    --tasks=<task_list> \
    --num_fewshot=<n_few_shot> \
    --batch_size=1 \
    --output_path=<output_path>
```

**Note:** We evaluate all models on a single node of 8 H100s, so the global batch size is 8 for each evaluation. If you don't use parallelism, adapt your batch size to fit.
*You can expect results to vary slightly for different batch sizes because of padding.*
The tasks and few shots parameters are:
- ARC: 25-shot, *arc-challenge* (`acc_norm`)
- HellaSwag: 10-shot, *hellaswag* (`acc_norm`)
- TruthfulQA: 0-shot, *truthfulqa-mc* (`mc2`)
- MMLU: 5-shot, *hendrycksTest-abstract_algebra,hendrycksTest-anatomy,hendrycksTest-astronomy,hendrycksTest-business_ethics,hendrycksTest-clinical_knowledge,hendrycksTest-college_biology,hendrycksTest-college_chemistry,hendrycksTest-college_computer_science,hendrycksTest-college_mathematics,hendrycksTest-college_medicine,hendrycksTest-college_physics,hendrycksTest-computer_security,hendrycksTest-conceptual_physics,hendrycksTest-econometrics,hendrycksTest-electrical_engineering,hendrycksTest-elementary_mathematics,hendrycksTest-formal_logic,hendrycksTest-global_facts,hendrycksTest-high_school_biology,hendrycksTest-high_school_chemistry,hendrycksTest-high_school_computer_science,hendrycksTest-high_school_european_history,hendrycksTest-high_school_geography,hendrycksTest-high_school_government_and_politics,hendrycksTest-high_school_macroeconomics,hendrycksTest-high_school_mathematics,hendrycksTest-high_school_microeconomics,hendrycksTest-high_school_physics,hendrycksTest-high_school_psychology,hendrycksTest-high_school_statistics,hendrycksTest-high_school_us_history,hendrycksTest-high_school_world_history,hendrycksTest-human_aging,hendrycksTest-human_sexuality,hendrycksTest-international_law,hendrycksTest-jurisprudence,hendrycksTest-logical_fallacies,hendrycksTest-machine_learning,hendrycksTest-management,hendrycksTest-marketing,hendrycksTest-medical_genetics,hendrycksTest-miscellaneous,hendrycksTest-moral_disputes,hendrycksTest-moral_scenarios,hendrycksTest-nutrition,hendrycksTest-philosophy,hendrycksTest-prehistory,hendrycksTest-professional_accounting,hendrycksTest-professional_law,hendrycksTest-professional_medicine,hendrycksTest-professional_psychology,hendrycksTest-public_relations,hendrycksTest-security_studies,hendrycksTest-sociology,hendrycksTest-us_foreign_policy,hendrycksTest-virology,hendrycksTest-world_religions* (average of all the results `acc`)
- Winogrande: 5-shot, *winogrande* (`acc`)
- GSM8k: 5-shot, *gsm8k* (`acc`)
Side note on the baseline scores: 
- for log-likelihood evaluation, we select the random baseline
- for GSM8K, we select the score obtained in the paper after finetuning a 6B model on the full GSM8K training set for 50 epochs

## Resources

### Quantization
To get more information about quantization, see:
- 8 bits: [blog post](https://huggingface.co/blog/hf-bitsandbytes-integration), [paper](https://arxiv.org/abs/2208.07339)
- 4 bits: [blog post](https://huggingface.co/blog/4bit-transformers-bitsandbytes), [paper](https://arxiv.org/abs/2305.14314)

### Useful links
- [Community resources](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions/174)
- [Collection of best models](https://huggingface.co/collections/open-llm-leaderboard/llm-leaderboard-best-models-652d6c7965a4619fb5c27a03)

### Other cool leaderboards
- [LLM safety](https://huggingface.co/spaces/AI-Secure/llm-trustworthy-leaderboard)
- [LLM performance](https://huggingface.co/spaces/optimum/llm-perf-leaderboard)
