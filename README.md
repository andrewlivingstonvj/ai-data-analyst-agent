# AI Data Analyst Agent

An AI agent that answers natural-language questions about a spreadsheet by writing and running its own Pandas code — no pre-built dashboards, no hardcoded queries.

## The problem

Traditional dashboards only answer the questions someone already thought to build a chart for. This agent lets you ask *any* question about your data in plain English and get a real, computed answer — instantly.

## How it works

- Reads a CSV into a Pandas DataFrame and extracts its schema (columns + sample rows)
- Sends the schema + your question to Google's Gemini API with strict instructions to return only executable Pandas code
- Runs that code in a sandboxed environment (only `df` and `pandas` are accessible — nothing else on the system)
- Returns the computed answer and the exact code used to get it

## Tech stack

Python · Pandas · Google Gemini API (`google-genai`) · Google Colab

## Example

**Question:** "Which region had the highest total sales?"

**AI-generated code:**
```python
result = df.groupby('region')['sales'].sum().idxmax()
```

**Answer:** `EMEA`

## Challenge I solved

The model doesn't always follow formatting instructions perfectly — on one question it computed the right value but never assigned it to `result`, silently returning `None`. I added a fallback that detects this and evaluates the last line of generated code directly, so the tool degrades gracefully instead of failing silently. This is the real challenge in building with LLMs: getting *reliable* structured output, not just *any* output.

## How to run it

1. Open `AIAgent.ipynb` in Google Colab
2. Get a free Gemini API key at [aistudio.google.com/apikey](https://aistudio.google.com/apikey) and paste it into the client setup cell
3. Upload your own CSV and run all cells — ask any question about your data

## Connect

Built by Andrew — [LinkedIn](https://www.linkedin.com/in/andrew-livingston-v-j-006987249/)
