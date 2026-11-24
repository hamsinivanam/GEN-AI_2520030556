# Next-Token Probability Inspector (GPT-2)

A small Week 7 experiment that shows how a language model predicts the next token. It loads GPT-2 locally with Hugging Face Transformers, feeds it short prompts, and inspects the raw **logits** and **softmax probabilities** for the next token. The top 5 candidates for each prompt are printed and saved to a CSV file.

## What It Demonstrates

1. **Tokenization**: the prompt is converted into token IDs.
2. **Forward pass**: the model produces logits (one score per vocabulary token) for every position.
3. **Next-token logits**: only the last position's logits are used, since they predict the next token.
4. **Softmax**: logits are converted into a probability distribution that sums to 1.
5. **Top-k**: the 5 most probable candidate tokens are extracted and decoded back into text.

This is the core step of autoregressive generation, shown in isolation.

## Project Structure

```
.
├── main.py                         # Script that runs the experiment
├── requirements.txt                # Python dependencies
├── week7_probability_results.csv   # Output (generated)
└── README.md
```

## Requirements

- Python 3.8+
- `torch`
- `transformers`
- Internet connection on the first run (downloads GPT-2, about 500 MB, then caches it)

No GPU is needed. The script runs on CPU.

## Setup

```bash
# Create and activate a virtual environment (recommended)
python -m venv venv
venv\Scripts\activate          # Windows
source venv/bin/activate       # macOS/Linux

# Install dependencies
pip install -r requirements.txt
```

If installation times out on a slow connection, retry with:

```bash
pip install --default-timeout=1000 --retries 10 -r requirements.txt
```

## Usage

```bash
python main.py
```

For each prompt, the script prints the input token IDs and the top 5 next-token candidates with their probabilities, then writes everything to `week7_probability_results.csv`.

## Configuration

Edit these values in `main.py`:

| Setting      | Default                       | Description                           |
|--------------|-------------------------------|---------------------------------------|
| `MODEL_NAME` | `openai-community/gpt2`       | Hugging Face model to load            |
| `prompts`    | 4 short prompts               | Text prompts to analyze               |
| `TOP_K`      | `5`                           | Number of candidates to show per prompt |

Default prompts:

- `The cat is`
- `Artificial intelligence is`
- `India is a`
- `Machine learning helps`

## Output

Console output looks like this (values will vary by prompt):

```
PROMPT: The cat is
Input Token IDs: [...]

Top 5 Next-Token Candidates:
Rank: 1 | Token: ' ...' | Probability: 0.xxxxxx
...
```

`week7_probability_results.csv` has one row per candidate (4 prompts x 5 candidates = 20 rows):

| Column            | Description                                   |
|-------------------|-----------------------------------------------|
| `Prompt`          | The input prompt                              |
| `Rank`            | Rank of the candidate (1 = most probable)     |
| `Candidate Token` | Decoded text of the candidate token           |
| `Token ID`        | Integer ID of the token in GPT-2's vocabulary |
| `Probability`     | Softmax probability, rounded to 6 decimals    |

Tokens often begin with a leading space (for example `' a'`), because GPT-2's tokenizer encodes the space as part of the token. The script prints tokens with `repr()` so this is visible.

## Notes

- The five probabilities shown are only the top of the distribution, so they sum to less than 1.
- Results are deterministic: the script reads probabilities directly and does no sampling.
- Model weights are cached by Hugging Face (usually in `~/.cache/huggingface`), so later runs start faster.

## Troubleshooting

- **`ModuleNotFoundError: No module named 'torch'`**: the virtual environment isn't active, or the install didn't finish. Reactivate it and rerun `pip install -r requirements.txt`.
- **Read timed out during install or model download**: this is a network issue. Retry, use the longer timeout above, or switch networks.
- **Hugging Face warnings about symlinks or `HF_TOKEN`**: harmless. GPT-2 is public and needs no login.

## Possible Extensions

- Add temperature scaling before the softmax and compare distributions.
- Implement greedy decoding, top-k and top-p sampling loops.
- Plot the probability distribution for each prompt.
- Compare GPT-2 against other models of different sizes.
