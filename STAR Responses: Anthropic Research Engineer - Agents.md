STAR Responses: Anthropic Research Engineer - Agents

Mapped to All Repository Evidence (residualstreambackdoor, HiddenSignalTest, studyassistant, cookbook-club)

---
STAR 1: Multi-Agent Reinforcement Learning System Design

JD Requirement: "Have experience developing complex agentic systems
using LLMs" / "Large-scale RL on language models" / "Multi-agent
systems"

Situation: The Jane Street Dormant Model Challenge required
identifying a hidden backdoor trigger in a 671-billion parameter
LLM (DeepSeek V3). A single detection strategy would be
insufficient — the search space was too large and the signal too
weak for any one approach. The problem demanded multiple
specialized agents coordinating across complementary strategies,
each contributing different capabilities to a shared goal.

Task: Design and implement a multi-agent reinforcement learning
system where specialized PPO agents collaborate to detect hidden
backdoor triggers by combining behavioral analysis (model outputs)
with mechanistic analysis (internal representations) — with each
agent operating in a different part of the search space.

Action:
- Implemented 4 PPO reinforcement learning agents in PyTorch, each
  with distinct state spaces, action spaces, and reward functions:
  PPOSolverAgent (35-dim state, 1600 actions for decoding parameter
  tuning), PPOTriggerDiscoveryAgent (128-dim state, 3000 actions
  for token-level trigger construction), PPOAdversaryAgent (64-dim
  state for adversarial probe hardening), and PPOMonitoringPolicy
  (quality gate with accept/reject/escalate actions).
- Designed an adversarial hardening cycle that alternates between
  probe training and PPO adversary training — the adversary
  generates inputs that fool current probes, forcing the probes to
  improve. The cycle exits early at >95% accuracy to prevent
  overfitting.
- Built comprehensive fallback chains: PPO agents fall back to
  heuristic actions when policy confidence is low, CARTProbe falls
  back to LogisticRegression below 15 samples per class,
  meta-learner falls back to linear blend, CCS falls back to
  divergence-based labeling.
- Implemented the PPOMonitoringPolicy as a quality gate over other
  agents — it observes agent outputs and decides whether to accept,
  reject, or escalate to human review, functioning as an autonomous
  evaluation agent within the system.
- Wrote 50+ PPO-specific tests covering all 4 agent types, action
  selection, reward computation, and policy update correctness.

Result: A multi-agent RL system where 4 specialized agents
coordinate via a shared solver loop, each contributing distinct
capabilities (parameter search, token construction, adversarial
challenge, quality monitoring) to a problem no single agent could
solve alone. The adversarial hardening cycle prevents agents from
converging on superficial correlations — directly applicable to
the kind of agent robustness challenges the Anthropic Agents team
works on. The architecture generalizes: swap the reward function
and action space, and the same multi-agent harness applies to any
complex agentic task requiring coordinated search.

---
STAR 2: Agent Harness Design & Cross-Strategy Orchestration

JD Requirement: "Ideate, develop, and compare the performance of
different agent harnesses (eg memory, context compression,
communication architectures for agents)"

Situation: The backdoor detection pipeline had three fundamentally
different detection strategies — behavioral output analysis,
residual stream probing, and iterative solver orchestration — each
producing different types of evidence. The challenge was designing
an agent harness that could route information between strategies,
maintain state across iterations, and detect convergence without
manual intervention.

Task: Build an agent orchestration harness that manages
cross-strategy data flow, persists state across iterations,
handles graceful degradation when individual strategies fail, and
automatically detects when the agents have converged on a solution.

Action:
- Designed a 3-phase pipeline harness: Phase 1 (Trigger Haystack)
  explores model behavior via 500+ decoding configurations with
  varied temperature (0.3-2.5), top-k (0-200), top-p (0.5-1.0),
  and repetition penalty — clustered via TF-IDF + DBSCAN to
  discover recurring trigger motifs.
- Phase 2 (Residual Stream Probing) extracts hidden-state
  activations from every transformer layer, trains per-layer linear
  probes (L2 logistic regression, L1 sparse probes, custom CART
  ensemble probes), and applies CCS unsupervised labeling and PCA
  separability diagnostics.
- Phase 3 (Solver Loop) implements the cross-strategy communication
  architecture: Phase 2's best probe layer feeds Phase 1's
  attention analysis, Phase 1's motifs feed Phase 2's trigger
  scanner — with convergence detection and parameter adaptation.
  The solver iterates until strategies agree or a max iteration
  count is reached.
- Built an 8-phase idempotent state machine (init, validated,
  provisioned, launched, bootstrapped, executing, collected,
  cleaned_up) with JSON-persisted state and at_least() guard
  checks — every phase is safely retryable without side effects.
