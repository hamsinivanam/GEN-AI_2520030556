# Week 5 - LLM Client with Usage Logging (MongoDB)

## What is this project?
An improved LLM client that, for every call, saves usage details to MongoDB:
- Tokens used (input, output, total)
- Response time (latency)
- Estimated cost
- Success or failure, and number of attempts

It supports **Gemini** (cloud) and **Ollama** (local).

## What I learned
- How to count tokens and estimate cost
- How to log every LLM call to a database
- How to retry on temporary errors with waiting time (2s, 4s...)
- How to switch to backup Gemini models automatically
- How to test with a fake provider and a fake logger

## Files
| File | Purpose |
|------|---------|
| `llm_client.py` | Providers, `LLMResult`, and `LLMClient` with retries and logging |
| `usage_logger.py` | Saves each call to MongoDB |
| `main.py` | Choose a provider, ask a question, get the answer |
| `test_client.py` | Unit test that checks usage is logged correctly |
| `requirements.txt` | Libraries needed |

## Setup
1. Install the libraries:
   ```
   pip install -r requirements.txt
   ```
2. Create a `.env` file:
   ```
   GEMINI_API_KEY=your_api_key_here
   MONGODB_URI=your_mongodb_connection_string
   GEMINI_INPUT_PRICE_PER_MILLION=0.30
   GEMINI_OUTPUT_PRICE_PER_MILLION=2.50
   ```
   Use the real prices from your Gemini pricing page. If you leave them out, the cost shows as 0.
3. For Ollama, install it and run:
   ```
   ollama pull llama3.2
   ```

## How to run
```
python main.py
```
Type `gemini` or `ollama`, then your question.

## How to run the test
```
python test_client.py
```
or
```
pytest
```
The test does not need MongoDB or an API key.

## What gets saved in MongoDB
Database: `llm_monitoring`, collection: `api_usage`

Each record has: provider, model, input tokens, output tokens, total tokens, latency in ms, estimated cost in USD, cost type, status, attempts, and a timestamp. Failed calls also save the error message.

## How retries and fallback work
- **Gemini models** are tried in order: `gemini-3.8-flash`, `gemini-3.7-flash`, `gemini-3.5-flash-lite`.
- **LLMClient** retries up to 3 times on temporary errors (timeout, 429, 500, 502, 503, 504).
- Failed calls are also logged.
- Gemini output is limited to 512 tokens.

## Common problems
- **"MONGODB_URI not found"** - Add it to `.env`.
- **MongoDB connection fails** - Check your connection string and that your IP is allowed in MongoDB Atlas.
- **Ollama connection error** - Make sure Ollama is running.
- **Cost shows 0** - Set the Gemini price values in `.env`. Ollama cost is always 0 here because local running cost is not included.
