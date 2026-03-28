# ATLAS Project Map

A comprehensive map of every file in the ATLAS project, organized by directory.

---

## Table of Contents

- [Root](#root)
- [api-portal/](#api-portal)
- [atlas/](#atlas)
  - [atlas/dashboard/](#atlasdashboard)
  - [atlas/sandbox/](#atlassandbox)
  - [atlas/task-worker/](#atlastask-worker)
  - [atlas/manifests/](#atlasmanifests)
  - [atlas/v1\_archived/](#atlasv1_archived)
- [benchmark/](#benchmark)
  - [benchmark/analysis/](#benchmarkanalysis)
  - [benchmark/custom/](#benchmarkcustom)
  - [benchmark/datasets/](#benchmarkdatasets)
  - [benchmark/v3/](#benchmarkv3)
- [docs/](#docs)
- [llama-server/](#llama-server)
- [llm-proxy/](#llm-proxy)
- [manifests/](#manifests)
- [rag-api/](#rag-api)
  - [rag-api/cache/](#rag-apicache)
  - [rag-api/geometric\_lens/](#rag-apigeometric_lens)
  - [rag-api/indexer/](#rag-apiindexer)
  - [rag-api/retriever/](#rag-apiretriever)
  - [rag-api/router/](#rag-apirouter)
- [scripts/](#scripts)
- [templates/](#templates)
- [tests/](#tests)
  - [tests/infrastructure/](#testsinfrastructure)
  - [tests/integration/](#testsintegration)
  - [tests/v1\_archived/](#testsv1_archived)
  - [tests/v3/](#testsv3)

---

## Root

```
.
├── .gitignore
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── MAP.md
├── README.md
├── V3_STATUS.md
├── atlas.conf.example
├── pyproject.toml
└── v3_ablation_runner.py
```

- **`.gitignore`** — Git ignore rules; excludes Python cache, `node_modules`, IDE files, secrets, models, test artifacts, and development task files.
- **`CHANGELOG.md`** — Version history and release notes documenting V2.5.1, V2.5, V2.0, and V3 ablation results.
- **`CODE_OF_CONDUCT.md`** — Community guidelines for respectful collaboration (Contributor Covenant v3.0).
- **`CONTRIBUTING.md`** — Contributor guidelines covering issue reporting, feature requests, and pull request submission.
- **`LICENSE`** — ATLAS Source Available License v1.0; permits source viewing and running but restricts commercial use without author consent.
- **`MAP.md`** — This file. A comprehensive project map describing every file.
- **`README.md`** — Project overview describing the architecture, benchmark results, cost comparison, and setup instructions.
- **`V3_STATUS.md`** — V3.0 implementation status with complete ablation results and preparation notes for V3.1.
- **`atlas.conf.example`** — Configuration template with network settings, namespaces, NodePorts, internal service ports, and resource limits.
- **`pyproject.toml`** — Python project metadata; version 2.0.0, Python 3.10+ requirement, and pytest configuration.
- **`v3_ablation_runner.py`** — V3 ablation study orchestrator; runs a 5-condition ablation isolating each phase's contribution to LCB pass@1.

---

## api-portal/

User management and API key portal. A FastAPI application providing user registration, JWT authentication, API key management, and an OpenAI-compatible `/v1/models` endpoint.

```
api-portal/
├── Dockerfile
├── requirements.txt
├── src/
│   ├── __init__.py
│   ├── auth.py
│   ├── config.py
│   ├── database.py
│   ├── main.py
│   └── schemas.py
├── templates/
│   ├── base.html
│   ├── dashboard.html
│   ├── index.html
│   ├── login.html
│   └── register.html
└── tests/
    ├── __init__.py
    └── test_api.py
```

- **`Dockerfile`** — Python 3.11 slim image; installs dependencies, copies app code and templates, exposes port 3000.
- **`requirements.txt`** — Dependencies: FastAPI, Uvicorn, python-jose (JWT), passlib/bcrypt, SQLAlchemy, Redis, Pydantic.
- **`src/__init__.py`** — Package initializer.
- **`src/auth.py`** — Password hashing (bcrypt), JWT token creation/validation, API key generation and hashing.
- **`src/config.py`** — Pydantic settings for app name, database URL, JWT secrets, API key prefix, RAG/LLM API URLs, Redis URL, and rate limits.
- **`src/database.py`** — SQLAlchemy models: `User`, `APIKey`, `UsageLog`, `LLMModel`, `ServerConfig` tables with relationships.
- **`src/main.py`** — FastAPI application with user registration/login, API key CRUD, rate limiting, usage statistics, and admin model management.
- **`src/schemas.py`** — Pydantic request/response schemas: `UserCreate`, `UserLogin`, `APIKeyCreate`, `UsageStats`, `LLMModelInfo`.
- **`templates/base.html`** — HTML base template with navigation, header, and footer.
- **`templates/dashboard.html`** — User dashboard displaying API keys, usage stats, and models.
- **`templates/index.html`** — Landing/home page.
- **`templates/login.html`** — User login form.
- **`templates/register.html`** — User registration form.
- **`tests/__init__.py`** — Test package initializer.
- **`tests/test_api.py`** — Pytest suite covering user auth, API key management, usage stats, model endpoints, and admin functions.

---

## atlas/

Core infrastructure services deployable to Kubernetes, including dashboarding, sandboxing, task execution, and archived V1 components.

```
atlas/
├── dashboard/
│   ├── Dockerfile
│   ├── app.py
│   └── templates/
│       └── dashboard.html
├── manifests/
│   └── training-cronjob.yaml
├── sandbox/
│   ├── Dockerfile
│   └── executor_server.py
├── task-worker/
│   ├── Dockerfile
│   ├── executor.py
│   ├── metrics.py
│   ├── ralph_loop.py
│   ├── requirements.txt
│   ├── task_queue.py
│   └── worker.py
└── v1_archived/
    ├── README.md
    ├── trainer/
    │   ├── Dockerfile
    │   └── training/
    │       ├── export_training_data.py
    │       ├── nightly_train.sh
    │       ├── train_lora.py
    │       └── validate_adapter.py
    └── training/
        ├── export_training_data.py
        ├── nightly_train.sh
        └── validate_adapter.py
```

### atlas/dashboard/

Real-time monitoring dashboard for task queues and performance metrics.

- **`Dockerfile`** — Python 3.11 slim; FastAPI dashboard service.
- **`app.py`** — FastAPI dashboard application displaying queue stats, daily/weekly metrics, and recent tasks from Redis.
- **`templates/dashboard.html`** — Real-time monitoring UI for task queues and performance metrics.

### atlas/sandbox/

Isolated code execution environment with resource limits.

- **`Dockerfile`** — Python service for isolated code execution with resource limits.
- **`executor_server.py`** — FastAPI service providing a `/execute` endpoint; runs code in temporary directories with timeout and memory constraints.

### atlas/task-worker/

Task queue processor that pulls from Redis, executes the ralph-loop, and stores results.

- **`Dockerfile`** — Python service polling Redis task queues and executing ralph-loop.
- **`executor.py`** — `SandboxExecutor` class making HTTP requests to the sandbox service for code execution.
- **`metrics.py`** — `MetricsCollector` for tracking execution times, pass rates, and per-task telemetry.
- **`ralph_loop.py`** — `RalphLoop` orchestrator implementing the main inference and self-verification loop.
- **`requirements.txt`** — Dependencies: Redis, requests, etc.
- **`task_queue.py`** — `TaskQueue` class interfacing with Redis for task CRUD and status tracking.
- **`worker.py`** — Main entry point; pulls tasks from Redis, runs ralph-loop, and stores results.

### atlas/manifests/

Kubernetes manifests specific to ATLAS core services.

- **`training-cronjob.yaml`** — Kubernetes CronJob for scheduled Geometric Lens retraining.

### atlas/v1\_archived/

Deprecated V1 infrastructure. Historical LoRA fine-tuning code replaced by frozen model architecture in V2.

- **`README.md`** — Explains that this directory contains legacy LoRA fine-tuning infrastructure no longer used in V2.
- **`trainer/Dockerfile`** — Docker image for a Python 3.11 CPU-based LoRA training environment (PyTorch CPU, transformers, PEFT).
- **`trainer/training/export_training_data.py`** — Exports successful task completions from Redis as JSONL training data with quality/rating filtering.
- **`trainer/training/nightly_train.sh`** — Orchestrates the nightly CPU-based LoRA fine-tuning pipeline: export data, train adapter, convert to GGUF, and hot-swap into production.
- **`trainer/training/train_lora.py`** — Fine-tunes a LoRA adapter on CPU using PEFT/transformers with instruction-tuning format.
- **`trainer/training/validate_adapter.py`** — Validates a trained LoRA adapter by running test prompts and checking for expected keywords.
- **`training/export_training_data.py`** — Exports training examples from Redis to JSONL format with quality filtering.
- **`training/nightly_train.sh`** — Orchestrates the nightly training pipeline with export, training, validation, and Kubernetes deployment steps.
- **`training/validate_adapter.py`** — Validates a LoRA adapter by testing code generation prompts and checking for expected keywords.

---

## benchmark/

Evaluation infrastructure for running benchmarks against multiple datasets. Includes V2 and V3 runners, dataset loaders, result analysis, and ablation support.

```
benchmark/
├── __init__.py
├── README.md
├── best_of_k.py
├── cli.py
├── config.py
├── geo_learning.py
├── measure_bok_latency.sh
├── models.py
├── run_v2_benchmark.sh
├── runner.py
├── submit.py
├── v1_benchmark_report.md
├── v2_report.py
├── v2_runner.py
├── v3_runner.py
├── analysis/
│   ├── __init__.py
│   ├── cost_analysis.py
│   ├── hardware_info.py
│   └── pass_at_k.py
├── custom/
│   ├── __init__.py
│   ├── tasks.json
│   ├── tasks.json.lock
│   └── validate.py
├── datasets/
│   ├── __init__.py
│   ├── base.py
│   ├── evalplus_humaneval.py
│   ├── evalplus_mbpp.py
│   ├── gpqa.py
│   ├── humaneval.py
│   ├── ifbench.py
│   ├── livecodebench.py
│   ├── mbpp.py
│   └── scicode.py
└── v3/
    ├── __init__.py
    ├── ace_pipeline.py
    ├── blend_asc.py
    ├── budget_forcing.py
    ├── constraint_refinement.py
    ├── derivation_chains.py
    ├── div_sampling.py
    ├── failure_analysis.py
    ├── lens_feedback.py
    ├── metacognitive.py
    ├── plan_search.py
    ├── pr_cot.py
    ├── reasc.py
    ├── refinement_loop.py
    ├── s_star.py
    └── self_test_gen.py
```

- **`__init__.py`** — Package initializer.
- **`README.md`** — Benchmark overview with V2 results, task metrics, reproduction steps, and file guide.
- **`best_of_k.py`** — Best-of-K selection logic using Geometric Lens energy scoring.
- **`cli.py`** — Command-line interface for running benchmarks with argument parsing.
- **`config.py`** — Configuration loader; parses `atlas.conf` and provides `BenchmarkConfig`.
- **`geo_learning.py`** — Geometric Lens retraining pipeline.
- **`measure_bok_latency.sh`** — Shell script for latency measurement of best-of-k selection.
- **`models.py`** — Dataclasses: `BenchmarkTask`, `TaskDifficulty`, `TaskCategory`, `AttemptResult`, `TaskResult`.
- **`run_v2_benchmark.sh`** — Bash launcher for V2 benchmark with pre-flight checks.
- **`runner.py`** — Base benchmark infrastructure; LLM communication, code extraction, execution, and result aggregation.
- **`submit.py`** — Task submission utilities.
- **`v1_benchmark_report.md`** — Historical V1 benchmark results documentation.
- **`v2_report.py`** — V2 result analysis and report generation.
- **`v2_runner.py`** — V2 benchmark runner; loads LiveCodeBench, runs k=3 generation with Geometric Lens selection.
- **`v3_runner.py`** — V3 orchestrator implementing Phase 1 (PlanSearch / BudgetForcing / DivSampling) → Phase 2 (Blend-ASC / ReASC / S\*) → Phase 3 (PR-CoT / refinement).

### benchmark/analysis/

Result analysis and reporting utilities.

- **`__init__.py`** — Package initializer.
- **`cost_analysis.py`** — Cost and efficiency analysis.
- **`hardware_info.py`** — Hardware capabilities detection.
- **`pass_at_k.py`** — Pass@K metric calculation.

### benchmark/custom/

Custom task set with 100 real-world coding tasks.

- **`__init__.py`** — Package initializer.
- **`tasks.json`** — 100 real-world coding tasks with solutions, tests, and metadata.
- **`tasks.json.lock`** — Locked version of tasks for reproducibility.
- **`validate.py`** — Validates task JSON structure and test execution.

### benchmark/datasets/

Dataset loaders for all supported evaluation benchmarks.

- **`__init__.py`** — Package initializer with dataset registry.
- **`base.py`** — Abstract `Dataset` base class with a standard interface.
- **`evalplus_humaneval.py`** — EvalPlus HumanEval variant with additional test cases.
- **`evalplus_mbpp.py`** — EvalPlus MBPP variant.
- **`gpqa.py`** — GPQA Diamond dataset (198 knowledge reasoning MCQs).
- **`humaneval.py`** — HumanEval dataset loader (89 coding tasks).
- **`ifbench.py`** — IFBench instruction-following dataset (300 tasks).
- **`livecodebench.py`** — LiveCodeBench v5 loader (599 tasks, primary benchmark).
- **`mbpp.py`** — MBPP dataset loader (500 coding tasks).
- **`scicode.py`** — SciCode scientific coding dataset (341 multi-step problems).

### benchmark/v3/

V3 pipeline phase implementations.

- **`__init__.py`** — Package initializer.
- **`ace_pipeline.py`** — Feature 3G: ACE persistent context engineering with versioned playbooks and Ebbinghaus decay.
- **`blend_asc.py`** — Feature 2A: Adaptive compute allocation per difficulty.
- **`budget_forcing.py`** — Token budget control; prevents runaway generation.
- **`constraint_refinement.py`** — Feature 3B: Refines PlanSearch constraints based on failures.
- **`derivation_chains.py`** — Feature 3D: Generates step-by-step intermediate solutions for complex problems.
- **`div_sampling.py`** — Diverse prompt sampling for candidate generation.
- **`failure_analysis.py`** — Feature 3A: Analyzes why candidates fail (output format, logic, missing cases, edge cases).
- **`lens_feedback.py`** — Geometric Lens feedback and retraining integration.
- **`metacognitive.py`** — Feature 3F: Generates compensatory strategies from failure analysis.
- **`plan_search.py`** — Feature 1A: Constraint-based plan generation for algorithmic diversity.
- **`pr_cot.py`** — Feature 3C: Multi-perspective repair (logic, completeness, biases, alternatives).
- **`reasc.py`** — Feature 2B: Early stopping on low-confidence predictions.
- **`refinement_loop.py`** — Feature 3E: Orchestrates iterative failure analysis → compensations → constraint refinement → derivation chains → loop control.
- **`s_star.py`** — Feature 2C: Tiebreaking for borderline candidates.
- **`self_test_gen.py`** — Feature 3D: Generates model's own test cases from problem statements for internal verification.

---

## docs/

Project documentation including setup guides, API references, architecture descriptions, and study results.

```
docs/
├── API.md
├── ARCHITECTURE.md
├── CONFIGURATION.md
├── SETUP.md
├── TROUBLESHOOTING.md
├── V2_5_ABLATION_STUDY.md
├── V2_TO_V2_5_MIGRATION.md
├── V3_ABLATION_STUDY.md
└── images/
    ├── banner.png
    └── v1_archived/
        ├── cost_comparison.png
        ├── custom_passk_runs.png
        ├── pass1_comparison.png
        └── passk_curves.png
```

- **`API.md`** — RAG API reference covering authentication, chat completions, project management, and streaming.
- **`ARCHITECTURE.md`** — System architecture overview: MaaS layer, routing, generation, candidate selection, knowledge retrieval, async processing, and feedback loop.
- **`CONFIGURATION.md`** — Complete configuration reference for network, storage, models, resource limits, feature flags, timeouts, RAG, training, and logging.
- **`SETUP.md`** — Installation guide covering prerequisites, model downloads, K3s/NVIDIA setup, cluster initialization, service deployment, and verification.
- **`TROUBLESHOOTING.md`** — Common issues and solutions: mlock failure, connection issues, GPU memory, pod health.
- **`V2_5_ABLATION_STUDY.md`** — V2.5 ablation results; C(x) energy confirmation with self-embeddings, G(x) dormancy analysis.
- **`V2_TO_V2_5_MIGRATION.md`** — Migration guide from V2.0 to V2.5.
- **`V3_ABLATION_STUDY.md`** — Complete V3.0 ablation study results with phase contributions, methodology, and risk mitigation.
- **`images/banner.png`** — ATLAS project banner image.
- **`images/v1_archived/cost_comparison.png`** — Historical V1 cost comparison chart.
- **`images/v1_archived/custom_passk_runs.png`** — Historical V1 custom pass@k run results chart.
- **`images/v1_archived/pass1_comparison.png`** — Historical V1 pass@1 comparison chart.
- **`images/v1_archived/passk_curves.png`** — Historical V1 pass@k curves chart.

---

## llama-server/

LLM inference server built on llama.cpp, configured for Qwen3 with speculative decoding and an embedding sidecar.

```
llama-server/
├── Dockerfile
├── entrypoint.sh
├── entrypoint-embed.sh
├── entrypoint-v3-specdec.sh
├── patches/
│   └── fix-embeddings-spec-decode.patch
└── templates/
    ├── Qwen3-custom.jinja
    └── Qwen3-no-think.jinja
```

- **`Dockerfile`** — Builds the llama.cpp server; installs dependencies, copies models, and applies patches.
- **`entrypoint.sh`** — Server A entrypoint: generation with speculative decoding enabled, embeddings disabled.
- **`entrypoint-embed.sh`** — Embedding sidecar entrypoint: CPU-only nomic-embed-text-v1.5.
- **`entrypoint-v3-specdec.sh`** — V3 speculative decode configuration entrypoint.
- **`patches/fix-embeddings-spec-decode.patch`** — Patch resolving the speculative decode + embeddings VRAM conflict.
- **`templates/Qwen3-custom.jinja`** — Chat template for Qwen3 instruction following.
- **`templates/Qwen3-no-think.jinja`** — Chat template for Qwen3 without extended thinking.

---

## llm-proxy/

Reverse proxy sitting in front of llama-server. Validates API keys against the API Portal and logs metrics to Redis.

```
llm-proxy/
├── Dockerfile
├── main.py
└── requirements.txt
```

- **`Dockerfile`** — Python 3.11 slim; FastAPI proxy service.
- **`main.py`** — Proxy application; validates API keys (with 60s cache), forwards requests to llama-server, streams responses, and logs metrics to Redis.
- **`requirements.txt`** — Dependencies: FastAPI, Uvicorn, httpx, redis.

---

## manifests/

Auto-generated Kubernetes deployment manifests rendered from `templates/` using `atlas.conf` values. Deployed to the K3s cluster.

```
manifests/
├── api-portal-deployment.yaml
├── dashboard-deployment.yaml
├── llama-deployment.yaml
├── llama-embed-deployment.yaml
├── llm-proxy-deployment.yaml
├── rag-api-deployment.yaml
├── redis-deployment.yaml
├── sandbox-deployment.yaml
├── task-worker-deployment.yaml
└── training-cronjob.yaml
```

- **`api-portal-deployment.yaml`** — API Portal deployment; 3000/internal, 30000/NodePort.
- **`dashboard-deployment.yaml`** — Dashboard deployment; 3001/internal, 30001/NodePort.
- **`llama-deployment.yaml`** — llama-server deployment; 8000/internal, 32735/NodePort; GPU resource requests.
- **`llama-embed-deployment.yaml`** — Embedding sidecar deployment; CPU-only nomic-embed.
- **`llm-proxy-deployment.yaml`** — LLM Proxy deployment; 8000/internal, 30080/NodePort.
- **`rag-api-deployment.yaml`** — RAG API deployment; 8001/internal, 31144/NodePort.
- **`redis-deployment.yaml`** — Redis deployment; 6379/internal (PVC-backed).
- **`sandbox-deployment.yaml`** — Sandbox executor deployment; 8020/internal, 30820/NodePort.
- **`task-worker-deployment.yaml`** — Task worker deployment; pulls from Redis queues.
- **`training-cronjob.yaml`** — Scheduled Geometric Lens retraining CronJob.

---

## rag-api/

Retrieval-Augmented Generation API. A FastAPI application combining PageIndex tree-based retrieval, BM25 search, pattern cache, confidence routing, and Geometric Lens integration.

```
rag-api/
├── Dockerfile
├── config.py
├── main.py
├── provenance.py
├── rag.py
├── requirements.txt
├── storage.py
├── cache/
│   ├── __init__.py
│   ├── co_occurrence.py
│   ├── consolidator.py
│   ├── pattern_extractor.py
│   ├── pattern_matcher.py
│   ├── pattern_scorer.py
│   ├── pattern_store.py
│   └── seed_patterns.py
├── geometric_lens/
│   ├── __init__.py
│   ├── correction.py
│   ├── cost_field.py
│   ├── embedding_extractor.py
│   ├── ewc.py
│   ├── gate_analysis.py
│   ├── gate_embeddings.json
│   ├── gate_report.json
│   ├── metric_tensor.py
│   ├── replay_buffer.py
│   ├── service.py
│   └── training.py
├── indexer/
│   ├── __init__.py
│   ├── ast_parser.py
│   ├── bm25_index.py
│   ├── persistence.py
│   ├── summarizer.py
│   └── tree_builder.py
├── retriever/
│   ├── __init__.py
│   ├── bm25_search.py
│   ├── hybrid.py
│   └── tree_search.py
└── router/
    ├── __init__.py
    ├── difficulty_estimator.py
    ├── fallback_chain.py
    ├── feedback_recorder.py
    ├── route_selector.py
    └── signal_collector.py
```

- **`Dockerfile`** — Python 3.11; RAG API service with all dependencies.
- **`config.py`** — YAML configuration loader for server, llama, limits, and retrieval settings.
- **`main.py`** — FastAPI application with project management, chat completions, project sync, index management, and streaming.
- **`provenance.py`** — Content source tracking; detects AI-generated commits from markers.
- **`rag.py`** — RAG orchestration: PageIndex retrieval, pattern cache lookup, confidence routing, and LLM integration.
- **`requirements.txt`** — Dependencies: FastAPI, Uvicorn, httpx, redis, PyYAML, pydantic.
- **`storage.py`** — `ProjectStore` class for file-based project metadata and file indexing.

### rag-api/cache/

Redis-backed pattern cache for fast code pattern lookup.

- **`__init__.py`** — Package initializer.
- **`co_occurrence.py`** — Co-occurrence analysis for related patterns.
- **`consolidator.py`** — Merges similar patterns to reduce duplication.
- **`pattern_extractor.py`** — Extracts patterns from code (loops, conditionals, function calls).
- **`pattern_matcher.py`** — BM25 in-memory index over pattern summaries for fast lookup.
- **`pattern_scorer.py`** — Scores pattern relevance and confidence.
- **`pattern_store.py`** — Redis persistence layer for patterns.
- **`seed_patterns.py`** — Pre-loaded seed patterns for common programming constructs.

### rag-api/geometric\_lens/

Energy-based candidate selection using learned cost fields and metric tensors.

- **`__init__.py`** — Package initializer.
- **`correction.py`** — Embedding space corrections using the learned metric.
- **`cost_field.py`** — C(x) cost field implementation; maps embeddings to an energy scalar \[0,1\].
- **`embedding_extractor.py`** — Extracts 5120-dim self-embeddings from Qwen3 using the `--embeddings` endpoint.
- **`ewc.py`** — Elastic Weight Consolidation for continual learning without catastrophic forgetting.
- **`gate_analysis.py`** — Analysis of gate activations and performance tracking per test category.
- **`gate_embeddings.json`** — Pre-computed gate embeddings for test categories.
- **`gate_report.json`** — Gate analysis report.
- **`metric_tensor.py`** — G(x) metric tensor; learns the geometry of the embedding space (currently dormant).
- **`replay_buffer.py`** — Experience replay buffer for Geometric Lens training.
- **`service.py`** — Entry point; lazy-loads C(x) and G(x) models, provides evaluate/correct/enable functions.
- **`training.py`** — Geometric Lens training pipeline; trains C(x) cost field and G(x) metric tensor.

### rag-api/indexer/

AST tree indexing and BM25 search indexing.

- **`__init__.py`** — Package initializer.
- **`ast_parser.py`** — AST parsing for supported languages; extracts functions, classes, and methods with line ranges.
- **`bm25_index.py`** — BM25 inverted index for identifier lookup with term frequency scoring and document length normalization.
- **`persistence.py`** — Serialization/deserialization of tree and BM25 indexes to JSON.
- **`summarizer.py`** — Tree node summarization; generates concise descriptions of code chunks.
- **`tree_builder.py`** — Unified navigation tree from filesystem and AST; supports Python, JS, TS, Go, Rust, Java, C/C++, Ruby.

### rag-api/retriever/

Hybrid retrieval combining keyword-based and semantic approaches.

- **`__init__.py`** — Package initializer.
- **`bm25_search.py`** — `BM25Searcher` for keyword-based identifier lookup.
- **`hybrid.py`** — `HybridRetriever` routing logic; BM25 for specific identifiers, tree search for semantic queries.
- **`tree_search.py`** — LLM-guided tree traversal; uses the model to navigate the AST hierarchy.

### rag-api/router/

Confidence-based adaptive routing with Thompson Sampling.

- **`__init__.py`** — Package initializer.
- **`difficulty_estimator.py`** — Weighted combination of signals; outputs a difficulty tier (Q1–Q4).
- **`fallback_chain.py`** — Fallback chain for unavailable routes.
- **`feedback_recorder.py`** — Records routing outcomes to Redis for Thompson state updates.
- **`route_selector.py`** — Thompson Sampling route selection; Beta posteriors per (difficulty\_bin, route) pair stored in Redis.
- **`signal_collector.py`** — Collects 4 signals: pattern cache hits, retrieval success, complexity estimate, and geometric energy.

---

## scripts/

Installation, deployment, and utility scripts for managing the ATLAS cluster.

```
scripts/
├── build-containers.sh
├── download-models.sh
├── generate-manifests.sh
├── install.sh
├── llama-cache-manager.py
├── run_full_benchmarks.sh
├── uninstall.sh
├── validate_benchmarks.py
├── verify-install.sh
└── lib/
    └── config.sh
```

- **`build-containers.sh`** — Builds all container images (podman/docker) and imports them to K3s.
- **`download-models.sh`** — Downloads Qwen3-14B-Q4\_K\_M and Qwen3-0.6B-Q8\_0 from HuggingFace.
- **`generate-manifests.sh`** — Renders Kubernetes manifests from templates using `atlas.conf` values.
- **`install.sh`** — Main installer; validates config, installs K3s and NVIDIA GPU operator, deploys manifests.
- **`llama-cache-manager.py`** — Utility for KV cache optimization and stats.
- **`run_full_benchmarks.sh`** — Orchestrates the full benchmark suite (V2, custom, GPQA, etc.).
- **`uninstall.sh`** — Removes the K3s cluster and ATLAS namespace.
- **`validate_benchmarks.py`** — Validator for benchmark reproducibility and result integrity.
- **`verify-install.sh`** — Post-installation health checks; pod readiness, service connectivity.
- **`lib/config.sh`** — Shared configuration loader; parses `atlas.conf`, sets paths, and handles kubeconfig.

---

## templates/

Jinja2 Kubernetes manifest templates interpolated with `atlas.conf` values by `generate-manifests.sh`.

```
templates/
├── api-portal-deployment.yaml.tmpl
├── dashboard-deployment.yaml.tmpl
├── llama-deployment.yaml.tmpl
├── llm-proxy-deployment.yaml.tmpl
├── rag-api-deployment.yaml.tmpl
├── redis-deployment.yaml.tmpl
├── sandbox-deployment.yaml.tmpl
├── task-worker-deployment.yaml.tmpl
└── training-cronjob.yaml.tmpl
```

- **`api-portal-deployment.yaml.tmpl`** — API Portal K8s deployment template.
- **`dashboard-deployment.yaml.tmpl`** — Dashboard K8s deployment template.
- **`llama-deployment.yaml.tmpl`** — llama-server K8s deployment template with GPU resource requests.
- **`llm-proxy-deployment.yaml.tmpl`** — LLM Proxy K8s deployment template.
- **`rag-api-deployment.yaml.tmpl`** — RAG API K8s deployment template.
- **`redis-deployment.yaml.tmpl`** — Redis K8s StatefulSet template with PVC.
- **`sandbox-deployment.yaml.tmpl`** — Sandbox executor K8s deployment template.
- **`task-worker-deployment.yaml.tmpl`** — Task worker K8s deployment template.
- **`training-cronjob.yaml.tmpl`** — Training CronJob K8s template.

---

## tests/

Pytest test suites providing comprehensive coverage across all services.

```
tests/
├── __init__.py
├── conftest.py
├── validate_tests.py
├── infrastructure/
│   ├── __init__.py
│   ├── test_api_portal.py
│   ├── test_dashboard.py
│   ├── test_embedding.py
│   ├── test_llm.py
│   ├── test_llm_proxy.py
│   ├── test_provenance.py
│   ├── test_rag.py
│   ├── test_ralph_loop.py
│   ├── test_redis.py
│   ├── test_sandbox.py
│   └── test_task_worker.py
├── integration/
│   ├── __init__.py
│   ├── test_e2e_auth.py
│   ├── test_e2e_flow.py
│   ├── test_e2e_rag.py
│   └── test_e2e_training.py
├── v1_archived/
│   ├── README.md
│   ├── test_chunking.py
│   ├── test_qdrant.py
│   └── test_training.py
└── v3/
    ├── __init__.py
    ├── test_ace_pipeline.py
    ├── test_blend_asc.py
    ├── test_budget_forcing.py
    ├── test_constraint_refinement.py
    ├── test_derivation_chains.py
    ├── test_div_sampling.py
    ├── test_enhanced_retrain.py
    ├── test_ewc.py
    ├── test_failure_analysis.py
    ├── test_lens_feedback.py
    ├── test_metacognitive.py
    ├── test_phase4_validation.py
    ├── test_plan_search.py
    ├── test_pr_cot.py
    ├── test_reasc.py
    ├── test_refinement_loop.py
    ├── test_replay_buffer.py
    ├── test_s_star.py
    ├── test_sandbox_adapter.py
    └── test_self_test_gen.py
```

- **`__init__.py`** — Package initializer.
- **`conftest.py`** — Pytest fixtures: Redis client, HTTP clients for all services, test user/API key creation, test project directory, and cleanup utilities.
- **`validate_tests.py`** — Test validation utilities.

### tests/infrastructure/

Service-level integration tests for each ATLAS component.

- **`__init__.py`** — Package initializer.
- **`test_api_portal.py`** — Tests for user registration, JWT auth, API key CRUD, usage stats, model endpoints, and admin functions.
- **`test_dashboard.py`** — Dashboard metrics and visualization tests.
- **`test_embedding.py`** — Embedding service and llama-server embedding endpoint tests.
- **`test_llm.py`** — llama-server inference tests: completions, chat, streaming, and token counting.
- **`test_llm_proxy.py`** — Proxy tests: API key validation, rate limiting, and metrics logging.
- **`test_provenance.py`** — Provenance tracking and content source detection tests.
- **`test_rag.py`** — RAG API tests: project sync, retrieval, and chat completions with context.
- **`test_ralph_loop.py`** — Task worker ralph-loop tests.
- **`test_redis.py`** — Redis connectivity and queue operations tests.
- **`test_sandbox.py`** — Sandbox executor tests: code execution, isolation, resource limits, and error handling.
- **`test_task_worker.py`** — Task worker integration tests.

### tests/integration/

End-to-end tests covering full system workflows.

- **`__init__.py`** — Package initializer.
- **`test_e2e_auth.py`** — End-to-end authentication flow tests.
- **`test_e2e_flow.py`** — Full system workflow tests.
- **`test_e2e_rag.py`** — End-to-end RAG workflow: project sync → retrieval → generation → execution.
- **`test_e2e_training.py`** — End-to-end Geometric Lens training tests.

### tests/v1\_archived/

Historical V1 test suites (archived; Qdrant and embedding-service deprecated).

- **`README.md`** — V1 test documentation explaining the archive status.
- **`test_chunking.py`** — Historical text chunking tests.
- **`test_qdrant.py`** — Qdrant vector database tests: collection operations, vector storage, and similarity search.
- **`test_training.py`** — Training infrastructure validation tests: directories, script existence, Redis storage, JSONL export, LoRA symlink, and CronJob configuration.

### tests/v3/

V3 pipeline phase tests covering every feature implementation.

- **`__init__.py`** — Package initializer.
- **`test_ace_pipeline.py`** — ACE persistent context engineering tests (Feature 3G).
- **`test_blend_asc.py`** — Adaptive compute allocation tests (Feature 2A).
- **`test_budget_forcing.py`** — Token budget control tests.
- **`test_constraint_refinement.py`** — Constraint refinement tests (Feature 3B).
- **`test_derivation_chains.py`** — Derivation chain tests (Feature 3D).
- **`test_div_sampling.py`** — Diverse sampling strategy tests.
- **`test_enhanced_retrain.py`** — Continual learning tests.
- **`test_ewc.py`** — Elastic Weight Consolidation tests.
- **`test_failure_analysis.py`** — Failure categorization tests (Feature 3A).
- **`test_lens_feedback.py`** — Geometric Lens feedback integration tests.
- **`test_metacognitive.py`** — Compensatory strategy tests (Feature 3F).
- **`test_phase4_validation.py`** — Phase 4 evolution validation tests.
- **`test_plan_search.py`** — PlanSearch tests (Feature 1A).
- **`test_pr_cot.py`** — Multi-perspective repair tests (Feature 3C).
- **`test_reasc.py`** — Early stopping tests (Feature 2B).
- **`test_refinement_loop.py`** — Loop orchestration tests (Feature 3E).
- **`test_replay_buffer.py`** — Experience replay buffer tests.
- **`test_s_star.py`** — Tiebreaking tests (Feature 2C).
- **`test_sandbox_adapter.py`** — Sandbox execution adapter tests.
- **`test_self_test_gen.py`** — Self-generated test case tests.