- Implemented a dual-format observability journal (JSONL for
  machine parsing, Markdown for human review) that logs every agent
  decision, strategy result, and state transition — enabling
  post-hoc analysis of agent behavior.
- Designed graceful degradation: notification failures never block
  orchestration, CCS falls back to divergence-based labeling when
  insufficient samples, probes fall back across model types.

Result: An agent harness that manages complex multi-strategy
coordination with persistent state, cross-strategy data flow, and
automatic convergence detection. The idempotent state machine
ensures safe recovery from any failure mode. The dual-journal
observability system provides the kind of agent behavior
traceability that's essential for understanding why agents succeed
or fail — directly relevant to Anthropic's work on making agentic
systems reliable over longer time horizon tasks.

---
STAR 3: Quantitative Benchmarks & Automated Model Evaluation

JD Requirement: "Design and implement rigorous quantitative
benchmarks for large scale agentic tasks" / "Assist with automated
evaluation of Claude models and prompts across the training and
product lifecycle"

Situation: The backdoor detection pipeline generated thousands of
data points across probe accuracies, decoding configurations, and
agent iterations — but raw accuracy numbers were misleading. Early
experiments showed inflated accuracies that didn't reproduce,
revealing the need for a principled evaluation framework that
accounts for multiple confounding factors.

Task: Design a quantitative evaluation framework that produces
reliable, comparable benchmarks across detection strategies — one
that catches false confidence and enables apples-to-apples
comparison between agent runs.

Action:
- Designed a probe reliability scoring system that combines 4
  independent signals: sample size (penalizes small samples),
  train-test accuracy gap (catches overfitting), layer position
  (weights middle layers where signal is strongest), and PCA
  separability (silhouette scores on activation space) — producing
  a single composite reliability score per probe.
- Built an automated evaluation pipeline over 500+ decoding
  configurations, scoring each using attention divergence (how much
  attention patterns shift with suspected triggers), entropy
  collapse (detecting abnormally confident outputs), and text
  divergence (measuring output distribution shift).
- Implemented per-layer probe accuracy tracking across all 32
  transformer layers of the 8B warmup model, identifying strongest
  signal in layers 10-18 — a benchmark that generalizes to any
  layer-wise model analysis.
- Created benchmark comparisons between custom CART ensemble probes
  and sklearn RandomForestClassifier baselines, with sigmoid
  calibration for fair probability comparison.
- Built a TF-IDF + DBSCAN clustering pipeline for automated motif
  discovery — clustering model outputs to identify recurring
  patterns without human labeling.
- Wrote 205 tests across 9 test files: 50+ PPO agent tests, 30
  CART adapter tests covering all fallback paths, strategy-level
  tests with synthetic data, and moto-mocked AWS integration tests.

Result: A rigorous evaluation framework that prevented false
confidence in early experiments — the reliability scoring caught
probes with inflated accuracies before they wasted compute on the
full 671B model. The ~70% accuracy on the 8B warmup model with
clear layer-by-layer signal mapping provides a reproducible
benchmark baseline. This evaluation methodology — composite
scoring, multi-signal validation, controlled baselines — maps
directly to the kind of rigorous benchmarking Anthropic needs for
measuring agent performance across tasks and model versions.

---
STAR 4: LLM Finetuning & Training Data Optimization

JD Requirement: "Help create and optimize data mixes for model
training that maximize Claude's performance or ease of use on
agentic tasks" / "Finetune Claude to maximize its performance
using a particular set of agent tools or harness"

Situation: Enterprise mainframe modernization required translating
natural language business logic descriptions into production Python
code. General-purpose LLMs produced generic code that missed
domain-specific patterns — CRUD operations with transactions, TTL
expiration policies, active-active replication, and error
recovery. The model needed domain-specific finetuning with
carefully composed training data.

Task: Build a synthetic dataset generation pipeline and finetune
CodeLlama-7B to maximize output quality on domain-specific code
generation — optimizing the data mix composition, training
hyperparameters, and evaluation criteria.

Action:
- Built a 4,323-line dataset generator (fine-tuneLLM.py) producing
  instruction-tuning data across 8 weighted business-logic
  categories: Data Operations (20%), Data Lifecycle (15%), System
  Architecture (15%), Error Recovery (15%), Communication (15%),
  Monitoring (10%), Business Rules (5%), Identifiers (5%). The
  weights reflect the frequency distribution of patterns in real
  mainframe codebases.
- Implemented 19 generator functions with SHA-256 deduplication to
  prevent training on duplicate examples, and configurable
  train/validation splits (95/5) for reliable evaluation.
- Ran QLoRA finetuning: 4-bit NF4 quantization via bitsandbytes,
  double quantization, LoRA r=64/alpha=16/dropout=0.1 targeting all
  attention and FFN projections (q_proj, k_proj, v_proj, o_proj,
  gate_proj, up_proj, down_proj). SFTTrainer with paged_adamw_32bit
  optimizer, BF16 mixed precision, gradient checkpointing —
  achieving ~12GB GPU memory usage.
