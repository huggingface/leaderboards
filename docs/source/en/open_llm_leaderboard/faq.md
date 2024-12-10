# FAQ

## Submissions

**Q: Do you keep track of who submits models?** 

A: Yes, we store information about which user submitted each model in the requests files here. This helps us prevent spam and encourages responsible submissions. Users are accountable for their submissions, as the community can identify who submitted each model.

**Q: Can I submit a model that requires `trust_remote_code=True`?**

A: We only accept models that have been integrated into a stable version of the `transformers` library to ensure the safety and stability of code executed on our cluster.

**Q: Are models of type X supported?**

A: For now, submission is limited to models that are included in a stable version of the transformers library.

**Q: Can I evaluate my model with a chat template?**

A: Sure! When submitting a model, you can choose whether to evaluate it using a chat template, which activates automatically for chat models.

**Q: How can I track the status of my model submission?**

A: You can monitor your model's status by checking the [Request file here](https://huggingface.co/datasets/open-llm-leaderboard/requests) or viewing the queues above the submit form.

**Q: What happens if my model disappears from all queues?**

A: A model’s disappearance typically indicates a failure. You can find your model in [Requests dataset here](https://huggingface.co/datasets/open-llm-leaderboard/requests) and check its status.

**Q: What causes an evaluation failure?**

A: Failures often stem from submission issues such as corrupted files or configuration errors. Please review the steps in About tab before submitting. Occasionally, failures are due to hardware or connectivity issues on our end.

**Q: How do I report an evaluation failure?**

A: Please create an issue in the [Community section]([https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions), linking your model’s request file for further investigation. If the error is on our side, we will relaunch your model promptly.

*Do not re-upload your model under a different name as it will not resolve the issue.*


## Results

**Q: What information is available about my model's evaluation results?**

A: For each model, you can access:

- **Request File**: Status of the evaluation.
- **Contents Dataset:** A full dataset that contains information about all evaluated models.
- **Details Dataset**: Comprehensive breakdown of scores and task examples.

**Q: Why do some models appear multiple times in the leaderboard?**

A: Models may appear multiple times due to submissions under different commits or precision settings, like float16 and 4bit. You can check this by clicking on the `Model sha` and `Precision` buttons under “Select Columns to Display” section on the main page. For evaluation, precision helps to assess the impact of quantization. 

*Duplicates with identical precision and commit should be reported.*

**Q: What is model flagging?**

A: Flagging helps report models that have unfair performance on the leaderboard. For example,   models that were trained on the evaluation data, models that are copies of other models not attributed properly, etc. 

*If your model is flagged incorrectly, you can open a discussion [here](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions) and defend your case.*


## Searching for a model

**Q: How do I search for models in the leaderboard?**

A: The search bar provides powerful filtering capabilities with several advanced features:

**Multiple Term Search**
- Combine Searches: Use semicolons (;) to combine multiple independent search terms.
- Stacked Results: Each term after the semicolon adds results to the previous search, creating a union of results rather than filtering by intersection.

Example: `llama; 7b` will find models containing "llama" OR models containing "7b."

**Special Field Search**
Use the @ prefix to target specific fields:
- @architecture: - Search by model architecture.
- @license: - Filter by license type.
- @precision: - Filter by model precision.

Example: @architecture:llama @license:apache will find Llama models with an Apache license.

**Regex Support**
- Advanced Pattern Matching: Supports regular expressions for flexible search criteria.
- Automatic Detection: Regex mode is activated automatically when special regex characters are used.

Example: `llama-2-(7|13|70)b` matches `llama-2-7b`, `llama-2-13b`, and `llama-2-70b`.

**Combined Search**
- Combine and stack all features for precise results:

Example: `meta @architecture:llama; 7b @license:apache` will find:
- Models containing "meta" AND having the Llama architecture, OR
- odels containing "7b" AND having an Apache license.

**Real-Time Results**
- Dynamic Updates: The search is performed in real-time with debouncing for smooth performance.
- Highlighting: Results are visually emphasized in the table for easy identification.

## Editing submissions

**Q: How can I update or rename my submitted model?**

A: To update, open an issue with your model's exact name for removal from the leaderboard before resubmitting with the new commit hash. For renaming, check [community resources](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions/174) page and use @Weyaxi's tool to request changes, then link the pull request in a discussion for approval.

## Additional information

**Q: What does “Only Official Providers” button do?**

A: This button filters and displays models from a curated list of trusted and high-quality model providers. We have introduced it to help users easily identify and choose top-tier models. The current set of trusted authors includes well-known names such as EleutherAI, CohereForAI, MistralAI and many others.
The dataset is available [here](https://huggingface.co/datasets/open-llm-leaderboard/official-providers).

**Q: How can I view raw scores for each evaluation?**

A: The Leaderboard displays normalized scores by default to provide a fair comparison. Normalization adjusts scores so that the lower bound corresponds to the score of a random baseline, ensuring a fairer average. To view the non-normalized values, go to "table options", "Score Display", and click "Raw".

**Q: How are model categories differentiated?**

A: Categories are defined to reflect the specific training stages and methodologies applied to each model, ensuring comparisons are both fair and meaningful. Here's a breakdown of each category:

- **Pretrained Models:** These foundational models are initially trained on large datasets without task-specific tuning, serving as a versatile base for further development.
- **Continuously Pretrained Models:** These undergo additional training beyond initial pretraining to enhance their capabilities, often using more specialized data.
- **Fine-Tuned Models:** Specifically adjusted on targeted datasets, these models are optimized for particular tasks, improving performance in those areas.
- **Chat Models:** Tailored for interactive applications like chatbots, these models are trained with methods such as Instruction Fine Tuning or Reinforcement Learning from Human Feedback to handle conversational contexts effectively.
- **Merge Models:** Combining multiple models or methods, these can show superior test results but do not always apply for real-world situations.

**Q: What are the leaderboard's intended uses?**

A: The leaderboard is ideal for:

1. Viewing rankings and scores of open pretrained models.
2. Experimenting with various fine-tuning and quantization techniques.
3. Comparing the performance of specific models within their categories.

**Q: Why don't you have closed-source models?**

A: The leaderboard focuses on open-source models to ensure transparency, reproducibility, and fairness. Closed-source models can change their APIs unpredictably, making it difficult to guarantee consistent and accurate scoring. Additionally, we rerun all evaluations on our cluster to maintain a uniform testing environment, which isn't possible with closed-source models.

**Q: I have another problem, help!**

A: Please, open an issue in the discussion tab, and we'll do our best to help you in a timely manner :