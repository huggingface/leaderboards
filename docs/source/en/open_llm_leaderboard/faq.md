# FAQ

## Submissions

My model requires `trust_remote_code=True`, can I submit it?
- *We only support models that have been integrated into a stable version of the `transformers` library for automatic submission, as we don't want to run possibly unsafe code on our cluster.*

What about models of type X? 
- *We only support models that have been integrated into a stable version of the `transformers` library for automatic submission.*
How can I follow when my model is launched?
- *You can look for its request file [here](https://huggingface.co/datasets/open-llm-leaderboard/requests) and follow the status evolution, or directly in the queues above the submit form.*

My model disappeared from all the queues, what happened?
- *A model disappearing from all the queues usually means that there has been a failure. You can check if that is the case by looking for your model [here](https://huggingface.co/datasets/open-llm-leaderboard/requests).*

What causes an evaluation failure?
- *Most of the failures we get come from problems in the submissions (corrupted files, config problems, wrong parameters selected for eval ...), so we'll be grateful if you first make sure you have followed the steps in `About`. However, from time to time, we have failures on our side (hardware/node failures, problems with an update of our backend, connectivity problems ending up in the results not being saved, ...).*

How can I report an evaluation failure?
- *As we store the logs for all models, feel free to create an issue, **where you link to the requests file of your model** (look for it [here](https://huggingface.co/datasets/open-llm-leaderboard/requests/tree/main)), so we can investigate! If the model failed due to a problem on our side, we'll relaunch it right away!* 
*Note: Please do not re-upload your model under a different name, it will not help*

## Results

What kind of information can I find?
- *Let's imagine you are interested in the Yi-34B results. You have access to 3 different information categories:*
      - *The [request file](https://huggingface.co/datasets/open-llm-leaderboard/requests/blob/main/01-ai/Yi-34B_eval_request_False_bfloat16_Original.json): it gives you information about the status of the evaluation*
      - *The [aggregated results folder](https://huggingface.co/datasets/open-llm-leaderboard/results/tree/main/01-ai/Yi-34B): it gives you aggregated scores, per experimental run*
      - *The [details dataset](https://huggingface.co/datasets/open-llm-leaderboard/details_01-ai__Yi-34B/tree/main): it gives you the full details (scores and examples for each task and a given model)*

Why do models appear several times in the leaderboard? 
- *We run evaluations with user-selected precision and model commit. Sometimes, users submit specific models at different commits and at different precisions (for example, in float16 and 4bit to see how quantization affects performance). You should be able to verify this by displaying the `precision` and `model sha` columns in the display. If, however, you see models appearing several times with the same precision and hash commit, this is not normal.*

What is this concept of "flagging"?
- *This mechanism allows users to report models that have unfair performance on the leaderboard. This contains several categories: exceedingly good results on the leaderboard because the model was (maybe accidentally) trained on the evaluation data, models that are copies of other models not attributed properly, etc.*

My model has been flagged improperly, what can I do?
- *Every flagged model has a discussion associated with it - feel free to plead your case there, and we'll see what to do together with the community.*

## How to search for a model

Search for models in the leaderboard by:
1. Name, e.g., *model_name*
2. Multiple names, separated by `;`, e.g., *model_name1;model_name2*
3. License, prefix with `Hub License:...`, e.g., *Hub License: MIT*
4. Combination of name and license, order is irrelevant, e.g., *model_name; Hub License: cc-by-sa-4.0*

## Editing submissions

I upgraded my model and want to re-submit, how can I do that?
- *Please open an issue with the precise name of your model, and we'll remove your model from the leaderboard so you can resubmit. You can also resubmit directly with the new commit hash!* 
I need to rename my model, how can I do that?
- *You can use @Weyaxi 's [super cool tool](https://huggingface.co/spaces/Weyaxi/open-llm-leaderboard-renamer) to request model name changes, then open a discussion where you link to the created pull request, and we'll check them and merge them as needed.*

## Other

Why do you differentiate between pretrained, continuously pretrained, fine-tuned, merges, etc?
- *These different models do not play in the same categories, and therefore need to be separated for fair comparison. Base pretrained models are the most interesting for the community, as they are usually good models to fine-tune later on - any jump in performance from a pretrained model represents a true improvement on the SOTA. 
Fine-tuned and IFT/RLHF/chat models usually have better performance, but the latter might be more sensitive to system prompts, which we do not cover at the moment in the Open LLM Leaderboard. 
Merges and moerges have artificially inflated performance on test sets, which is not always explainable, and does not always apply to real-world situations.*

What should I use the leaderboard for?
- *We recommend using the leaderboard for 3 use cases: 1) getting an idea of the state of open pretrained models, by looking only at the ranks and score of this category; 2) experimenting with different fine-tuning methods, datasets, quantization techniques, etc, and comparing their score in a reproducible setup, and 3) checking the performance of a model of interest to you, wrt to other models of its category.*

Why don't you display closed-source model scores? 
- *This is a leaderboard for Open models, both for philosophical reasons (openness is cool) and for practical reasons: we want to ensure that the results we display are accurate and reproducible, but 1) commercial closed models can change their API thus rendering any scoring at a given time incorrect 2) we re-run everything on our cluster to ensure all models are run on the same setup and you can't do that for these models.*

I have an issue with accessing the leaderboard through the Gradio API
- *Since this is not the recommended way to access the leaderboard, we won't provide support for this, but you can look at tools provided by the community for inspiration!*

I have another problem, help!
- *Please open an issue in the discussion tab, and we'll do our best to help you in a timely manner :)*