- Conducted a dataset sizing analysis to find the optimal data mix:
  10K examples emerged as the sweet spot for ~1,000-2,000 unique
  patterns. Below 5K, the model underfit domain-specific patterns;
  above 20K, returns diminished rapidly.
- Built batch evaluation tools (batch_converter.py, cli_converter.py,
  standalone_converter.py) with MD5-based response caching and
  intelligent document chunking with context overlap for API token
  limit management.
- Deployed the finetuned model via two paths: SageMaker Serverless
  Inference (scales to zero, pay-per-invocation) and HuggingFace
  Inference Endpoints — enabling A/B comparison of hosting
  strategies.

Result: 84/100 average output quality and 85-90% business logic
extraction accuracy at under $1 per 100 files — a 1000x cost
improvement over manual rewriting. The dataset sizing analysis
confirmed that data mix composition matters more than raw volume:
the category weights directly impacted which patterns the model
learned to generate accurately. This experience with synthetic
data generation, data mix optimization, and finetuning evaluation
maps directly to Anthropic's need to create and optimize training
data that maximizes Claude's performance on agentic tasks.

---
STAR 5: Agent Communication Architecture & Context Management

JD Requirement: "Ideate, develop, and compare the performance of
different agent harnesses (eg memory, context compression,
communication architectures for agents)" / "Design and build agent
affordances that unlock new capabilities for internal use and
deployed products"

Situation: The RAG-powered study assistant needed to orchestrate 8
interdependent modules through a 10-stage processing pipeline —
screen capture, preprocessing, OCR, parsing, retrieval, context
merging, generation, validation, display, and storage — with each
stage producing typed output consumed by the next. The system
needed real-time progress communication, strict context budget
management, and the ability to swap any module without breaking the
pipeline.

Task: Design an agent communication architecture with typed event
contracts, context compression for LLM token budgets, and modular
interfaces that enable independent development and replacement of
pipeline components.

Action:
- Designed 9 typed WebSocket event types (WSEventTypes) for
  real-time agent-to-frontend communication: capture_started,
  preprocessing_complete, ocr_complete, parsing_complete,
  retrieval_complete, answer_ready, error, document_indexed,
  settings_updated — each carrying structured payloads matching
  backend Pydantic models.
- Implemented context compression via a 3-stage pipeline: semantic
  chunking (300-800 tokens, 15% overlap), cross-encoder re-ranking
  (cross-encoder/ms-marco-MiniLM-L-6-v2), and context budget
  trimming to 2000 tokens before LLM prompt assembly — ensuring
  the most relevant context fits within the generation budget.
- Built contract-first agent interfaces using shared Python
  dataclasses (CaptureResult, ParsedMCQ, RetrievalResult,
  GeneratedAnswer) with mirrored TypeScript interfaces
  (frontend/src/types/index.ts) — type safety across the full
  stack, ensuring agents communicate through validated contracts.
- Defined an 8-agent development framework
  (docs/CLAUDE_CODE_AGENTS.md) where each agent has: identity and
  scope, system prompt, I/O contracts, file structure,
  dependencies, and acceptance criteria. The dependency graph
  enables parallel execution: Agents 1, 2, 7 run concurrently;
  downstream agents wait for their dependencies.
- Implemented per-stage error propagation with an error_stage enum,
  so the system knows exactly where in the pipeline a failure
  occurred — enabling targeted retry rather than full-pipeline
  restart.
- Built a Context Merger and Re-ranker that combines local vector
  retrieval and web results, deduplicates similar content, and
  applies cross-encoder re-ranking — an agent affordance that
  improves answer quality by selecting the highest-relevance
  context within the token budget.

Result: An agent pipeline that processes screen capture to grounded
LLM answer in under 4 seconds across 10 stages, with real-time
progress reporting, strict context compression, and fully typed
inter-agent contracts. The context management system — chunking,
re-ranking, budget trimming — directly addresses the context
compression challenges in agentic systems where agents must
operate within token limits while maximizing information density.
The contract-first design means any agent can be upgraded (swap
Tesseract for EasyOCR, ChromaDB for FAISS) without touching the
rest of the pipeline — the kind of modularity that scales to
large agent systems.

---
STAR 6: Production Model Orchestration & Prompt Engineering

JD Requirement: "Build the prompting and model orchestration for a
production application backed by a language model" / "Have spent
time prompting and/or building products with language models"

Situation: Two production applications — a live social cooking
platform and a RAG-powered study assistant — each required LLM
integration at multiple touchpoints, with different prompt
strategies per feature. The cooking platform needed vision OCR,
allergen-aware substitution, shopping list aggregation, and
recommendation engines. The study assistant needed structured
question answering with evidence citation and confidence scoring.
Both demanded reliable, production-grade prompt orchestration.

