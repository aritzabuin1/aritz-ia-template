# SOP: LLM Systematic Evaluation (Evals)

**Objective**: Quantify the quality of AI outputs to prevent regressions and hallucinations.

## 1. The "Golden Dataset"
- For every complex prompt, maintain a JSON file in `tests/evals/` containing:
  - `input`: The prompt/data sent.
  - `expected_output`: The ground truth (perfect answer).

## 2. Evaluation Metrics
- **Faithfulness**: Does the answer remain true to the source?
- **Relevance**: Does it answer exactly what was asked?
- **Format Compliance**: Does it follow the JSON/Markdown schema required?

## 3. Automated Eval Skill
- Use `execution/run_evals.py` to compare current agent outputs against the Golden Dataset.
- Report a "Quality Score" (0-100) before any production deployment.

## 4. Performance Benchmarking
- A task is not complete if the Quality Score is below 85% for core business logic.
