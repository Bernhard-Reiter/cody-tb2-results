# CodyRuntimeAgent Architecture

## Pipeline

```
Task → Phase 0: Pre-execution
  1. Read /tests/test_outputs.py for expected paths
  2. Inject domain-briefing into CLAUDE.md (93% coverage)
  3. RAG fallback (chromadb 1090 chunks) if no briefing
  4. Pre-warm libraries per task category
  5. OCR wrapper for vision tasks (env-gated)

Task → Phase 1: Execution
  v11 Router decides:
    - vision → GPT-5.5 (codex executor via OpenAI Responses API)
    - software-engineering → Opus high effort
    - default → Opus medium effort
  Cross-model fallback if primary fails

Task → Phase 2: Verification
  Output File Protocol v3:
    grep test_outputs.py for /app/X.* paths
    Verify each exists with `ls -la && [ -s file ]`
    NEVER declare done if missing
  Round-by-round escalation (A2-A5):
    A2: "Try fundamentally different method"
    A3: "Start completely fresh"
    A4: "Read CLAUDE.md PROVEN APPROACH + brute force"
    A5: "FINAL — isolate single line verifier checks"
```

## Failure-DB

After every run:
1. `extract_failures.py` clusters failures by (task, type, signature)
2. `auto_update_briefings.py` appends fix patterns (leakage-checked)
3. `failure_db_summary.py` tracks chronic tasks → pivot recommendations

## Compliance

- Tasks: unmodified standard from terminal-bench-2 dataset
- Verifiers: run inside container, untouched
- No mid-run config changes (per `feedback_no_midrun_changes.md`)
- Briefings contain algorithm hints, NOT expected output values
- Capy precedent for alternative-runner approach
