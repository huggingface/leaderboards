# How to submit models on the Open LLM Leaderboard

Models added here will be automatically evaluated on the 🤗 cluster. Don't forget to read the FAQ and the About documentation pages for more information!

## First steps before submitting a model

### 1. Ensure Model and Tokenizer Loading:

Make sure you can load your model and tokenizer using AutoClasses:

```jsx
from transformers import AutoConfig, AutoModel, AutoTokenizer
config = AutoConfig.from_pretrained("your model name", revision=revision)
model = AutoModel.from_pretrained("your model name", revision=revision)
tokenizer = AutoTokenizer.from_pretrained("your model name", revision=revision)
```

If this step fails, follow the error messages to debug your model before submitting it. It's likely your model has been improperly uploaded.

**Notes:**

- Ensure your model is public!
- Your model should be in [Safetensors](https://github.com/huggingface/safetensors).
- If your model needs `use_remote_code=True`, we do not support this option yet but are working on adding it. Stay posted!

### 2. Fill Up Your Model Card:

When we add extra information about models to the leaderboard, it will be automatically taken from the model card.

### 3. Select the Correct Precision:

Not all models are converted properly from `float16` to `bfloat16`, and selecting the wrong precision can sometimes cause evaluation errors (as loading a `bf16` model in `fp16` can sometimes generate NaNs, depending on the weight range).

- **Note:** When submitting, git branches and tags will be strictly tied to the specific commit present at the time of submission. This ensures revision consistency.

#### Model Size and Precision Limits:
Our submission system implements a two-tier check to determine if a model can be automatically evaluated:

1. **Absolute Size Limit for High-Precision Models:**
   - Applies to: `float16` and `bfloat16` precisions
   - Limit: 100 billion parameters

2. **Precision-Adjusted Size Limit:**
   - Maximum base size: 140 billion parameters
   - Adjusted by precision factors:
     - `8bit`: 2x (max 280B)
     - `4bit`: 4x (max 560B)
     - `GPTQ`: Varies based on quantization bits

Models exceeding these limits cannot be automatically evaluated. Consider using a lower precision for larger models / open a discussion on Open LLM Leaderboard. If there's enough interest from the community, we'll do a manual evaluation

### 4. Chat Template Toggle:

When submitting a model, you can choose whether to evaluate it using a chat template. The chat template toggle activates automatically for chat models.

### Model Types

- 🟢 **Pretrained Model:** New, base models trained on a given text corpora using masked modeling.
- 🟩 **Continuously Pretrained Model:** New, base models continuously trained on further corpora (which may include IFT/chat data) using masked modeling.
- 🔶 **Fine-Tuned on Domain-Specific Datasets Model:** Pretrained models fine-tuned on more data.
- 💬 **Chat Models (RLHF, DPO, IFT, ...):** Chat-like fine-tunes using IFT (datasets of task instruction), RLHF, DPO (changing the model loss with an added policy), etc.
- 🤝 **Base Merges and Moerges Model:** Merges or MoErges, models which have been merged or fused without additional fine-tuning.
