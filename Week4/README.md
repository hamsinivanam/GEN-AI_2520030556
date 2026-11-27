# Tokenizer Comparison: BPE vs WordPiece vs SentencePiece

A small experiment that compares how three popular subword tokenization algorithms handle the same sentence in **English**, **Telugu**, and **Hindi**.

## Why this matters

Most tokenizers are trained mostly on English text. For Indic scripts, this often means words get split into many small pieces (sometimes down to raw bytes or single characters). More tokens means higher cost, slower inference, and less context available per request. This script makes that gap easy to see.

## Tokenizers compared

| Algorithm | Model | Hugging Face ID |
|-----------|-------|-----------------|
| BPE (Byte-Pair Encoding) | GPT-2 | `gpt2` |
| WordPiece | Multilingual BERT | `bert-base-multilingual-cased` |
| SentencePiece | XLM-RoBERTa | `xlm-roberta-base` |

## Sample texts

| Language | Text |
|----------|------|
| English | Artificial intelligence is changing the world. |
| Telugu | కృత్రిమ మేధస్సు ప్రపంచాన్ని మారుస్తోంది. |
| Hindi | कृत्रिम बुद्धिमत्ता दुनिया को बदल रही है। |

## Project structure

```
.
├── tokenizer_comparison.py   # Main script
├── requirements.txt          # Python dependencies
└── README.md
```

## Setup

Requires Python 3.8+.

```bash
# (optional) create a virtual environment
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# install dependencies
pip install -r requirements.txt
```

## Usage

```bash
python tokenizer_comparison.py
```

The first run downloads the tokenizer files from the Hugging Face Hub, so an internet connection is required. After that they are cached locally.

## Output

For each language and each tokenizer, the script prints:

- **Tokens**: the subword pieces, including special tokens (e.g. `[CLS]`, `<s>`)
- **Token IDs**: the vocabulary IDs for those pieces
- **Token count**: total number of tokens

Example shape of the output:

```
===========================================================================
LANGUAGE: Telugu
INPUT TEXT: కృత్రిమ మేధస్సు ప్రపంచాన్ని మారుస్తోంది.
===========================================================================

BPE - GPT-2
Tokens     : [...]
Token IDs  : [...]
Token count: ...
```

## What to look for

- **GPT-2 (BPE)** uses a byte-level vocabulary, so Telugu and Hindi text typically explodes into many byte-level tokens that look like gibberish.
- **mBERT (WordPiece)** has some multilingual coverage, so splits are shorter, but still fragmented for Telugu. Continuation pieces are marked with `##`.
- **XLM-R (SentencePiece)** was trained on 100 languages and usually gives the most compact, meaningful splits for Indic scripts. Word starts are marked with `▁`.

Exact token counts depend on the tokenizer versions, so run the script to see the real numbers.

## Notes

- `add_special_tokens=True` means special tokens are included in the counts. Set it to `False` in the script if you want counts for the text alone.
- `sentencepiece` is required by the XLM-R tokenizer; `transformers` handles the rest.
- To try more languages or tokenizers, edit the `TEXTS` and `TOKENIZERS` dictionaries at the top of `tokenizer_comparison.py`.

## Possible extensions

- Add a summary table of token counts per language and tokenizer
- Compute tokens-per-word or tokens-per-character ratios
- Add more tokenizers (e.g. Llama, tiktoken `cl100k_base`) or more languages (Tamil, Kannada, Bengali)
- Plot the comparison with matplotlib
