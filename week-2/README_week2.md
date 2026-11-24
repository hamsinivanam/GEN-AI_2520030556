# Week 2 - LLM Client with Multiple Providers

## What is this project?
A small program that lets you choose between two AI providers:
- **Gemini** (cloud, needs an API key)
- **Ollama** (runs on your own computer, no API key)

Both work through the same simple client, so the rest of the code does not care which one you pick.

## What I learned
- How to use a common interface (`LLMProvider`) for different providers
- How to retry when Gemini is busy (errors like 503)
- How to fall back to backup Gemini models
- How to test code with a fake provider

## Files
| File | Purpose |
|------|---------|
| `llm_client.py` | `GeminiProvider`, `OllamaProvider` and `LLMClient` |
| `main.py` | Asks you for a provider and a question, then prints the answer |
| `test_client.py` | A simple unit test using a fake provider |
| `requirements.txt` | Libraries needed |

## Setup
1. Install the libraries:
   ```
   pip install -r requirements.txt
   ```
2. For Gemini, create a `.env` file:
   ```
   GEMINI_API_KEY=your_api_key_here
   ```
3. For Ollama, install it from ollama.com, then download the model:
   ```
   ollama pull llama3.2
   ```
   Keep Ollama running while you use the program.

## How to run
```
python main.py
```
Type `gemini` or `ollama`, then type your question.

## How to run the test
```
python test_client.py
```
or
```
pytest
```

## How the fallback works
Gemini tries the models in order:
1. `gemini-3.8-flash`
2. `gemini-3.5-flash-lite`
3. `gemini-3.7-flash`

If a model is busy, the program waits 3 seconds, retries once, then moves to the next model.

## Common problems
- **"Cannot connect to Ollama"** - Make sure Ollama is running.
- **"Gemini request failed"** - Check your API key, model access and quota.
- **"Question cannot be empty"** - Type something before pressing Enter.
