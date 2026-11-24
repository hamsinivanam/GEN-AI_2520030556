# Week 4 - Token Economics Budget Calculator

## What is this project?
A calculator that estimates how much an LLM feature will cost and whether it fits your limits, before you build it.

## What it checks
- Tokens used per day
- Cost per day and per month (30 days)
- Peak requests per minute (RPM) and tokens per minute (TPM)
- Whether you stay within your RPM limit, TPM limit, daily budget and monthly budget

A safety margin (default 20%) is added so you do not plan too tightly.

## Files
| File | Purpose |
|------|---------|
| `budget.py` | The calculator and report |
| `test_budget.py` | Unit tests for the calculator |
| `requirements.txt` | Libraries needed (`python-dotenv`) |

## Setup
1. Install the library:
   ```
   pip install -r requirements.txt
   ```
2. Create a `.env` file with your assumptions (example values):
   ```
   REQUESTS_PER_DAY=1000
   AVG_INPUT_TOKENS=1000
   AVG_OUTPUT_TOKENS=500
   PEAK_REQUESTS_PER_MINUTE=10

   INPUT_PRICE_PER_MILLION=1.0
   OUTPUT_PRICE_PER_MILLION=2.0

   RPM_LIMIT=100
   TPM_LIMIT=50000

   MONTHLY_BUDGET_USD=100
   DAILY_BUDGET_USD=5
   SAFETY_MARGIN_PERCENT=20
   ```
   Use the real prices and limits from your provider.

## How to run
```
python budget.py
```

## How to run the tests
```
python test_budget.py
```
or
```
pytest
```

## Reading the report
Each check shows:
- **PASS** - within your limit
- **REVIEW** - too high; change your assumptions, limits or design

If all checks pass, you will see: "WITHIN configured planning limits."

## How the cost is worked out
```
daily cost = (input tokens x input price + output tokens x output price) / 1,000,000
monthly cost = daily cost x 30
```

## Notes
- Negative numbers are not allowed and will raise an error.
- This is a planning estimate, not your real bill.
