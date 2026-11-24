# Week 3 - Tokenizer Comparison

## What is this project?
A program that shows how different tokenizers split the same sentence into tokens. It tests English, Telugu and Hindi.

## Why does this matter?
LLMs charge and measure by tokens, not words. Some languages need many more tokens for the same meaning, which can mean higher cost and less room in the context window.

## Tokenizers compared
| Type | Model |
|------|-------|
| BPE | GPT-2 |
| WordPiece | Multilingual BERT |
| SentencePiece | XLM-R |

## Files
| File | Purpose |
|------|---------|
| `tokenizer_comparison.py` | Loads the tokenizers and prints tokens, token IDs and token count |
| `requirements.txt` | Libraries needed (`transformers`, `sentencepiece`) |

## Setup
```
pip install -r requirements.txt
```
The first run downloads the tokenizers, so you need internet.

## How to run
```
python tokenizer_comparison.py
```

## What you will see
For each language and each tokenizer:
- **Tokens** - the pieces the text was split into
- **Token IDs** - the number for each piece
- **Token count** - how many tokens in total

## What to look for
- English usually needs the fewest tokens.
- Telugu and Hindi often need many more, especially with GPT-2.
- Multilingual tokenizers (mBERT, XLM-R) usually handle Telugu and Hindi better.

## Change the text
Edit the `TEXTS` dictionary in `tokenizer_comparison.py` to try your own sentences or languages.
