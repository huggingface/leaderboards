# Scores Normalization

This page explains how scores are normalized in our leaderboard, particularly for tasks with subtasks like MUSR and BBH.

## General Normalization Process

For tasks without subtasks (e.g., GPQA, MMLU-PRO), normalization is straightforward:
1. Subtract the random score
2. Remap to the range (0, 1)

For tasks with subtasks (e.g., MUSR, BBH), the process is more complex.

## Normalizing Tasks with Subtasks

To normalize results for tasks with subtasks, follow these steps:

1. Define a normalization function:

```
def normalize_within_range(value, lower_bound, higher_bound):
    return (value - lower_bound) / (higher_bound - lower_bound)
```

2. Calculate the lower bound for each subtask:
The lower bound is the score you would get with a random baseline, which is the reciprocal of the number of choices for each subtask.

Example for MUSR:
| Subtask               | Choices | Lower Bound |
|-----------------------|---------|-------------|
| MUSR murder mysteries | 2       | 0.5         |
| MUSR object placement | 5       | 0.2         |
| MUSR team allocation  | 3       | 0.333       |

You can find the number of choices for other benchmarks [here](https://huggingface.co/docs/leaderboards/open_llm_leaderboard/about) in our documentation.

3. Normalize the raw scores for each subtask:
    - If the raw score is below the lower bound, set the normalized score to 0.
    - Otherwise, apply the normalization function and scale it to a percentage:
    ```
    if raw_score < lower_bound:
        normalized_score = 0
    else:
        normalized_score = normalize_within_range(raw_score, lower_bound, 1) * 100
    ```

4. Average the normalized scores across subtasks to obtain the overall normalized score for the task.

## Example: Normalizing MUSR Scores

Here's a step-by-step example of how to normalize MUSR scores:

1. Calculate lower bounds for each subtask (as shown above).
2. For each subtask:
    - Apply the normalization function to the raw score.
    - If the result is negative, set it to 0.
    - Scale the result to a percentage.
3. Calculate the average of the normalized subtask scores.

This process ensures that scores are comparable across different tasks and subtasks, regardless of the number of choices or difficulty level.

## Further Information
For more detailed information and examples, please refer to our [blog post](https://huggingface.co/spaces/open-llm-leaderboard/blog) on scores normalization.

If you have any questions or need clarification, please start a new discussion on [the Leaderboard page](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions).