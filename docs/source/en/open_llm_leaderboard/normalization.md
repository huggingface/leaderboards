# Scores Normalization

This page explains how scores are normalized on the Open LLM Leaderboard for the six presented benchmarks. We can categorize all tasks into those with subtasks, those without subtasks, and generative evaluation.

## What is Normalization?
Normalization is the process of adjusting values measured on different scales to a common scale, making it possible to compare scores across different tasks. For the Open LLM Leaderboard, we normalize scores to:

1. Account for the varying difficulty and random guess baselines of different tasks.
2. Provide a consistent scale (0-100) for all tasks, enabling fair comparisons.
3. Ensure that improvements over random guessing are appropriately reflected in the scores.


## General Normalization Process

The basic normalization process involves two steps:
1. Subtracting the random baseline score (lower bound).
2. Scaling the result to a range of 0-100.

We use the following normalization function:

```python
def normalize_within_range(value, lower_bound, higher_bound):
    return (value - lower_bound) / (higher_bound - lower_bound)
```

## Normalizing Tasks without Subtasks
For tasks without subtasks (e.g., GPQA, MMLU-PRO), the normalization process is straightforward:
- Determine the lower bound (random guess baseline).
- Apply the normalization function.
- Scale to a percentage.

### Example: Normalizing GPQA Scores
GPQA has 4 `num_choices`, so the lower bound is 0.25 (1/`num_choices` = 1/4 = 0.25).

```python
raw_score = 0.6  # Example raw score
lower_bound = 0.25
higher_bound = 1.0

if raw_score < lower_bound:
    normalized_score = 0
else:
    normalized_score = normalize_within_range(raw_score, lower_bound, higher_bound) * 100

print(f"Normalized GPQA score: {normalized_score:.2f}")
# Output: Normalized GPQA score: 46.67
```

## Normalizing Tasks with Subtasks
For tasks with subtasks (e.g., MUSR, BBH), we follow these steps:
- Calculate the lower bound for each subtask.
- Normalize each subtask score.
- Average the normalized subtask scores.

### Example: Normalizing MUSR Scores

MUSR has three subtasks with different numbers of choices:
| Subtask               | Choices | Lower Bound |
|-----------------------|---------|-------------|
| MUSR murder mysteries | 2       | 0.5         |
| MUSR object placement | 5       | 0.2         |
| MUSR team allocation  | 3       | 0.333       |

```python
subtasks = [
    {"name": "murder_mysteries", "raw_score": 0.7, "lower_bound": 0.5},
    {"name": "object_placement", "raw_score": 0.4, "lower_bound": 0.2},
    {"name": "team_allocation", "raw_score": 0.6, "lower_bound": 0.333}
]

normalized_scores = []

for subtask in subtasks:
    if subtask["raw_score"] < subtask["lower_bound"]:
        normalized_score = 0
    else:
        normalized_score = normalize_within_range(
            subtask["raw_score"], 
            subtask["lower_bound"], 
            1.0
        ) * 100
    normalized_scores.append(normalized_score)
    print(f"{subtask['name']} normalized score: {normalized_score:.2f}")

overall_normalized_score = sum(normalized_scores) / len(normalized_scores)
print(f"Overall normalized MUSR score: {overall_normalized_score:.2f}")

# Output:
# murder_mysteries normalized score: 40.00
# object_placement normalized score: 25.00
# team_allocation normalized score: 40.00
# Overall normalized MUSR score: 35.00
```

## Generative Evaluations
Generative evaluations like MATH and IFEval require a different approach:
1. **MATH:** Uses exact match accuracy. The lower bound is effectively 0, as random guessing is unlikely to produce a correct answer.
2. IFEval:
    - For instance-level evaluation (`ifeval_inst`), we use strict accuracy.
    - For prompt-level evaluation (`ifeval_prompt`), we also use strict accuracy.
    - The lower bound for both is 0, as random generation is unlikely to produce correct answers.

For these tasks, the normalization process in the code is as follows:

```python
for subtask in subtasks:
    subtask_val = subtask.value
    value = data["results"][subtask_val.benchmark][subtask_val.metric]
    higher_bound = 1
    lower_bound = higher_bound / subtask_val.num_choices if subtask_val.num_choices > 0 else 0
    if value < lower_bound:
        normalized_score = 0
    else:
        normalized_score = normalize_within_range(
            value=value, 
            lower_bound=lower_bound,
            higher_bound=higher_bound
        )
    normalized_scores.append(normalized_score * 100)
```
This approach ensures that even for generative tasks, we can provide normalized scores that are comparable across different evaluations.

## Further Information
For more detailed information and examples, please refer to our [blog post](https://huggingface.co/spaces/open-llm-leaderboard/blog) on scores normalization.

If you have any questions or need clarification, please start a new discussion on [the Leaderboard page](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions).