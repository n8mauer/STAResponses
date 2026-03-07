# Anthropic — Research Engineer, Agents
## Portfolio Evidence: Technical Mapping

---

Six repositories demonstrate direct coverage of the Research Engineer, Agents competency model. The work spans multi-agent RL systems, LLM finetuning with synthetic data generation, production agent orchestration, model interpretability at 671B-parameter scale, prompt-driven inference pipelines, adversarial game-playing agents with partial observability, and quantitative market simulation — implemented in Python and TypeScript with PyTorch, HuggingFace, scikit-learn, and 17 AWS services.

### Alignment to Role

| JD Requirement | Evidence |
|---|---|
| Complex agentic systems using LLMs | 4 PPO agents with adversarial hardening cycles (residualstreambackdoor), 8-agent dev framework with dependency graph and contract-driven interfaces (studyassistant), game-playing agents with minimax/alpha-beta across 5 variants including 3-player adversarial search (tic-tac-toeandconnectfour) |
| Agent harness design (memory, context compression, communication) | Solver loop with cross-strategy data flow and convergence detection (residualstreambackdoor), RAG pipeline with context-budgeted prompt assembly and 2000-token context windows (studyassistant), SHA-256 hash-keyed response caching as agent memory (cookbook-club), polymorphic agent dispatch across game types with heuristic evaluation (tic-tac-toeandconnectfour) |
| Rigorous quantitative benchmarks | Probe reliability scoring (sample size, train-test gap, layer position, PCA separability), ~70% accuracy on 8B warmup model (residualstreambackdoor), 84/100 output quality with 85-90% business logic accuracy (HiddenSignalTest), portfolio simulation with Sharpe ratio, cumulative return, and daily return metrics benchmarked against SPY (Marketsim) |
| Automated evaluation of models and prompts | Structured output validation with confidence scoring and uncertainty flagging (studyassistant), probe accuracy benchmarks across all transformer layers (residualstreambackdoor), model quality harness with batch evaluation tools (HiddenSignalTest), pytest-parametrized grading framework with 12 test cases and 0.1% tolerance assertions (Marketsim) |
| Data mixes for model training | 4,323-line synthetic dataset generator with 8 weighted business-logic categories, configurable train/val splits, SHA-256 deduplication (HiddenSignalTest) |
| Large-scale RL on language models | 4 PPO agents: PPOSolverAgent (35-dim, 1600 actions), PPOTriggerDiscoveryAgent (128-dim, 3000 actions), PPOAdversaryAgent (64-dim), PPOMonitoringPolicy — with adversarial hardening cycles and early exit at >95% accuracy (residualstreambackdoor) |
| Multi-agent systems | 4 coordinated PPO agents with solver loop orchestration (residualstreambackdoor), 8-agent development framework with dependency ordering and interface contracts (studyassistant), 3-player adversarial game search with partial observability (tic-tac-toeandconnectfour) |
| Finetuning for agent tool use | QLoRA on CodeLlama-7B: 4-bit NF4, LoRA r=64/α=16, SFTTrainer, paged_adamw_32bit, targeting all attention+FFN projections (HiddenSignalTest) |
| Prompting and building products with LLMs | 7 production GPT-4o features with response caching (cookbook-club), structured Claude API prompts with evidence citation (studyassistant), 500+ decoding configurations for memory extraction (residualstreambackdoor) |
| Software engineering and ML experience | 10,400+ lines Python with strict mypy, 205 tests, 6-job CI/CD (residualstreambackdoor), production TypeScript monolith with 124+ endpoints (cookbook-club), automated grading framework with namedtuple test definitions and tolerance-based validation (Marketsim) |
| Model internals and interpretability | Residual stream probing across all layers of 671B DeepSeek V3, per-layer linear probes, CCS unsupervised labeling, PCA diagnostics, attention divergence scoring, weight forensics (residualstreambackdoor) |

---

## Portfolio Project: Multi-Strategy LLM Backdoor Detection Pipeline

*The JD asks: "share with us a project built on LLMs that showcases your skill at getting them to do complex tasks."*

**residualstreambackdoor** is the strongest single-project submission. It combines multi-agent RL, model interpretability, quantitative benchmarking, and production engineering into a single coherent system operating on a 671B-parameter model.

### What it does
Detects hidden backdoor triggers in fine-tuned LLMs by probing both behavioral outputs and internal representations — a three-phase pipeline where agents coordinate across strategies to converge on trigger identification.

### Why it maps to this role

| JD Responsibility | How This Project Demonstrates It |
|---|---|
| *"Ideate, develop, and compare the performance of different agent harnesses"* | 4 PPO agents with distinct state/action spaces (35-dim to 128-dim, 1600-3000 actions), each solving a different sub-problem. The solver loop orchestrates cross-strategy data flow — Phase 2's best probe layer feeds Phase 1's attention analysis, Phase 1's motifs feed Phase 2's trigger scanner — with convergence detection and parameter adaptation. |
| *"Design and implement rigorous quantitative benchmarks"* | Probe reliability scoring combines 4 quantitative signals (sample size, train-test gap, layer position, PCA separability) to prevent false confidence. ~70% accuracy on 8B warmup model with strongest signal in layers 10-18 of 32. Every metric is tracked and compared. |
| *"Assist with automated evaluation of Claude models and prompts"* | The pipeline evaluates model behavior across 500+ decoding configurations (temperature 0.3-2.5, top-k 0-200, top-p 0.5-1.0, repetition penalty), clusters outputs via TF-IDF + DBSCAN, and scores using attention divergence, entropy collapse, and text divergence — a fully automated eval framework. |
| *"Help create and optimize data mixes for model training"* | Adversarial hardening cycles alternate probe training and PPO adversary training — the adversary generates adversarial examples that force probes to improve, creating an iterative training data refinement loop. |
| *"Large-scale RL on language models"* | 4 PPO agents implemented in PyTorch: PPOSolverAgent (parameter tuning), PPOTriggerDiscoveryAgent (token-level trigger construction), PPOAdversaryAgent (probe hardening), PPOMonitoringPolicy (quality gate with accept/reject/escalate). |
| *"Multi-agent systems"* | The solver loop is a multi-agent coordination system — agents share findings across strategies, with convergence detection determining when to stop. The PPOMonitoringPolicy acts as a quality gate over other agents' outputs. |

### Engineering rigor
- 10,400+ lines of Python, strict mypy (`disallow_untyped_defs`, `warn_return_any`)
- 205 tests across 9 test files (50+ PPO tests, 30 CART adapter tests, moto-mocked AWS)
- 6-job parallel CI/CD: ruff, mypy, bandit, pip-audit, pytest, formatting
- Pre-commit hooks, dual-format observability journal (JSONL + Markdown)
- Academic paper (LaTeX with TikZ diagrams)

---

## Supporting Projects

### HiddenSignalTest: LLM Finetuning & Synthetic Data Generation

Maps to: *"Help create and optimize data mixes"* / *"Finetune Claude to maximize its performance"*

| Capability | Evidence |
|---|---|
| **Synthetic data generation** | 4,323-line generator (`fine-tuneLLM.py`) producing instruction-tuning data across 8 weighted business-logic categories (Data Operations 20%, Data Lifecycle 15%, System Architecture 15%, Error Recovery 15%, Communication 15%, Monitoring 10%, Business Rules 5%, Identifiers 5%). 19 generator functions with SHA-256 deduplication and configurable train/validation splits (95/5). |
| **QLoRA finetuning** | CodeLlama-7B with 4-bit NF4 quantization, double quantization, LoRA r=64/α=16/dropout=0.1 targeting all attention and FFN projections (q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj). SFTTrainer with paged_adamw_32bit, BF16 mixed precision, gradient checkpointing. ~12GB GPU memory usage. |
| **Pipeline orchestration** | AWS Step Functions state machine: GenerateDataset → TrainModel → DeployModel. One-click retraining and redeployment — a self-service ML platform pattern. |
| **Quantitative evaluation** | 84/100 average output quality, 85-90% business logic extraction accuracy. Dataset sizing analysis confirmed 10K examples as sweet spot for ~1,000-2,000 unique patterns. |
| **Batch processing & context management** | `batch_converter.py`, `cli_converter.py`, `standalone_converter.py` with MD5-based response caching and intelligent document chunking with context overlap for API token limits. |

**Key files:** `fine-tuneLLM.py` (dataset generator), `lambda/app.py` (inference), `template.yaml` (SAM/CloudFormation), `terraform/` (IaC)

---

### studyassistant: Agent Orchestration & RAG Pipeline

Maps to: *"Agent harnesses (memory, context compression, communication architectures)"* / *"Prompting and model orchestration for a production application"*

| Capability | Evidence |
|---|---|
| **Agent framework** | 8-agent development model (`docs/CLAUDE_CODE_AGENTS.md`) — each agent defined by system prompt, I/O contracts, acceptance criteria, and dependencies. Dependency graph enables parallel execution: Agents 1, 2, 7 run in parallel; Agent 3 depends on 7; Agent 4 depends on 3; Agents 5-6 depend on all processing agents; Agent 8 integrates. |
| **Context management** | Semantic chunking (300-800 tokens, 15% overlap), 2000-token context budget for LLM prompts, cross-encoder re-ranking for context quality, token budget management across retrieval + generation stages. |
| **Agent communication** | 9 typed WebSocket events (`WSEventTypes`: `capture_started`, `preprocessing_complete`, `ocr_complete`, `parsing_complete`, `retrieval_complete`, `answer_ready`, `error`, `document_indexed`, `settings_updated`) — real-time agent-to-frontend communication. |
| **Prompt orchestration** | Structured prompt pipeline with question stem emphasis, source-attributed context injection, explicit output format instructions (ANSWER, JUSTIFICATION, EVIDENCE, CONFIDENCE). Configurable generation settings (model, max_tokens, temperature, timeout) with runtime model switching. |
| **Structured output validation** | GeneratedAnswer dataclass captures: answer, generation time, tokens used, model version, confidence level (HIGH/MEDIUM/LOW), and explicit `uncertain` boolean. Output validation ensures selected answer is always a valid option. |
| **Contract-first design** | Shared Python dataclasses + mirrored TypeScript interfaces — `CaptureResult` → `ParsedMCQ` → `RetrievalResult` → `GeneratedAnswer` flow across pipeline stages with type safety at every boundary. |

**Key files:** `src/api/` (FastAPI routes), `src/rag/` (6-component RAG module), `src/generation/` (prompts, LLM client), `docs/CLAUDE_CODE_AGENTS.md` (agent framework), `docs/SHARED_MODELS.md` (contracts)

---

### cookbook-club: Production LLM Integration at Scale

Maps to: *"Build the prompting and model orchestration for a production application backed by a language model"*

| Capability | Evidence |
|---|---|
| **Production LLM integration** | GPT-4o integrated at 7 distinct touchpoints: cookbook cover OCR, recipe photo OCR, allergen-safe ingredient substitution, multi-recipe shopping list aggregation, AI cookbook suggestions based on rating history, host-preference-weighted recommendations, conversational AI. Live at `cookbookclub.vip`. |
| **Response caching as agent memory** | SHA-256 hash-keyed response caching in PostgreSQL `ai_cache` table with configurable TTLs (7-30 days) — functions as deterministic agent memory, eliminating redundant inference calls. |
| **Prompt engineering at scale** | Different prompt strategies per feature: structured OCR prompts for vision tasks, constraint-aware prompts for allergen substitution (must respect dietary restrictions), aggregation prompts for shopping lists (merge quantities, categorize by aisle). |
| **Resilient orchestration** | Fallback chains for external services: email falls back from Gmail API to SES; OpenAI client uses lazy initialization via JavaScript Proxy to avoid crashes when API key is missing. |
| **Production engineering** | 124+ REST endpoints, dual auth (OAuth sessions + JWT Bearer), Helmet.js CSP, presigned S3 uploads, React 18 + Express 5 + PostgreSQL 16 monolith, 22+ table schema, comprehensive Vitest test suite. |

**Key files:** `server/routes.ts` (124+ endpoints), `server/openai.ts` (GPT-4o integration), `shared/schema.ts` (Zod schemas), `server/storage.ts` (IStorage interface)

---

## JD Requirement → Repository Coverage Matrix

| JD Requirement | residualstreambackdoor | HiddenSignalTest | studyassistant | cookbook-club | tic-tac-toeandconnectfour | Marketsim |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **Complex agentic systems** | 4 PPO agents, solver loop | Step Functions orchestration | 8-agent framework, 10-stage pipeline | 7 AI touchpoints | Minimax + alpha-beta agents, 5 game variants | — |
| **Agent harness design** | Cross-strategy data flow, convergence detection | Pipeline orchestration | Context budgeting, WebSocket communication | Response caching, fallback chains | Polymorphic dispatch, heuristic evaluation | — |
| **Quantitative benchmarks** | Probe reliability scoring, ~70% accuracy | 84/100 quality, 85-90% accuracy | Latency budgets (<4s E2E) | — | GameStats outcome tracking | Sharpe ratio, cumulative return, benchmarked vs SPY |
| **Automated evaluation** | 500+ decoding configs, TF-IDF clustering | Batch evaluation tools | Confidence scoring, uncertainty flags | — | — | pytest-parametrized, 12 test cases, 0.1% tolerance |
| **Data mixes for training** | Adversarial training data generation | 8-category synthetic data generator | — | — | — | — |
| **Large-scale RL** | 4 PPO agents (PyTorch) | — | — | — | — | — |
| **Multi-agent systems** | Solver loop coordination | — | 8-agent dependency graph | — | 3-player adversarial search, partial observability | — |
| **Finetuning** | — | QLoRA on CodeLlama-7B | — | — | — | — |
| **Prompt engineering** | 500+ decoding configs | Instruction tuning templates | Structured prompt pipeline | 7 prompt strategies | — | — |
| **Production LLM products** | — | Serverless inference platform | RAG-powered assistant | Live platform (cookbookclub.vip) | — | — |
| **Model internals** | Residual stream probing, CCS, PCA, attention, weight forensics | — | — | — | — | — |
| **Software engineering** | 10,400+ lines, 205 tests, strict mypy, CI/CD | Terraform + SAM IaC, Lambda | FastAPI async, typed contracts | 124+ endpoints, dual auth, Vitest | Polymorphic agent framework, PRD-driven | Automated grading, namedtuple test defs |

---

## Representative Projects Alignment

The JD lists 6 representative projects. Here is how the portfolio maps:

| JD Representative Project | Portfolio Match |
|---|---|
| *"Design and build a novel agent harness that outperforms existing agents on coding or knowledge work benchmarks"* | residualstreambackdoor's solver loop — a multi-strategy agent harness with 4 PPO agents, cross-strategy data flow, convergence detection, and adversarial hardening that iteratively improves agent performance; tic-tac-toeandconnectfour's polymorphic agent harness dispatching minimax vs alpha-beta search by game complexity |
| *"Design and build agent affordances that unlock new capabilities for internal use and deployed products"* | studyassistant's 10-stage pipeline with WebSocket event bus — each pipeline stage is an agent affordance (capture, OCR, retrieval, generation) composed via typed contracts; cookbook-club's 7 production AI features as deployed agent capabilities |
| *"Design and build a novel eval that measures how many agents interact in groups to solve problems"* | residualstreambackdoor's probe reliability scoring — quantitatively measures how 4 PPO agents and 3 detection strategies interact via the solver loop, with convergence detection determining when the group has solved the problem; tic-tac-toeandconnectfour's 3-player adversarial search with GameStats tracking win/loss/timing across agent configurations |
| *"Build a scaled model evaluation framework driven by model-based evaluation techniques"* | residualstreambackdoor's automated eval over 500+ decoding configurations with TF-IDF + DBSCAN clustering, attention divergence scoring, and per-layer probe accuracy tracking across all transformer layers; Marketsim's pytest-parametrized grading framework with 12 test cases, namedtuple definitions, and tolerance-based quantitative assertions |
| *"Build the prompting and model orchestration for a production application backed by a language model"* | cookbook-club (7 GPT-4o features, live at cookbookclub.vip) and studyassistant (Claude API with structured prompts, confidence scoring, and evidence citation in a 10-stage pipeline) |
| *"Finetune Claude to maximize its performance using a particular set of agent tools or harness"* | HiddenSignalTest's QLoRA finetuning of CodeLlama-7B with 4,323-line synthetic dataset generator across 8 weighted categories, achieving 84/100 output quality at 1000x cost reduction |

---

## Technical Stack Summary

| Layer | Technology |
|-------|-----------|
| **Languages** | Python 3.11+ (strict mypy), TypeScript (full-stack), HCL (Terraform) |
| **ML/AI** | PyTorch, scikit-learn, HuggingFace Transformers, PEFT, TRL, bitsandbytes |
| **RL / Search** | PPO (4 specialized agents), adversarial hardening cycles, minimax, alpha-beta pruning with heuristic evaluation |
| **Interpretability** | Linear probes, CCS, PCA diagnostics, attention analysis, weight forensics |
| **LLM APIs** | Claude API (Anthropic), GPT-4o (OpenAI), CodeLlama-7B (Meta) |
| **RAG** | ChromaDB, sentence-transformers, cross-encoder re-ranking |
| **Agent Communication** | WebSocket (9 event types), SNS pub-sub, SSM approval gates |
| **Frontend** | React 18, Vite, Tailwind CSS, React Native/Expo |
| **Backend** | FastAPI (async), Express 5, Flask, Lambda |
| **AWS** | SageMaker, Step Functions, Lambda, API Gateway, EC2 (GPU), S3, SNS, SSM, CloudFormation, CloudWatch, IAM, and 6 more (17 total) |
| **Testing** | pytest (205 tests + 12 parametrized market sim tests), Vitest, moto (AWS mocks), strict mypy, bandit, pip-audit |
| **CI/CD** | GitHub Actions (6-job parallel pipeline), pre-commit hooks |

---

## Quantified Impact

| Metric | Value | Repo |
|---|---|---|
| PPO agents implemented | 4 (solver, trigger discovery, adversary, monitor) | residualstreambackdoor |
| Model scale handled | 671B parameters (DeepSeek V3) | residualstreambackdoor |
| Probe accuracy (warmup) | ~70% on 8B model, strongest signal layers 10-18/32 | residualstreambackdoor |
| Decoding configurations tested | 500+ (temp, top-k, top-p, repetition penalty) | residualstreambackdoor |
| Test coverage | 205 tests, strict mypy across 24 source files | residualstreambackdoor |
| Finetuning output quality | 84/100 average | HiddenSignalTest |
| Business logic accuracy | 85-90% | HiddenSignalTest |
| Synthetic data generator | 4,323 lines, 8 weighted categories, 19 generators | HiddenSignalTest |
| Cost reduction | 1000x ($1/100 files vs $100-200/hr manual) | HiddenSignalTest |
| E2E pipeline latency | <4 seconds across 10 stages | studyassistant |
| Production AI features | 7 GPT-4o touchpoints, live users | cookbook-club |
| API endpoints | 124+ REST endpoints | cookbook-club |
| AWS services | 17 unique services across repos | all |
| Game variants supported | 5 (TTT, Connect 4 Basic/Extended/Multiplayer/Hidden) | tic-tac-toeandconnectfour |
| Search algorithms | Minimax (full tree) + alpha-beta (depth-limited) | tic-tac-toeandconnectfour |
| Partial observability | Hidden Multiplayer with opponent token masking | tic-tac-toeandconnectfour |
| Portfolio metrics | Sharpe ratio, cumulative return, avg/std daily return | Marketsim |
| Automated test scenarios | 12 parametrized cases across 3 cost model groups | Marketsim |
| Evaluation tolerance | 0.1% across all quantitative assertions | Marketsim |