Task: Design and implement LLM prompt strategies for 10+ distinct
features across two production applications, with response
caching, structured output validation, error handling, and runtime
model configuration.

Action:
- Integrated GPT-4o at 7 distinct production touchpoints in
  cookbook-club: cookbook cover OCR (vision prompt with structured
  extraction), recipe photo OCR, allergen-safe ingredient
  substitution (constraint-aware prompting respecting dietary
  restrictions), multi-recipe shopping list aggregation (merge
  quantities, categorize by aisle), AI cookbook suggestions based on
  rating history, host-preference-weighted recommendations, and
  conversational AI. Each feature uses a different prompt strategy
  optimized for its specific task.
- Implemented SHA-256 hash-based response caching in a dedicated
  PostgreSQL ai_cache table with configurable TTLs (7-30 days) —
  deterministic cache keys ensure identical prompts return cached
  results, eliminating redundant API calls and reducing latency.
- Built the Claude API integration in studyassistant with
  structured prompt templates (src/generation/prompts.py): question
  stem with qualifier emphasis, source-attributed context, and
  explicit output format instructions (ANSWER, JUSTIFICATION,
  EVIDENCE, CONFIDENCE).
- Implemented output validation ensuring the LLM never selects an
  answer outside the provided options, with explicit uncertainty
  flagging when evidence is insufficient — a GeneratedAnswer
  dataclass captures answer, generation time, tokens used, model
  version, confidence level (HIGH/MEDIUM/LOW), and an uncertain
  boolean.
- Created configurable generation settings (model, max_tokens,
  temperature, timeout) managed through YAML config and exposed via
  API, enabling runtime model switching without redeployment.
- Built resilient orchestration with fallback chains: OpenAI client
  uses lazy initialization via JavaScript Proxy to avoid crashes
  when API key is missing; email sending falls back from Gmail API
  to SES.

Result: Two production applications with 10+ LLM-powered features
serving live users — cookbook-club at cookbookclub.vip with 7
GPT-4o integrations, and studyassistant with Claude API structured
inference in under 1.5 seconds. The prompt engineering spans vision
OCR, constraint-aware generation, multi-document aggregation,
recommendation, and evidence-cited question answering — each
requiring a different prompting approach. The response caching and
output validation patterns demonstrate production-grade LLM
orchestration that's reliable enough for real users, not just
demos — the same reliability standard Anthropic's deployed
products demand.

---
Summary: JD Requirement Coverage Matrix

Complex agentic systems
  4 PPO agents with adversarial hardening, solver loop
  coordination (residualstreambackdoor); 8-agent framework
  (studyassistant)

Agent harness design (memory, context compression, comms)
  Cross-strategy data flow, convergence detection, idempotent
  state machine (residualstreambackdoor); context budgeting,
  WebSocket events (studyassistant); response caching
  (cookbook-club)

Quantitative benchmarks for agentic tasks
  Probe reliability scoring, ~70% accuracy, PCA separability
  (residualstreambackdoor); 84/100 output quality
  (HiddenSignalTest)

Automated model/prompt evaluation
  500+ decoding configs with TF-IDF clustering
  (residualstreambackdoor); confidence scoring, uncertainty
  flagging (studyassistant); batch evaluation tools
  (HiddenSignalTest)

Data mixes for model training
  4,323-line generator, 8 weighted categories, SHA-256 dedup
  (HiddenSignalTest); adversarial training data
  (residualstreambackdoor)

Large-scale RL on language models
  4 PPO agents in PyTorch: PPOSolverAgent,
  PPOTriggerDiscovery, PPOAdversary, PPOMonitoringPolicy
  (residualstreambackdoor)

Multi-agent systems
  4 PPO agents with solver loop (residualstreambackdoor);
  8-agent dev framework with dependency graph (studyassistant)

LLM finetuning
  QLoRA on CodeLlama-7B: 4-bit NF4, LoRA r=64/a=16,
  SFTTrainer (HiddenSignalTest)

Prompting / building products with LLMs
  7 GPT-4o features in production (cookbook-club); Claude API
  with structured prompts (studyassistant); 500+ decoding
  configs (residualstreambackdoor)

Software engineering and ML experience
  10,400+ lines, 205 tests, strict mypy, 6-job CI/CD
  (residualstreambackdoor); production TypeScript monolith,
  124+ endpoints (cookbook-club)

Model internals / interpretability
  Residual stream probing, CCS, PCA, attention divergence,
  weight forensics on 671B model (residualstreambackdoor)

AI safety / beneficial technology
  Backdoor detection for model governance
  (residualstreambackdoor); uncertainty flagging, confidence
  scoring (studyassistant)
