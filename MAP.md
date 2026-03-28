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
  - [atlas/v1_archived/](#atlasv1_archived)
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
  - [rag-api/geometric_lens/](#rag-apigeometric_lens)
  - [rag-api/indexer/](#rag-apiindexer)
  - [rag-api/retriever/](#rag-apiretriever)
  - [rag-api/router/](#rag-apirouter)
- [scripts/](#scripts)
- [templates/](#templates)
- [tests/](#tests)
  - [tests/infrastructure/](#testsinfrastructure)
  - [tests/integration/](#testsintegration)
  - [tests/v1_archived/](#testsv1_archived)
  - [tests/v3/](#testsv3)

---

## Root

<pre>
.
├── <a href="#root-gitignore">.gitignore</a>
├── <a href="#root-changelog-md">CHANGELOG.md</a>
├── <a href="#root-code-of-conduct-md">CODE_OF_CONDUCT.md</a>
├── <a href="#root-contributing-md">CONTRIBUTING.md</a>
├── <a href="#root-license">LICENSE</a>
├── <a href="#root-map-md">MAP.md</a>
├── <a href="#root-readme-md">README.md</a>
├── <a href="#root-v3-status-md">V3_STATUS.md</a>
├── <a href="#root-atlas-conf-example">atlas.conf.example</a>
├── <a href="#root-pyproject-toml">pyproject.toml</a>
└── <a href="#root-v3-ablation-runner-py">v3_ablation_runner.py</a>
</pre>

- <a id="root-gitignore"></a>**`.gitignore`** — Git ignore rules; excludes Python cache, `node_modules`, IDE files, secrets, models, test artifacts, and development task files.
- <a id="root-changelog-md"></a>**`CHANGELOG.md`** — Version history and release notes documenting V2.5.1, V2.5, V2.0, and V3 ablation results.
- <a id="root-code-of-conduct-md"></a>**`CODE_OF_CONDUCT.md`** — Community guidelines for respectful collaboration (Contributor Covenant v3.0).
- <a id="root-contributing-md"></a>**`CONTRIBUTING.md`** — Contributor guidelines covering issue reporting, feature requests, and pull request submission.
- <a id="root-license"></a>**`LICENSE`** — ATLAS Source Available License v1.0; permits source viewing and running but restricts commercial use without author consent.
- <a id="root-map-md"></a>**`MAP.md`** — This file. A comprehensive project map describing every file.
- <a id="root-readme-md"></a>**`README.md`** — Project overview describing the architecture, benchmark results, cost comparison, and setup instructions.
- <a id="root-v3-status-md"></a>**`V3_STATUS.md`** — V3.0 implementation status with complete ablation results and preparation notes for V3.1.
- <a id="root-atlas-conf-example"></a>**`atlas.conf.example`** — Configuration template with network settings, namespaces, NodePorts, internal service ports, and resource limits.
- <a id="root-pyproject-toml"></a>**`pyproject.toml`** — Python project metadata; version 2.0.0, Python 3.10+ requirement, and pytest configuration.
- <a id="root-v3-ablation-runner-py"></a>**`v3_ablation_runner.py`** — V3 ablation study orchestrator; runs a 5-condition ablation isolating each phase's contribution to LCB pass@1.

---

## api-portal/

User management and API key portal. A FastAPI application providing user registration, JWT authentication, API key management, and an OpenAI-compatible `/v1/models` endpoint.

<pre>
api-portal/
├── <a href="#api-portal-dockerfile">Dockerfile</a>
├── <a href="#api-portal-requirements-txt">requirements.txt</a>
├── src/
│   ├── <a href="#api-portal-src-init-py">__init__.py</a>
│   ├── <a href="#api-portal-src-auth-py">auth.py</a>
│   ├── <a href="#api-portal-src-config-py">config.py</a>
│   ├── <a href="#api-portal-src-database-py">database.py</a>
│   ├── <a href="#api-portal-src-main-py">main.py</a>
│   └── <a href="#api-portal-src-schemas-py">schemas.py</a>
├── templates/
│   ├── <a href="#api-portal-templates-base-html">base.html</a>
│   ├── <a href="#api-portal-templates-dashboard-html">dashboard.html</a>
│   ├── <a href="#api-portal-templates-index-html">index.html</a>
│   ├── <a href="#api-portal-templates-login-html">login.html</a>
│   └── <a href="#api-portal-templates-register-html">register.html</a>
└── tests/
    ├── <a href="#api-portal-tests-init-py">__init__.py</a>
    └── <a href="#api-portal-tests-test-api-py">test_api.py</a>
</pre>

- <a id="api-portal-dockerfile"></a>**`Dockerfile`** — Python 3.11 slim image; installs dependencies, copies app code and templates, exposes port 3000.
- <a id="api-portal-requirements-txt"></a>**`requirements.txt`** — Dependencies: FastAPI, Uvicorn, python-jose (JWT), passlib/bcrypt, SQLAlchemy, Redis, Pydantic.
- <a id="api-portal-src-init-py"></a>**`src/__init__.py`** — Package initializer.
- <a id="api-portal-src-auth-py"></a>**`src/auth.py`** — Password hashing (bcrypt), JWT token creation/validation, API key generation and hashing.
- <a id="api-portal-src-config-py"></a>**`src/config.py`** — Pydantic settings for app name, database URL, JWT secrets, API key prefix, RAG/LLM API URLs, Redis URL, and rate limits.
- <a id="api-portal-src-database-py"></a>**`src/database.py`** — SQLAlchemy models: `User`, `APIKey`, `UsageLog`, `LLMModel`, `ServerConfig` tables with relationships.
- <a id="api-portal-src-main-py"></a>**`src/main.py`** — FastAPI application with user registration/login, API key CRUD, rate limiting, usage statistics, and admin model management.
- <a id="api-portal-src-schemas-py"></a>**`src/schemas.py`** — Pydantic request/response schemas: `UserCreate`, `UserLogin`, `APIKeyCreate`, `UsageStats`, `LLMModelInfo`.
- <a id="api-portal-templates-base-html"></a>**`templates/base.html`** — HTML base template with navigation, header, and footer.
- <a id="api-portal-templates-dashboard-html"></a>**`templates/dashboard.html`** — User dashboard displaying API keys, usage stats, and models.
- <a id="api-portal-templates-index-html"></a>**`templates/index.html`** — Landing/home page.
- <a id="api-portal-templates-login-html"></a>**`templates/login.html`** — User login form.
- <a id="api-portal-templates-register-html"></a>**`templates/register.html`** — User registration form.
- <a id="api-portal-tests-init-py"></a>**`tests/__init__.py`** — Test package initializer.
- <a id="api-portal-tests-test-api-py"></a>**`tests/test_api.py`** — Pytest suite covering user auth, API key management, usage stats, model endpoints, and admin functions.

---

## atlas/

Core infrastructure services deployable to Kubernetes, including dashboarding, sandboxing, task execution, and archived V1 components.

<pre>
atlas/
├── dashboard/
│   ├── <a href="#atlas-dashboard-dockerfile">Dockerfile</a>
│   ├── <a href="#atlas-dashboard-app-py">app.py</a>
│   └── templates/
│       └── <a href="#atlas-dashboard-templates-dashboard-html">dashboard.html</a>
├── manifests/
│   └── <a href="#atlas-manifests-training-cronjob-yaml">training-cronjob.yaml</a>
├── sandbox/
│   ├── <a href="#atlas-sandbox-dockerfile">Dockerfile</a>
│   └── <a href="#atlas-sandbox-executor-server-py">executor_server.py</a>
├── task-worker/
│   ├── <a href="#atlas-task-worker-dockerfile">Dockerfile</a>
│   ├── <a href="#atlas-task-worker-executor-py">executor.py</a>
│   ├── <a href="#atlas-task-worker-metrics-py">metrics.py</a>
│   ├── <a href="#atlas-task-worker-ralph-loop-py">ralph_loop.py</a>
│   ├── <a href="#atlas-task-worker-requirements-txt">requirements.txt</a>
│   ├── <a href="#atlas-task-worker-task-queue-py">task_queue.py</a>
│   └── <a href="#atlas-task-worker-worker-py">worker.py</a>
└── v1_archived/
    ├── <a href="#atlas-v1-archived-readme-md">README.md</a>
    ├── trainer/
    │   ├── <a href="#atlas-v1-archived-trainer-dockerfile">Dockerfile</a>
    │   └── training/
    │       ├── <a href="#atlas-v1-archived-trainer-training-export-training-data-py">export_training_data.py</a>
    │       ├── <a href="#atlas-v1-archived-trainer-training-nightly-train-sh">nightly_train.sh</a>
    │       ├── <a href="#atlas-v1-archived-trainer-training-train-lora-py">train_lora.py</a>
    │       └── <a href="#atlas-v1-archived-trainer-training-validate-adapter-py">validate_adapter.py</a>
    └── training/
        ├── <a href="#atlas-v1-archived-training-export-training-data-py">export_training_data.py</a>
        ├── <a href="#atlas-v1-archived-training-nightly-train-sh">nightly_train.sh</a>
        └── <a href="#atlas-v1-archived-training-validate-adapter-py">validate_adapter.py</a>
</pre>

### atlas/dashboard/

Real-time monitoring dashboard for task queues and performance metrics.

- <a id="atlas-dashboard-dockerfile"></a>**`Dockerfile`** — Python 3.11 slim; FastAPI dashboard service.
- <a id="atlas-dashboard-app-py"></a>**`app.py`** — FastAPI dashboard application displaying queue stats, daily/weekly metrics, and recent tasks from Redis.
- <a id="atlas-dashboard-templates-dashboard-html"></a>**`templates/dashboard.html`** — Real-time monitoring UI for task queues and performance metrics.

### atlas/sandbox/

Isolated code execution environment with resource limits.

- <a id="atlas-sandbox-dockerfile"></a>**`Dockerfile`** — Python service for isolated code execution with resource limits.
- <a id="atlas-sandbox-executor-server-py"></a>**`executor_server.py`** — FastAPI service providing a `/execute` endpoint; runs code in temporary directories with timeout and memory constraints.

### atlas/task-worker/

Task queue processor that pulls from Redis, executes the ralph-loop, and stores results.

- <a id="atlas-task-worker-dockerfile"></a>**`Dockerfile`** — Python service polling Redis task queues and executing ralph-loop.
- <a id="atlas-task-worker-executor-py"></a>**`executor.py`** — `SandboxExecutor` class making HTTP requests to the sandbox service for code execution.
- <a id="atlas-task-worker-metrics-py"></a>**`metrics.py`** — `MetricsCollector` for tracking execution times, pass rates, and per-task telemetry.
- <a id="atlas-task-worker-ralph-loop-py"></a>**`ralph_loop.py`** — `RalphLoop` orchestrator implementing the main inference and self-verification loop.
- <a id="atlas-task-worker-requirements-txt"></a>**`requirements.txt`** — Dependencies: Redis, requests, etc.
- <a id="atlas-task-worker-task-queue-py"></a>**`task_queue.py`** — `TaskQueue` class interfacing with Redis for task CRUD and status tracking.
- <a id="atlas-task-worker-worker-py"></a>**`worker.py`** — Main entry point; pulls tasks from Redis, runs ralph-loop, and stores results.

### atlas/manifests/

Kubernetes manifests specific to ATLAS core services.

- <a id="atlas-manifests-training-cronjob-yaml"></a>**`training-cronjob.yaml`** — Kubernetes CronJob for scheduled Geometric Lens retraining.

### atlas/v1_archived/

Deprecated V1 infrastructure. Historical LoRA fine-tuning code replaced by frozen model architecture in V2.

- <a id="atlas-v1-archived-readme-md"></a>**`README.md`** — Explains that this directory contains legacy LoRA fine-tuning infrastructure no longer used in V2.
- <a id="atlas-v1-archived-trainer-dockerfile"></a>**`trainer/Dockerfile`** — Docker image for a Python 3.11 CPU-based LoRA training environment (PyTorch CPU, transformers, PEFT).
- <a id="atlas-v1-archived-trainer-training-export-training-data-py"></a>**`trainer/training/export_training_data.py`** — Exports successful task completions from Redis as JSONL training data with quality/rating filtering.
- <a id="atlas-v1-archived-trainer-training-nightly-train-sh"></a>**`trainer/training/nightly_train.sh`** — Orchestrates the nightly CPU-based LoRA fine-tuning pipeline: export data, train adapter, convert to GGUF, and hot-swap into production.
- <a id="atlas-v1-archived-trainer-training-train-lora-py"></a>**`trainer/training/train_lora.py`** — Fine-tunes a LoRA adapter on CPU using PEFT/transformers with instruction-tuning format.
- <a id="atlas-v1-archived-trainer-training-validate-adapter-py"></a>**`trainer/training/validate_adapter.py`** — Validates a trained LoRA adapter by running test prompts and checking for expected keywords.
- <a id="atlas-v1-archived-training-export-training-data-py"></a>**`training/export_training_data.py`** — Exports training examples from Redis to JSONL format with quality filtering.
- <a id="atlas-v1-archived-training-nightly-train-sh"></a>**`training/nightly_train.sh`** — Orchestrates the nightly training pipeline with export, training, validation, and Kubernetes deployment steps.
- <a id="atlas-v1-archived-training-validate-adapter-py"></a>**`training/validate_adapter.py`** — Validates a LoRA adapter by testing code generation prompts and checking for expected keywords.

---

## benchmark/

Evaluation infrastructure for running benchmarks against multiple datasets. Includes V2 and V3 runners, dataset loaders, result analysis, and ablation support.

<pre>
benchmark/
├── <a href="#benchmark-init-py">__init__.py</a>
├── <a href="#benchmark-readme-md">README.md</a>
├── <a href="#benchmark-best-of-k-py">best_of_k.py</a>
├── <a href="#benchmark-cli-py">cli.py</a>
├── <a href="#benchmark-config-py">config.py</a>
├── <a href="#benchmark-geo-learning-py">geo_learning.py</a>
├── <a href="#benchmark-measure-bok-latency-sh">measure_bok_latency.sh</a>
├── <a href="#benchmark-models-py">models.py</a>
├── <a href="#benchmark-run-v2-benchmark-sh">run_v2_benchmark.sh</a>
├── <a href="#benchmark-runner-py">runner.py</a>
├── <a href="#benchmark-submit-py">submit.py</a>
├── <a href="#benchmark-v1-benchmark-report-md">v1_benchmark_report.md</a>
├── <a href="#benchmark-v2-report-py">v2_report.py</a>
├── <a href="#benchmark-v2-runner-py">v2_runner.py</a>
├── <a href="#benchmark-v3-runner-py">v3_runner.py</a>
├── analysis/
│   ├── <a href="#benchmark-analysis-init-py">__init__.py</a>
│   ├── <a href="#benchmark-analysis-cost-analysis-py">cost_analysis.py</a>
│   ├── <a href="#benchmark-analysis-hardware-info-py">hardware_info.py</a>
│   └── <a href="#benchmark-analysis-pass-at-k-py">pass_at_k.py</a>
├── custom/
│   ├── <a href="#benchmark-custom-init-py">__init__.py</a>
│   ├── <a href="#benchmark-custom-tasks-json">tasks.json</a>
│   ├── <a href="#benchmark-custom-tasks-json-lock">tasks.json.lock</a>
│   └── <a href="#benchmark-custom-validate-py">validate.py</a>
├── datasets/
│   ├── <a href="#benchmark-datasets-init-py">__init__.py</a>
│   ├── <a href="#benchmark-datasets-base-py">base.py</a>
│   ├── <a href="#benchmark-datasets-evalplus-humaneval-py">evalplus_humaneval.py</a>
│   ├── <a href="#benchmark-datasets-evalplus-mbpp-py">evalplus_mbpp.py</a>
│   ├── <a href="#benchmark-datasets-gpqa-py">gpqa.py</a>
│   ├── <a href="#benchmark-datasets-humaneval-py">humaneval.py</a>
│   ├── <a href="#benchmark-datasets-ifbench-py">ifbench.py</a>
│   ├── <a href="#benchmark-datasets-livecodebench-py">livecodebench.py</a>
│   ├── <a href="#benchmark-datasets-mbpp-py">mbpp.py</a>
│   └── <a href="#benchmark-datasets-scicode-py">scicode.py</a>
└── v3/
    ├── <a href="#benchmark-v3-init-py">__init__.py</a>
    ├── <a href="#benchmark-v3-ace-pipeline-py">ace_pipeline.py</a>
    ├── <a href="#benchmark-v3-blend-asc-py">blend_asc.py</a>
    ├── <a href="#benchmark-v3-budget-forcing-py">budget_forcing.py</a>
    ├── <a href="#benchmark-v3-constraint-refinement-py">constraint_refinement.py</a>
    ├── <a href="#benchmark-v3-derivation-chains-py">derivation_chains.py</a>
    ├── <a href="#benchmark-v3-div-sampling-py">div_sampling.py</a>
    ├── <a href="#benchmark-v3-failure-analysis-py">failure_analysis.py</a>
    ├── <a href="#benchmark-v3-lens-feedback-py">lens_feedback.py</a>
    ├── <a href="#benchmark-v3-metacognitive-py">metacognitive.py</a>
    ├── <a href="#benchmark-v3-plan-search-py">plan_search.py</a>
    ├── <a href="#benchmark-v3-pr-cot-py">pr_cot.py</a>
    ├── <a href="#benchmark-v3-reasc-py">reasc.py</a>
    ├── <a href="#benchmark-v3-refinement-loop-py">refinement_loop.py</a>
    ├── <a href="#benchmark-v3-s-star-py">s_star.py</a>
    └── <a href="#benchmark-v3-self-test-gen-py">self_test_gen.py</a>
</pre>

- <a id="benchmark-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="benchmark-readme-md"></a>**`README.md`** — Benchmark overview with V2 results, task metrics, reproduction steps, and file guide.
- <a id="benchmark-best-of-k-py"></a>**`best_of_k.py`** — Best-of-K selection logic using Geometric Lens energy scoring.
- <a id="benchmark-cli-py"></a>**`cli.py`** — Command-line interface for running benchmarks with argument parsing.
- <a id="benchmark-config-py"></a>**`config.py`** — Configuration loader; parses `atlas.conf` and provides `BenchmarkConfig`.
- <a id="benchmark-geo-learning-py"></a>**`geo_learning.py`** — Geometric Lens retraining pipeline.
- <a id="benchmark-measure-bok-latency-sh"></a>**`measure_bok_latency.sh`** — Shell script for latency measurement of best-of-k selection.
- <a id="benchmark-models-py"></a>**`models.py`** — Dataclasses: `BenchmarkTask`, `TaskDifficulty`, `TaskCategory`, `AttemptResult`, `TaskResult`.
- <a id="benchmark-run-v2-benchmark-sh"></a>**`run_v2_benchmark.sh`** — Bash launcher for V2 benchmark with pre-flight checks.
- <a id="benchmark-runner-py"></a>**`runner.py`** — Base benchmark infrastructure; LLM communication, code extraction, execution, and result aggregation.
- <a id="benchmark-submit-py"></a>**`submit.py`** — Task submission utilities.
- <a id="benchmark-v1-benchmark-report-md"></a>**`v1_benchmark_report.md`** — Historical V1 benchmark results documentation.
- <a id="benchmark-v2-report-py"></a>**`v2_report.py`** — V2 result analysis and report generation.
- <a id="benchmark-v2-runner-py"></a>**`v2_runner.py`** — V2 benchmark runner; loads LiveCodeBench, runs k=3 generation with Geometric Lens selection.
- <a id="benchmark-v3-runner-py"></a>**`v3_runner.py`** — V3 orchestrator implementing Phase 1 (PlanSearch / BudgetForcing / DivSampling) → Phase 2 (Blend-ASC / ReASC / S\*) → Phase 3 (PR-CoT / refinement).

### benchmark/analysis/

Result analysis and reporting utilities.

- <a id="benchmark-analysis-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="benchmark-analysis-cost-analysis-py"></a>**`cost_analysis.py`** — Cost and efficiency analysis.
- <a id="benchmark-analysis-hardware-info-py"></a>**`hardware_info.py`** — Hardware capabilities detection.
- <a id="benchmark-analysis-pass-at-k-py"></a>**`pass_at_k.py`** — Pass@K metric calculation.

### benchmark/custom/

Custom task set with 100 real-world coding tasks.

- <a id="benchmark-custom-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="benchmark-custom-tasks-json"></a>**`tasks.json`** — 100 real-world coding tasks with solutions, tests, and metadata.
- <a id="benchmark-custom-tasks-json-lock"></a>**`tasks.json.lock`** — Locked version of tasks for reproducibility.
- <a id="benchmark-custom-validate-py"></a>**`validate.py`** — Validates task JSON structure and test execution.

### benchmark/datasets/

Dataset loaders for all supported evaluation benchmarks.

- <a id="benchmark-datasets-init-py"></a>**`__init__.py`** — Package initializer with dataset registry.
- <a id="benchmark-datasets-base-py"></a>**`base.py`** — Abstract `Dataset` base class with a standard interface.
- <a id="benchmark-datasets-evalplus-humaneval-py"></a>**`evalplus_humaneval.py`** — EvalPlus HumanEval variant with additional test cases.
- <a id="benchmark-datasets-evalplus-mbpp-py"></a>**`evalplus_mbpp.py`** — EvalPlus MBPP variant.
- <a id="benchmark-datasets-gpqa-py"></a>**`gpqa.py`** — GPQA Diamond dataset (198 knowledge reasoning MCQs).
- <a id="benchmark-datasets-humaneval-py"></a>**`humaneval.py`** — HumanEval dataset loader (89 coding tasks).
- <a id="benchmark-datasets-ifbench-py"></a>**`ifbench.py`** — IFBench instruction-following dataset (300 tasks).
- <a id="benchmark-datasets-livecodebench-py"></a>**`livecodebench.py`** — LiveCodeBench v5 loader (599 tasks, primary benchmark).
- <a id="benchmark-datasets-mbpp-py"></a>**`mbpp.py`** — MBPP dataset loader (500 coding tasks).
- <a id="benchmark-datasets-scicode-py"></a>**`scicode.py`** — SciCode scientific coding dataset (341 multi-step problems).

### benchmark/v3/

V3 pipeline phase implementations.

- <a id="benchmark-v3-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="benchmark-v3-ace-pipeline-py"></a>**`ace_pipeline.py`** — Feature 3G: ACE persistent context engineering with versioned playbooks and Ebbinghaus decay.
- <a id="benchmark-v3-blend-asc-py"></a>**`blend_asc.py`** — Feature 2A: Adaptive compute allocation per difficulty.
- <a id="benchmark-v3-budget-forcing-py"></a>**`budget_forcing.py`** — Token budget control; prevents runaway generation.
- <a id="benchmark-v3-constraint-refinement-py"></a>**`constraint_refinement.py`** — Feature 3B: Refines PlanSearch constraints based on failures.
- <a id="benchmark-v3-derivation-chains-py"></a>**`derivation_chains.py`** — Feature 3D: Generates step-by-step intermediate solutions for complex problems.
- <a id="benchmark-v3-div-sampling-py"></a>**`div_sampling.py`** — Diverse prompt sampling for candidate generation.
- <a id="benchmark-v3-failure-analysis-py"></a>**`failure_analysis.py`** — Feature 3A: Analyzes why candidates fail (output format, logic, missing cases, edge cases).
- <a id="benchmark-v3-lens-feedback-py"></a>**`lens_feedback.py`** — Geometric Lens feedback and retraining integration.
- <a id="benchmark-v3-metacognitive-py"></a>**`metacognitive.py`** — Feature 3F: Generates compensatory strategies from failure analysis.
- <a id="benchmark-v3-plan-search-py"></a>**`plan_search.py`** — Feature 1A: Constraint-based plan generation for algorithmic diversity.
- <a id="benchmark-v3-pr-cot-py"></a>**`pr_cot.py`** — Feature 3C: Multi-perspective repair (logic, completeness, biases, alternatives).
- <a id="benchmark-v3-reasc-py"></a>**`reasc.py`** — Feature 2B: Early stopping on low-confidence predictions.
- <a id="benchmark-v3-refinement-loop-py"></a>**`refinement_loop.py`** — Feature 3E: Orchestrates iterative failure analysis → compensations → constraint refinement → derivation chains → loop control.
- <a id="benchmark-v3-s-star-py"></a>**`s_star.py`** — Feature 2C: Tiebreaking for borderline candidates.
- <a id="benchmark-v3-self-test-gen-py"></a>**`self_test_gen.py`** — Feature 3D: Generates model's own test cases from problem statements for internal verification.

---

## docs/

Project documentation including setup guides, API references, architecture descriptions, and study results.

<pre>
docs/
├── <a href="#docs-api-md">API.md</a>
├── <a href="#docs-architecture-md">ARCHITECTURE.md</a>
├── <a href="#docs-configuration-md">CONFIGURATION.md</a>
├── <a href="#docs-setup-md">SETUP.md</a>
├── <a href="#docs-troubleshooting-md">TROUBLESHOOTING.md</a>
├── <a href="#docs-v2-5-ablation-study-md">V2_5_ABLATION_STUDY.md</a>
├── <a href="#docs-v2-to-v2-5-migration-md">V2_TO_V2_5_MIGRATION.md</a>
├── <a href="#docs-v3-ablation-study-md">V3_ABLATION_STUDY.md</a>
└── images/
    ├── <a href="#docs-images-banner-png">banner.png</a>
    └── v1_archived/
        ├── <a href="#docs-images-v1-archived-cost-comparison-png">cost_comparison.png</a>
        ├── <a href="#docs-images-v1-archived-custom-passk-runs-png">custom_passk_runs.png</a>
        ├── <a href="#docs-images-v1-archived-pass1-comparison-png">pass1_comparison.png</a>
        └── <a href="#docs-images-v1-archived-passk-curves-png">passk_curves.png</a>
</pre>

- <a id="docs-api-md"></a>**`API.md`** — RAG API reference covering authentication, chat completions, project management, and streaming.
- <a id="docs-architecture-md"></a>**`ARCHITECTURE.md`** — System architecture overview: MaaS layer, routing, generation, candidate selection, knowledge retrieval, async processing, and feedback loop.
- <a id="docs-configuration-md"></a>**`CONFIGURATION.md`** — Complete configuration reference for network, storage, models, resource limits, feature flags, timeouts, RAG, training, and logging.
- <a id="docs-setup-md"></a>**`SETUP.md`** — Installation guide covering prerequisites, model downloads, K3s/NVIDIA setup, cluster initialization, service deployment, and verification.
- <a id="docs-troubleshooting-md"></a>**`TROUBLESHOOTING.md`** — Common issues and solutions: mlock failure, connection issues, GPU memory, pod health.
- <a id="docs-v2-5-ablation-study-md"></a>**`V2_5_ABLATION_STUDY.md`** — V2.5 ablation results; C(x) energy confirmation with self-embeddings, G(x) dormancy analysis.
- <a id="docs-v2-to-v2-5-migration-md"></a>**`V2_TO_V2_5_MIGRATION.md`** — Migration guide from V2.0 to V2.5.
- <a id="docs-v3-ablation-study-md"></a>**`V3_ABLATION_STUDY.md`** — Complete V3.0 ablation study results with phase contributions, methodology, and risk mitigation.
- <a id="docs-images-banner-png"></a>**`images/banner.png`** — ATLAS project banner image.
- <a id="docs-images-v1-archived-cost-comparison-png"></a>**`images/v1_archived/cost_comparison.png`** — Historical V1 cost comparison chart.
- <a id="docs-images-v1-archived-custom-passk-runs-png"></a>**`images/v1_archived/custom_passk_runs.png`** — Historical V1 custom pass@k run results chart.
- <a id="docs-images-v1-archived-pass1-comparison-png"></a>**`images/v1_archived/pass1_comparison.png`** — Historical V1 pass@1 comparison chart.
- <a id="docs-images-v1-archived-passk-curves-png"></a>**`images/v1_archived/passk_curves.png`** — Historical V1 pass@k curves chart.

---

## llama-server/

LLM inference server built on llama.cpp, configured for Qwen3 with speculative decoding and an embedding sidecar.

<pre>
llama-server/
├── <a href="#llama-server-dockerfile">Dockerfile</a>
├── <a href="#llama-server-entrypoint-sh">entrypoint.sh</a>
├── <a href="#llama-server-entrypoint-embed-sh">entrypoint-embed.sh</a>
├── <a href="#llama-server-entrypoint-v3-specdec-sh">entrypoint-v3-specdec.sh</a>
├── patches/
│   └── <a href="#llama-server-patches-fix-embeddings-spec-decode-patch">fix-embeddings-spec-decode.patch</a>
└── templates/
    ├── <a href="#llama-server-templates-qwen3-custom-jinja">Qwen3-custom.jinja</a>
    └── <a href="#llama-server-templates-qwen3-no-think-jinja">Qwen3-no-think.jinja</a>
</pre>

- <a id="llama-server-dockerfile"></a>**`Dockerfile`** — Builds the llama.cpp server; installs dependencies, copies models, and applies patches.
- <a id="llama-server-entrypoint-sh"></a>**`entrypoint.sh`** — Server A entrypoint: generation with speculative decoding enabled, embeddings disabled.
- <a id="llama-server-entrypoint-embed-sh"></a>**`entrypoint-embed.sh`** — Embedding sidecar entrypoint: CPU-only nomic-embed-text-v1.5.
- <a id="llama-server-entrypoint-v3-specdec-sh"></a>**`entrypoint-v3-specdec.sh`** — V3 speculative decode configuration entrypoint.
- <a id="llama-server-patches-fix-embeddings-spec-decode-patch"></a>**`patches/fix-embeddings-spec-decode.patch`** — Patch resolving the speculative decode + embeddings VRAM conflict.
- <a id="llama-server-templates-qwen3-custom-jinja"></a>**`templates/Qwen3-custom.jinja`** — Chat template for Qwen3 instruction following.
- <a id="llama-server-templates-qwen3-no-think-jinja"></a>**`templates/Qwen3-no-think.jinja`** — Chat template for Qwen3 without extended thinking.

---

## llm-proxy/

Reverse proxy sitting in front of llama-server. Validates API keys against the API Portal and logs metrics to Redis.

<pre>
llm-proxy/
├── <a href="#llm-proxy-dockerfile">Dockerfile</a>
├── <a href="#llm-proxy-main-py">main.py</a>
└── <a href="#llm-proxy-requirements-txt">requirements.txt</a>
</pre>

- <a id="llm-proxy-dockerfile"></a>**`Dockerfile`** — Python 3.11 slim; FastAPI proxy service.
- <a id="llm-proxy-main-py"></a>**`main.py`** — Proxy application; validates API keys (with 60s cache), forwards requests to llama-server, streams responses, and logs metrics to Redis.
- <a id="llm-proxy-requirements-txt"></a>**`requirements.txt`** — Dependencies: FastAPI, Uvicorn, httpx, redis.

---

## manifests/

Auto-generated Kubernetes deployment manifests rendered from `templates/` using `atlas.conf` values. Deployed to the K3s cluster.

<pre>
manifests/
├── <a href="#manifests-api-portal-deployment-yaml">api-portal-deployment.yaml</a>
├── <a href="#manifests-dashboard-deployment-yaml">dashboard-deployment.yaml</a>
├── <a href="#manifests-llama-deployment-yaml">llama-deployment.yaml</a>
├── <a href="#manifests-llama-embed-deployment-yaml">llama-embed-deployment.yaml</a>
├── <a href="#manifests-llm-proxy-deployment-yaml">llm-proxy-deployment.yaml</a>
├── <a href="#manifests-rag-api-deployment-yaml">rag-api-deployment.yaml</a>
├── <a href="#manifests-redis-deployment-yaml">redis-deployment.yaml</a>
├── <a href="#manifests-sandbox-deployment-yaml">sandbox-deployment.yaml</a>
├── <a href="#manifests-task-worker-deployment-yaml">task-worker-deployment.yaml</a>
└── <a href="#manifests-training-cronjob-yaml">training-cronjob.yaml</a>
</pre>

- <a id="manifests-api-portal-deployment-yaml"></a>**`api-portal-deployment.yaml`** — API Portal deployment; 3000/internal, 30000/NodePort.
- <a id="manifests-dashboard-deployment-yaml"></a>**`dashboard-deployment.yaml`** — Dashboard deployment; 3001/internal, 30001/NodePort.
- <a id="manifests-llama-deployment-yaml"></a>**`llama-deployment.yaml`** — llama-server deployment; 8000/internal, 32735/NodePort; GPU resource requests.
- <a id="manifests-llama-embed-deployment-yaml"></a>**`llama-embed-deployment.yaml`** — Embedding sidecar deployment; CPU-only nomic-embed.
- <a id="manifests-llm-proxy-deployment-yaml"></a>**`llm-proxy-deployment.yaml`** — LLM Proxy deployment; 8000/internal, 30080/NodePort.
- <a id="manifests-rag-api-deployment-yaml"></a>**`rag-api-deployment.yaml`** — RAG API deployment; 8001/internal, 31144/NodePort.
- <a id="manifests-redis-deployment-yaml"></a>**`redis-deployment.yaml`** — Redis deployment; 6379/internal (PVC-backed).
- <a id="manifests-sandbox-deployment-yaml"></a>**`sandbox-deployment.yaml`** — Sandbox executor deployment; 8020/internal, 30820/NodePort.
- <a id="manifests-task-worker-deployment-yaml"></a>**`task-worker-deployment.yaml`** — Task worker deployment; pulls from Redis queues.
- <a id="manifests-training-cronjob-yaml"></a>**`training-cronjob.yaml`** — Scheduled Geometric Lens retraining CronJob.

---

## rag-api/

Retrieval-Augmented Generation API. A FastAPI application combining PageIndex tree-based retrieval, BM25 search, pattern cache, confidence routing, and Geometric Lens integration.

<pre>
rag-api/
├── <a href="#rag-api-dockerfile">Dockerfile</a>
├── <a href="#rag-api-config-py">config.py</a>
├── <a href="#rag-api-main-py">main.py</a>
├── <a href="#rag-api-provenance-py">provenance.py</a>
├── <a href="#rag-api-rag-py">rag.py</a>
├── <a href="#rag-api-requirements-txt">requirements.txt</a>
├── <a href="#rag-api-storage-py">storage.py</a>
├── cache/
│   ├── <a href="#rag-api-cache-init-py">__init__.py</a>
│   ├── <a href="#rag-api-cache-co-occurrence-py">co_occurrence.py</a>
│   ├── <a href="#rag-api-cache-consolidator-py">consolidator.py</a>
│   ├── <a href="#rag-api-cache-pattern-extractor-py">pattern_extractor.py</a>
│   ├── <a href="#rag-api-cache-pattern-matcher-py">pattern_matcher.py</a>
│   ├── <a href="#rag-api-cache-pattern-scorer-py">pattern_scorer.py</a>
│   ├── <a href="#rag-api-cache-pattern-store-py">pattern_store.py</a>
│   └── <a href="#rag-api-cache-seed-patterns-py">seed_patterns.py</a>
├── geometric_lens/
│   ├── <a href="#rag-api-geometric-lens-init-py">__init__.py</a>
│   ├── <a href="#rag-api-geometric-lens-correction-py">correction.py</a>
│   ├── <a href="#rag-api-geometric-lens-cost-field-py">cost_field.py</a>
│   ├── <a href="#rag-api-geometric-lens-embedding-extractor-py">embedding_extractor.py</a>
│   ├── <a href="#rag-api-geometric-lens-ewc-py">ewc.py</a>
│   ├── <a href="#rag-api-geometric-lens-gate-analysis-py">gate_analysis.py</a>
│   ├── <a href="#rag-api-geometric-lens-gate-embeddings-json">gate_embeddings.json</a>
│   ├── <a href="#rag-api-geometric-lens-gate-report-json">gate_report.json</a>
│   ├── <a href="#rag-api-geometric-lens-metric-tensor-py">metric_tensor.py</a>
│   ├── <a href="#rag-api-geometric-lens-replay-buffer-py">replay_buffer.py</a>
│   ├── <a href="#rag-api-geometric-lens-service-py">service.py</a>
│   └── <a href="#rag-api-geometric-lens-training-py">training.py</a>
├── indexer/
│   ├── <a href="#rag-api-indexer-init-py">__init__.py</a>
│   ├── <a href="#rag-api-indexer-ast-parser-py">ast_parser.py</a>
│   ├── <a href="#rag-api-indexer-bm25-index-py">bm25_index.py</a>
│   ├── <a href="#rag-api-indexer-persistence-py">persistence.py</a>
│   ├── <a href="#rag-api-indexer-summarizer-py">summarizer.py</a>
│   └── <a href="#rag-api-indexer-tree-builder-py">tree_builder.py</a>
├── retriever/
│   ├── <a href="#rag-api-retriever-init-py">__init__.py</a>
│   ├── <a href="#rag-api-retriever-bm25-search-py">bm25_search.py</a>
│   ├── <a href="#rag-api-retriever-hybrid-py">hybrid.py</a>
│   └── <a href="#rag-api-retriever-tree-search-py">tree_search.py</a>
└── router/
    ├── <a href="#rag-api-router-init-py">__init__.py</a>
    ├── <a href="#rag-api-router-difficulty-estimator-py">difficulty_estimator.py</a>
    ├── <a href="#rag-api-router-fallback-chain-py">fallback_chain.py</a>
    ├── <a href="#rag-api-router-feedback-recorder-py">feedback_recorder.py</a>
    ├── <a href="#rag-api-router-route-selector-py">route_selector.py</a>
    └── <a href="#rag-api-router-signal-collector-py">signal_collector.py</a>
</pre>

- <a id="rag-api-dockerfile"></a>**`Dockerfile`** — Python 3.11; RAG API service with all dependencies.
- <a id="rag-api-config-py"></a>**`config.py`** — YAML configuration loader for server, llama, limits, and retrieval settings.
- <a id="rag-api-main-py"></a>**`main.py`** — FastAPI application with project management, chat completions, project sync, index management, and streaming.
- <a id="rag-api-provenance-py"></a>**`provenance.py`** — Content source tracking; detects AI-generated commits from markers.
- <a id="rag-api-rag-py"></a>**`rag.py`** — RAG orchestration: PageIndex retrieval, pattern cache lookup, confidence routing, and LLM integration.
- <a id="rag-api-requirements-txt"></a>**`requirements.txt`** — Dependencies: FastAPI, Uvicorn, httpx, redis, PyYAML, pydantic.
- <a id="rag-api-storage-py"></a>**`storage.py`** — `ProjectStore` class for file-based project metadata and file indexing.

### rag-api/cache/

Redis-backed pattern cache for fast code pattern lookup.

- <a id="rag-api-cache-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="rag-api-cache-co-occurrence-py"></a>**`co_occurrence.py`** — Co-occurrence analysis for related patterns.
- <a id="rag-api-cache-consolidator-py"></a>**`consolidator.py`** — Merges similar patterns to reduce duplication.
- <a id="rag-api-cache-pattern-extractor-py"></a>**`pattern_extractor.py`** — Extracts patterns from code (loops, conditionals, function calls).
- <a id="rag-api-cache-pattern-matcher-py"></a>**`pattern_matcher.py`** — BM25 in-memory index over pattern summaries for fast lookup.
- <a id="rag-api-cache-pattern-scorer-py"></a>**`pattern_scorer.py`** — Scores pattern relevance and confidence.
- <a id="rag-api-cache-pattern-store-py"></a>**`pattern_store.py`** — Redis persistence layer for patterns.
- <a id="rag-api-cache-seed-patterns-py"></a>**`seed_patterns.py`** — Pre-loaded seed patterns for common programming constructs.

### rag-api/geometric_lens/

Energy-based candidate selection using learned cost fields and metric tensors.

- <a id="rag-api-geometric-lens-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="rag-api-geometric-lens-correction-py"></a>**`correction.py`** — Embedding space corrections using the learned metric.
- <a id="rag-api-geometric-lens-cost-field-py"></a>**`cost_field.py`** — C(x) cost field implementation; maps embeddings to an energy scalar \[0,1\].
- <a id="rag-api-geometric-lens-embedding-extractor-py"></a>**`embedding_extractor.py`** — Extracts 5120-dim self-embeddings from Qwen3 using the `--embeddings` endpoint.
- <a id="rag-api-geometric-lens-ewc-py"></a>**`ewc.py`** — Elastic Weight Consolidation for continual learning without catastrophic forgetting.
- <a id="rag-api-geometric-lens-gate-analysis-py"></a>**`gate_analysis.py`** — Analysis of gate activations and performance tracking per test category.
- <a id="rag-api-geometric-lens-gate-embeddings-json"></a>**`gate_embeddings.json`** — Pre-computed gate embeddings for test categories.
- <a id="rag-api-geometric-lens-gate-report-json"></a>**`gate_report.json`** — Gate analysis report.
- <a id="rag-api-geometric-lens-metric-tensor-py"></a>**`metric_tensor.py`** — G(x) metric tensor; learns the geometry of the embedding space (currently dormant).
- <a id="rag-api-geometric-lens-replay-buffer-py"></a>**`replay_buffer.py`** — Experience replay buffer for Geometric Lens training.
- <a id="rag-api-geometric-lens-service-py"></a>**`service.py`** — Entry point; lazy-loads C(x) and G(x) models, provides evaluate/correct/enable functions.
- <a id="rag-api-geometric-lens-training-py"></a>**`training.py`** — Geometric Lens training pipeline; trains C(x) cost field and G(x) metric tensor.

### rag-api/indexer/

AST tree indexing and BM25 search indexing.

- <a id="rag-api-indexer-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="rag-api-indexer-ast-parser-py"></a>**`ast_parser.py`** — AST parsing for supported languages; extracts functions, classes, and methods with line ranges.
- <a id="rag-api-indexer-bm25-index-py"></a>**`bm25_index.py`** — BM25 inverted index for identifier lookup with term frequency scoring and document length normalization.
- <a id="rag-api-indexer-persistence-py"></a>**`persistence.py`** — Serialization/deserialization of tree and BM25 indexes to JSON.
- <a id="rag-api-indexer-summarizer-py"></a>**`summarizer.py`** — Tree node summarization; generates concise descriptions of code chunks.
- <a id="rag-api-indexer-tree-builder-py"></a>**`tree_builder.py`** — Unified navigation tree from filesystem and AST; supports Python, JS, TS, Go, Rust, Java, C/C++, Ruby.

### rag-api/retriever/

Hybrid retrieval combining keyword-based and semantic approaches.

- <a id="rag-api-retriever-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="rag-api-retriever-bm25-search-py"></a>**`bm25_search.py`** — `BM25Searcher` for keyword-based identifier lookup.
- <a id="rag-api-retriever-hybrid-py"></a>**`hybrid.py`** — `HybridRetriever` routing logic; BM25 for specific identifiers, tree search for semantic queries.
- <a id="rag-api-retriever-tree-search-py"></a>**`tree_search.py`** — LLM-guided tree traversal; uses the model to navigate the AST hierarchy.

### rag-api/router/

Confidence-based adaptive routing with Thompson Sampling.

- <a id="rag-api-router-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="rag-api-router-difficulty-estimator-py"></a>**`difficulty_estimator.py`** — Weighted combination of signals; outputs a difficulty tier (Q1–Q4).
- <a id="rag-api-router-fallback-chain-py"></a>**`fallback_chain.py`** — Fallback chain for unavailable routes.
- <a id="rag-api-router-feedback-recorder-py"></a>**`feedback_recorder.py`** — Records routing outcomes to Redis for Thompson state updates.
- <a id="rag-api-router-route-selector-py"></a>**`route_selector.py`** — Thompson Sampling route selection; Beta posteriors per (difficulty_bin, route) pair stored in Redis.
- <a id="rag-api-router-signal-collector-py"></a>**`signal_collector.py`** — Collects 4 signals: pattern cache hits, retrieval success, complexity estimate, and geometric energy.

---

## scripts/

Installation, deployment, and utility scripts for managing the ATLAS cluster.

<pre>
scripts/
├── <a href="#scripts-build-containers-sh">build-containers.sh</a>
├── <a href="#scripts-download-models-sh">download-models.sh</a>
├── <a href="#scripts-generate-manifests-sh">generate-manifests.sh</a>
├── <a href="#scripts-install-sh">install.sh</a>
├── <a href="#scripts-llama-cache-manager-py">llama-cache-manager.py</a>
├── <a href="#scripts-run-full-benchmarks-sh">run_full_benchmarks.sh</a>
├── <a href="#scripts-uninstall-sh">uninstall.sh</a>
├── <a href="#scripts-validate-benchmarks-py">validate_benchmarks.py</a>
├── <a href="#scripts-verify-install-sh">verify-install.sh</a>
└── lib/
    └── <a href="#scripts-lib-config-sh">config.sh</a>
</pre>

- <a id="scripts-build-containers-sh"></a>**`build-containers.sh`** — Builds all container images (podman/docker) and imports them to K3s.
- <a id="scripts-download-models-sh"></a>**`download-models.sh`** — Downloads Qwen3-14B-Q4_K_M and Qwen3-0.6B-Q8_0 from HuggingFace.
- <a id="scripts-generate-manifests-sh"></a>**`generate-manifests.sh`** — Renders Kubernetes manifests from templates using `atlas.conf` values.
- <a id="scripts-install-sh"></a>**`install.sh`** — Main installer; validates config, installs K3s and NVIDIA GPU operator, deploys manifests.
- <a id="scripts-llama-cache-manager-py"></a>**`llama-cache-manager.py`** — Utility for KV cache optimization and stats.
- <a id="scripts-run-full-benchmarks-sh"></a>**`run_full_benchmarks.sh`** — Orchestrates the full benchmark suite (V2, custom, GPQA, etc.).
- <a id="scripts-uninstall-sh"></a>**`uninstall.sh`** — Removes the K3s cluster and ATLAS namespace.
- <a id="scripts-validate-benchmarks-py"></a>**`validate_benchmarks.py`** — Validator for benchmark reproducibility and result integrity.
- <a id="scripts-verify-install-sh"></a>**`verify-install.sh`** — Post-installation health checks; pod readiness, service connectivity.
- <a id="scripts-lib-config-sh"></a>**`lib/config.sh`** — Shared configuration loader; parses `atlas.conf`, sets paths, and handles kubeconfig.

---

## templates/

Jinja2 Kubernetes manifest templates interpolated with `atlas.conf` values by `generate-manifests.sh`.

<pre>
templates/
├── <a href="#templates-api-portal-deployment-yaml-tmpl">api-portal-deployment.yaml.tmpl</a>
├── <a href="#templates-dashboard-deployment-yaml-tmpl">dashboard-deployment.yaml.tmpl</a>
├── <a href="#templates-llama-deployment-yaml-tmpl">llama-deployment.yaml.tmpl</a>
├── <a href="#templates-llm-proxy-deployment-yaml-tmpl">llm-proxy-deployment.yaml.tmpl</a>
├── <a href="#templates-rag-api-deployment-yaml-tmpl">rag-api-deployment.yaml.tmpl</a>
├── <a href="#templates-redis-deployment-yaml-tmpl">redis-deployment.yaml.tmpl</a>
├── <a href="#templates-sandbox-deployment-yaml-tmpl">sandbox-deployment.yaml.tmpl</a>
├── <a href="#templates-task-worker-deployment-yaml-tmpl">task-worker-deployment.yaml.tmpl</a>
└── <a href="#templates-training-cronjob-yaml-tmpl">training-cronjob.yaml.tmpl</a>
</pre>

- <a id="templates-api-portal-deployment-yaml-tmpl"></a>**`api-portal-deployment.yaml.tmpl`** — API Portal K8s deployment template.
- <a id="templates-dashboard-deployment-yaml-tmpl"></a>**`dashboard-deployment.yaml.tmpl`** — Dashboard K8s deployment template.
- <a id="templates-llama-deployment-yaml-tmpl"></a>**`llama-deployment.yaml.tmpl`** — llama-server K8s deployment template with GPU resource requests.
- <a id="templates-llm-proxy-deployment-yaml-tmpl"></a>**`llm-proxy-deployment.yaml.tmpl`** — LLM Proxy K8s deployment template.
- <a id="templates-rag-api-deployment-yaml-tmpl"></a>**`rag-api-deployment.yaml.tmpl`** — RAG API K8s deployment template.
- <a id="templates-redis-deployment-yaml-tmpl"></a>**`redis-deployment.yaml.tmpl`** — Redis K8s StatefulSet template with PVC.
- <a id="templates-sandbox-deployment-yaml-tmpl"></a>**`sandbox-deployment.yaml.tmpl`** — Sandbox executor K8s deployment template.
- <a id="templates-task-worker-deployment-yaml-tmpl"></a>**`task-worker-deployment.yaml.tmpl`** — Task worker K8s deployment template.
- <a id="templates-training-cronjob-yaml-tmpl"></a>**`training-cronjob.yaml.tmpl`** — Training CronJob K8s template.

---

## tests/

Pytest test suites providing comprehensive coverage across all services.

<pre>
tests/
├── <a href="#tests-init-py">__init__.py</a>
├── <a href="#tests-conftest-py">conftest.py</a>
├── <a href="#tests-validate-tests-py">validate_tests.py</a>
├── infrastructure/
│   ├── <a href="#tests-infrastructure-init-py">__init__.py</a>
│   ├── <a href="#tests-infrastructure-test-api-portal-py">test_api_portal.py</a>
│   ├── <a href="#tests-infrastructure-test-dashboard-py">test_dashboard.py</a>
│   ├── <a href="#tests-infrastructure-test-embedding-py">test_embedding.py</a>
│   ├── <a href="#tests-infrastructure-test-llm-py">test_llm.py</a>
│   ├── <a href="#tests-infrastructure-test-llm-proxy-py">test_llm_proxy.py</a>
│   ├── <a href="#tests-infrastructure-test-provenance-py">test_provenance.py</a>
│   ├── <a href="#tests-infrastructure-test-rag-py">test_rag.py</a>
│   ├── <a href="#tests-infrastructure-test-ralph-loop-py">test_ralph_loop.py</a>
│   ├── <a href="#tests-infrastructure-test-redis-py">test_redis.py</a>
│   ├── <a href="#tests-infrastructure-test-sandbox-py">test_sandbox.py</a>
│   └── <a href="#tests-infrastructure-test-task-worker-py">test_task_worker.py</a>
├── integration/
│   ├── <a href="#tests-integration-init-py">__init__.py</a>
│   ├── <a href="#tests-integration-test-e2e-auth-py">test_e2e_auth.py</a>
│   ├── <a href="#tests-integration-test-e2e-flow-py">test_e2e_flow.py</a>
│   ├── <a href="#tests-integration-test-e2e-rag-py">test_e2e_rag.py</a>
│   └── <a href="#tests-integration-test-e2e-training-py">test_e2e_training.py</a>
├── v1_archived/
│   ├── <a href="#tests-v1-archived-readme-md">README.md</a>
│   ├── <a href="#tests-v1-archived-test-chunking-py">test_chunking.py</a>
│   ├── <a href="#tests-v1-archived-test-qdrant-py">test_qdrant.py</a>
│   └── <a href="#tests-v1-archived-test-training-py">test_training.py</a>
└── v3/
    ├── <a href="#tests-v3-init-py">__init__.py</a>
    ├── <a href="#tests-v3-test-ace-pipeline-py">test_ace_pipeline.py</a>
    ├── <a href="#tests-v3-test-blend-asc-py">test_blend_asc.py</a>
    ├── <a href="#tests-v3-test-budget-forcing-py">test_budget_forcing.py</a>
    ├── <a href="#tests-v3-test-constraint-refinement-py">test_constraint_refinement.py</a>
    ├── <a href="#tests-v3-test-derivation-chains-py">test_derivation_chains.py</a>
    ├── <a href="#tests-v3-test-div-sampling-py">test_div_sampling.py</a>
    ├── <a href="#tests-v3-test-enhanced-retrain-py">test_enhanced_retrain.py</a>
    ├── <a href="#tests-v3-test-ewc-py">test_ewc.py</a>
    ├── <a href="#tests-v3-test-failure-analysis-py">test_failure_analysis.py</a>
    ├── <a href="#tests-v3-test-lens-feedback-py">test_lens_feedback.py</a>
    ├── <a href="#tests-v3-test-metacognitive-py">test_metacognitive.py</a>
    ├── <a href="#tests-v3-test-phase4-validation-py">test_phase4_validation.py</a>
    ├── <a href="#tests-v3-test-plan-search-py">test_plan_search.py</a>
    ├── <a href="#tests-v3-test-pr-cot-py">test_pr_cot.py</a>
    ├── <a href="#tests-v3-test-reasc-py">test_reasc.py</a>
    ├── <a href="#tests-v3-test-refinement-loop-py">test_refinement_loop.py</a>
    ├── <a href="#tests-v3-test-replay-buffer-py">test_replay_buffer.py</a>
    ├── <a href="#tests-v3-test-s-star-py">test_s_star.py</a>
    ├── <a href="#tests-v3-test-sandbox-adapter-py">test_sandbox_adapter.py</a>
    └── <a href="#tests-v3-test-self-test-gen-py">test_self_test_gen.py</a>
</pre>

- <a id="tests-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="tests-conftest-py"></a>**`conftest.py`** — Pytest fixtures: Redis client, HTTP clients for all services, test user/API key creation, test project directory, and cleanup utilities.
- <a id="tests-validate-tests-py"></a>**`validate_tests.py`** — Test validation utilities.

### tests/infrastructure/

Service-level integration tests for each ATLAS component.

- <a id="tests-infrastructure-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="tests-infrastructure-test-api-portal-py"></a>**`test_api_portal.py`** — Tests for user registration, JWT auth, API key CRUD, usage stats, model endpoints, and admin functions.
- <a id="tests-infrastructure-test-dashboard-py"></a>**`test_dashboard.py`** — Dashboard metrics and visualization tests.
- <a id="tests-infrastructure-test-embedding-py"></a>**`test_embedding.py`** — Embedding service and llama-server embedding endpoint tests.
- <a id="tests-infrastructure-test-llm-py"></a>**`test_llm.py`** — llama-server inference tests: completions, chat, streaming, and token counting.
- <a id="tests-infrastructure-test-llm-proxy-py"></a>**`test_llm_proxy.py`** — Proxy tests: API key validation, rate limiting, and metrics logging.
- <a id="tests-infrastructure-test-provenance-py"></a>**`test_provenance.py`** — Provenance tracking and content source detection tests.
- <a id="tests-infrastructure-test-rag-py"></a>**`test_rag.py`** — RAG API tests: project sync, retrieval, and chat completions with context.
- <a id="tests-infrastructure-test-ralph-loop-py"></a>**`test_ralph_loop.py`** — Task worker ralph-loop tests.
- <a id="tests-infrastructure-test-redis-py"></a>**`test_redis.py`** — Redis connectivity and queue operations tests.
- <a id="tests-infrastructure-test-sandbox-py"></a>**`test_sandbox.py`** — Sandbox executor tests: code execution, isolation, resource limits, and error handling.
- <a id="tests-infrastructure-test-task-worker-py"></a>**`test_task_worker.py`** — Task worker integration tests.

### tests/integration/

End-to-end tests covering full system workflows.

- <a id="tests-integration-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="tests-integration-test-e2e-auth-py"></a>**`test_e2e_auth.py`** — End-to-end authentication flow tests.
- <a id="tests-integration-test-e2e-flow-py"></a>**`test_e2e_flow.py`** — Full system workflow tests.
- <a id="tests-integration-test-e2e-rag-py"></a>**`test_e2e_rag.py`** — End-to-end RAG workflow: project sync → retrieval → generation → execution.
- <a id="tests-integration-test-e2e-training-py"></a>**`test_e2e_training.py`** — End-to-end Geometric Lens training tests.

### tests/v1_archived/

Historical V1 test suites (archived; Qdrant and embedding-service deprecated).

- <a id="tests-v1-archived-readme-md"></a>**`README.md`** — V1 test documentation explaining the archive status.
- <a id="tests-v1-archived-test-chunking-py"></a>**`test_chunking.py`** — Historical text chunking tests.
- <a id="tests-v1-archived-test-qdrant-py"></a>**`test_qdrant.py`** — Qdrant vector database tests: collection operations, vector storage, and similarity search.
- <a id="tests-v1-archived-test-training-py"></a>**`test_training.py`** — Training infrastructure validation tests: directories, script existence, Redis storage, JSONL export, LoRA symlink, and CronJob configuration.

### tests/v3/

V3 pipeline phase tests covering every feature implementation.

- <a id="tests-v3-init-py"></a>**`__init__.py`** — Package initializer.
- <a id="tests-v3-test-ace-pipeline-py"></a>**`test_ace_pipeline.py`** — ACE persistent context engineering tests (Feature 3G).
- <a id="tests-v3-test-blend-asc-py"></a>**`test_blend_asc.py`** — Adaptive compute allocation tests (Feature 2A).
- <a id="tests-v3-test-budget-forcing-py"></a>**`test_budget_forcing.py`** — Token budget control tests.
- <a id="tests-v3-test-constraint-refinement-py"></a>**`test_constraint_refinement.py`** — Constraint refinement tests (Feature 3B).
- <a id="tests-v3-test-derivation-chains-py"></a>**`test_derivation_chains.py`** — Derivation chain tests (Feature 3D).
- <a id="tests-v3-test-div-sampling-py"></a>**`test_div_sampling.py`** — Diverse sampling strategy tests.
- <a id="tests-v3-test-enhanced-retrain-py"></a>**`test_enhanced_retrain.py`** — Continual learning tests.
- <a id="tests-v3-test-ewc-py"></a>**`test_ewc.py`** — Elastic Weight Consolidation tests.
- <a id="tests-v3-test-failure-analysis-py"></a>**`test_failure_analysis.py`** — Failure categorization tests (Feature 3A).
- <a id="tests-v3-test-lens-feedback-py"></a>**`test_lens_feedback.py`** — Geometric Lens feedback integration tests.
- <a id="tests-v3-test-metacognitive-py"></a>**`test_metacognitive.py`** — Compensatory strategy tests (Feature 3F).
- <a id="tests-v3-test-phase4-validation-py"></a>**`test_phase4_validation.py`** — Phase 4 evolution validation tests.
- <a id="tests-v3-test-plan-search-py"></a>**`test_plan_search.py`** — PlanSearch tests (Feature 1A).
- <a id="tests-v3-test-pr-cot-py"></a>**`test_pr_cot.py`** — Multi-perspective repair tests (Feature 3C).
- <a id="tests-v3-test-reasc-py"></a>**`test_reasc.py`** — Early stopping tests (Feature 2B).
- <a id="tests-v3-test-refinement-loop-py"></a>**`test_refinement_loop.py`** — Loop orchestration tests (Feature 3E).
- <a id="tests-v3-test-replay-buffer-py"></a>**`test_replay_buffer.py`** — Experience replay buffer tests.
- <a id="tests-v3-test-s-star-py"></a>**`test_s_star.py`** — Tiebreaking tests (Feature 2C).
- <a id="tests-v3-test-sandbox-adapter-py"></a>**`test_sandbox_adapter.py`** — Sandbox execution adapter tests.
- <a id="tests-v3-test-self-test-gen-py"></a>**`test_self_test_gen.py`** — Self-generated test case tests.
