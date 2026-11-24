# Week 1 - My First LLM API Call

## What is this project?
A very small Python program that sends one question to Google Gemini and prints the answer.

## What I learned
- How to keep an API key safe in a `.env` file
- How to create a Gemini client
- How to make my first LLM call

## Files
| File | Purpose |
|------|---------|
| `main.py` | Sends a question to Gemini and prints the reply |
| `requirements.txt` | Libraries needed (`google-genai`, `python-dotenv`) |

## Setup
1. Install the libraries:
   ```
   pip install -r requirements.txt
   ```
2. Create a file named `.env` in the same folder and add your key:
   ```
   GEMINI_API_KEY=your_api_key_here
   ```

## How to run
```
python main.py
```

## What happens
1. The program reads your API key from `.env`.
2. It connects to Gemini.
3. It asks: "What is Artificial Intelligence? Explain in simple words."
4. It prints the answer.

## Common problems
- **"Gemini API key not found!"** - Check that `.env` exists and the key name is `GEMINI_API_KEY`.
- **Model error** - Check that the model name in `main.py` is available for your API key.

## Important
Never share your `.env` file or upload it to GitHub. Add `.env` to `.gitignore`.
