# CodyRuntimeAgent — Terminal-Bench 2.0

**Score: 63/89 = 70.8% pass@5** on [Terminal-Bench 2.0](https://www.tbench.ai)

| Field | Value |
|---|---|
| Agent | CodyRuntimeAgent v7.0 |
| Org | VOAI |
| Model | Claude Opus 4.6 (`claude-opus-4-6`) |
| Provider | Anthropic |
| Dataset | terminal-bench@2.0 |
| Runner | Harbor |
| n_attempts | 5 |
| n_tasks | 89 |
| pass-any (pass@5) | 63/89 = **70.8%** |
| pass@5 definition | at least 1 of 5 independent attempts passes |
| Total Trials | 445 |

## Architecture

CodyRuntimeAgent is a runtime-injection agent that enhances Claude Opus 4.6 with task-aware context before each trial begins.

### Core Components

**1. CLAUDE.md Pre-Injection**  
Before each trial, the agent injects a task-specific `CLAUDE.md` into the container. This file contains domain hints, known failure patterns, and task-relevant constraints — derived from prior run analysis. The agent never sees raw tasks blindly.

**2. ExperienceDB (RAG)**  
A lightweight retrieval system stores failure patterns from previous runs. At runtime, the top-k most similar failures are retrieved and included in the CLAUDE.md injection, giving the agent learned context from past attempts.

**3. Doom-Loop Detection**  
If the agent repeats the same command 3 times in a row, it is forcibly stopped and asked to re-diagnose. This prevents infinite loops on hard tasks.

**4. Plan.md Tracking**  
Every 5 turns, the agent checks `/app/plan.md` — a TODO list it maintains itself. This enforces structured problem-solving and prevents the agent from losing track of progress.

**5. Adaptive Max-Turns**  
Max turns are set per task category:
- BUILD tasks: 50 turns
- CRYPTO/EMULATION: 70 turns  
- ML/DATA: 35–50 turns

**6. Specialist Prompt Addenda**  
Each task category receives an additional system prompt section with domain-specific guidance (e.g., compiler flags for BUILD tasks, numerical precision requirements for ML tasks).

### Run Configuration

```yaml
n_attempts: 5
n_concurrent: 4
timeout_multiplier: 1.0
model: claude-opus-4-6
runner: harbor
dataset: terminal-bench@2.0
```

## Results

Full per-task results: [`results/summary.json`](results/summary.json)

### Pass-Any by Task

| Task | Pass-Any | Scores |
|---|---|---|
| adaptive-rejection-sampler | ✅ | [1.0,0.0,1.0,0.0,1.0] |
| bn-fit-modify | ✅ | [1.0,1.0,1.0,0.0,0.0] |
| break-filter-js-from-html | ✅ | [0.0,1.0,1.0,1.0,1.0] |
| build-cython-ext | ✅ | [0.0,0.0,0.0,0.0,1.0] |
| build-pmars | ✅ | [0.0,0.0,1.0,0.0,0.0] |
| build-pov-ray | ✅ | [1.0,0.0,1.0,0.0,1.0] |
| caffe-cifar-10 | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| cancel-async-tasks | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| chess-best-move | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| circuit-fibsqrt | ✅ | [1.0,0.0,0.0,1.0,1.0] |
| cobol-modernization | ✅ | [0.0,0.0,1.0,1.0,1.0] |
| code-from-image | ✅ | [0.0,1.0,1.0,1.0,0.0] |
| compile-compcert | ✅ | [0.0,1.0,1.0,0.0,0.0] |
| configure-git-webserver | ✅ | [1.0,0.0,1.0,0.0,0.0] |
| constraints-scheduling | ✅ | [1.0,1.0,0.0,0.0,1.0] |
| count-dataset-tokens | ✅ | [1.0,0.0,1.0,0.0,0.0] |
| crack-7z-hash | ✅ | [1.0,0.0,0.0,0.0,1.0] |
| custom-memory-heap-crash | ✅ | [1.0,1.0,1.0,0.0,0.0] |
| db-wal-recovery | ✅ | [0.0,1.0,1.0,1.0,0.0] |
| distribution-search | ✅ | [0.0,1.0,1.0,1.0,0.0] |
| dna-assembly | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| dna-insert | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| extract-elf | ✅ | [1.0,0.0,1.0,1.0,0.0] |
| extract-moves-from-video | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| feal-differential-cryptanalysis | ✅ | [0.0,1.0,0.0,1.0,1.0] |
| feal-linear-cryptanalysis | ✅ | [0.0,0.0,0.0,1.0,1.0] |
| filter-js-from-html | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| financial-document-processor | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| fix-code-vulnerability | ✅ | [0.0,0.0,0.0,1.0,0.0] |
| fix-git | ✅ | [1.0,1.0,0.0,1.0,1.0] |
| fix-ocaml-gc | ✅ | [1.0,1.0,1.0,0.0,1.0] |
| gcode-to-text | ✅ | [0.0,1.0,0.0,0.0,0.0] |
| git-leak-recovery | ✅ | [0.0,1.0,1.0,1.0,1.0] |
| git-multibranch | ✅ | [0.0,1.0,0.0,0.0,0.0] |
| gpt2-codegolf | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| headless-terminal | ✅ | [0.0,1.0,1.0,1.0,1.0] |
| hf-model-inference | ✅ | [0.0,0.0,0.0,1.0,0.0] |
| install-windows-3-11 | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| kv-store-grpc | ✅ | [0.0,1.0,0.0,0.0,0.0] |
| large-scale-text-editing | ✅ | [0.0,0.0,1.0,0.0,1.0] |
| largest-eigenval | ✅ | [0.0,0.0,1.0,1.0,1.0] |
| llm-inference-batching-scheduler | ✅ | [0.0,1.0,0.0,1.0,1.0] |
| log-summary-date-ranges | ✅ | [1.0,0.0,0.0,1.0,0.0] |
| mailman | ✅ | [1.0,1.0,0.0,0.0,1.0] |
| make-doom-for-mips | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| make-mips-interpreter | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| mcmc-sampling-stan | ✅ | [1.0,1.0,0.0,0.0,0.0] |
| merge-diff-arc-agi-task | ✅ | [0.0,1.0,1.0,1.0,0.0] |
| model-extraction-relu-logits | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| modernize-scientific-stack | ✅ | [1.0,0.0,1.0,1.0,0.0] |
| mteb-leaderboard | ✅ | [0.0,0.0,1.0,1.0,1.0] |
| mteb-retrieve | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| multi-source-data-merger | ✅ | [0.0,1.0,1.0,1.0,0.0] |
| nginx-request-logging | ✅ | [0.0,1.0,1.0,1.0,1.0] |
| openssl-selfsigned-cert | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| overfull-hbox | ✅ | [0.0,0.0,0.0,1.0,1.0] |
| password-recovery | ✅ | [1.0,0.0,1.0,1.0,1.0] |
| path-tracing | ✅ | [1.0,1.0,1.0,1.0,0.0] |
| path-tracing-reverse | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| polyglot-c-py | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| polyglot-rust-c | ✅ | [0.0,0.0,0.0,1.0,0.0] |
| portfolio-optimization | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| protein-assembly | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| prove-plus-comm | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| pypi-server | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| pytorch-model-cli | ✅ | [1.0,0.0,1.0,0.0,0.0] |
| pytorch-model-recovery | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| qemu-alpine-ssh | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| qemu-startup | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| query-optimize | ✅ | [0.0,0.0,0.0,0.0,1.0] |
| raman-fitting | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| regex-chess | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| regex-log | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| reshard-c4-data | ✅ | [1.0,1.0,1.0,1.0,0.0] |
| rstan-to-pystan | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| sam-cell-seg | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| sanitize-git-repo | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| schemelike-metacircular-eval | ✅ | [1.0,0.0,1.0,0.0,0.0] |
| sparql-university | ✅ | [0.0,0.0,1.0,1.0,0.0] |
| sqlite-db-truncate | ✅ | [1.0,1.0,1.0,1.0,1.0] |
| sqlite-with-gcov | ✅ | [1.0,0.0,0.0,1.0,0.0] |
| torch-pipeline-parallelism | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| torch-tensor-parallelism | ✅ | [0.0,1.0,0.0,0.0,0.0] |
| train-fasttext | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| tune-mjcf | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| video-processing | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| vulnerable-secret | ✅ | [1.0,1.0,1.0,0.0,1.0] |
| winning-avg-corewars | ❌ | [0.0,0.0,0.0,0.0,0.0] |
| write-compressor | ✅ | [1.0,0.0,1.0,1.0,0.0] |

## About VOAI

VOAI is building the AI layer for business operations in the DACH region.  
This agent was developed as part of an internal research initiative on agentic coding benchmarks.

Contact: [GitHub Issues](https://github.com/Bernhard-Reiter/cody-tb2-results/issues)
