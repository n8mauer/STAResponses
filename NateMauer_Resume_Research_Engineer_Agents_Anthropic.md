Nate Mauer
AI/ML Research Engineering | Multi-Agent Systems | LLM Interpretability & Finetuning
C: (703) 966-1007 | E: n8mauer@gmail.com | GH: github.com/n8mauer
LI: www.linkedin.com/in/natemauer | Top Secret Clearance

SUMMARY

Research engineer building multi-agent RL systems, LLM finetuning
pipelines, and model evaluation frameworks. Implemented 4 PPO agents
for backdoor detection on a 671B-parameter model with residual stream
probing across all transformer layers. Built game-playing agents with
minimax and alpha-beta pruning across 5 variants including partial
observability. Fine-tuned CodeLlama-7B via QLoRA with custom synthetic
data generation, achieving 84/100 output quality. Built production LLM
applications with Claude API and GPT-4o serving live users. 15 years
of applied AI experience across Fortune 500, federal, and personal
research projects, with hands-on work spanning PyTorch RL, adversarial
search, model interpretability, agent harness design, and quantitative
benchmarking.

TECHNICAL SKILLS

  Agent Systems & Search:   PPO (PyTorch), multi-agent coordination,
                            adversarial hardening, agent harness
                            design, solver loop orchestration,
                            minimax, alpha-beta pruning, heuristic
                            evaluation, convergence detection,
                            state machines

  Model Training & Eval:    QLoRA/LoRA finetuning, SFTTrainer,
                            synthetic data generation, dataset mix
                            optimization, probe reliability scoring,
                            pytest-parametrized evaluation frameworks,
                            A/B model comparison, adversarial testing

  LLM Integration:          Claude API, GPT-4o, prompt orchestration,
                            structured output validation, RAG
                            (ChromaDB, sentence-transformers,
                            cross-encoder re-ranking), context
                            compression, token budget management

  Model Interpretability:   Residual stream probing, linear probes
                            (L1/L2/CART), CCS unsupervised labeling,
                            PCA separability diagnostics, attention
                            divergence analysis, weight forensics

  Infrastructure:           Python, PyTorch, scikit-learn, HuggingFace,
                            FastAPI, AWS (SageMaker, Lambda, Step
                            Functions, EC2 GPU, CloudFormation),
                            Terraform, Docker, GitHub Actions CI/CD

PROJECTS

LLM Backdoor Detection — Multi-Agent RL & Interpretability
github.com/n8mauer/residualstreambackdoor
- Designed and implemented 4 PPO reinforcement learning agents in
  PyTorch for detecting hidden backdoor triggers in a 671B-parameter
  LLM (DeepSeek V3): PPOSolverAgent (35-dim state, 1600 actions),
  PPOTriggerDiscoveryAgent (128-dim, 3000 actions), PPOAdversaryAgent
  (64-dim), and PPOMonitoringPolicy (accept/reject/escalate)
- Built a solver loop harness with cross-strategy data flow —
  residual stream probe results feed behavioral analysis, behavioral
  motifs feed probe trigger scanning — with automatic convergence
  detection and adversarial hardening cycles
- Implemented per-layer residual stream probing across all 32
  transformer layers with L2/L1/CART ensemble probes, CCS unsupervised
  labeling, and PCA separability diagnostics; ~70% accuracy on 8B
  warmup model with strongest signal in layers 10-18
- Designed probe reliability scoring combining sample size, train-test
  gap, layer position, and PCA separability — caught false confidence
  in early experiments before wasting compute on the 671B model
- Automated evaluation over 500+ decoding configurations scored via
  attention divergence, entropy collapse, and text divergence metrics,
  with TF-IDF + DBSCAN clustering for motif discovery
- 10,400+ lines Python, 205 tests, strict mypy, 6-job CI/CD pipeline,
  LaTeX paper with TikZ diagrams

Legacy Code Modernization — LLM Finetuning & Synthetic Data
github.com/n8mauer/HiddenSignalTest
- Fine-tuned CodeLlama-7B via QLoRA (4-bit NF4, LoRA r=64/a=16,
  SFTTrainer, paged_adamw_32bit, BF16 mixed precision) achieving
  84/100 output quality and 85-90% business logic accuracy
- Built a 4,323-line synthetic dataset generator producing
  instruction-tuning data across 8 weighted categories with SHA-256
  deduplication; dataset sizing analysis identified 10K examples as
  optimal for ~1,000-2,000 unique patterns
- Deployed via SageMaker Serverless Inference and HuggingFace
  Inference Endpoints for A/B hosting comparison; Step Functions
  orchestration (GenerateDataset > TrainModel > DeployModel)
- Terraform IaC with cost optimization: spot training, budget alerts,
  idle auto-termination; GovCloud deployment path for FedRAMP

RAG Study Assistant — Agent Orchestration & Context Management
github.com/n8mauer/studyassistant
- Architected an 8-agent framework with dependency graph, typed I/O
  contracts (Python dataclasses + mirrored TypeScript interfaces),
  and per-agent system prompts, acceptance criteria, and scoping
- Built a 10-stage pipeline with context compression: semantic
  chunking (300-800 tokens), cross-encoder re-ranking, 2000-token
  budget trimming — screen capture to Claude API answer in <4 seconds
- Designed 9 typed WebSocket events for real-time agent-to-frontend
  communication with per-stage error propagation (error_stage enum)
- Structured output validation with confidence scoring
  (HIGH/MEDIUM/LOW), uncertainty flagging, and evidence citation

AI-Integrated Production Platform — LLM Prompt Engineering at Scale
github.com/n8mauer/cookbook-club | Live: cookbookclub.vip
- Integrated GPT-4o at 7 production touchpoints with distinct prompt
  strategies per feature: vision OCR, allergen substitution, shopping
  list aggregation, recommendation engines, conversational AI
- SHA-256 hash-keyed response caching in PostgreSQL with configurable
  TTLs; production TypeScript monolith with 124+ REST endpoints,
  dual auth (OAuth + JWT), React 18 frontend, React Native mobile

Game-Playing Agent Framework — Adversarial Search & Multi-Agent AI
github.com/n8mauer/tic-tac-toeandconnectfour
- Implemented minimax (full game tree) for Tic-Tac-Toe and alpha-beta
  pruning (depth-limited) for Connect 4, with depth-penalized scoring
  to prefer faster wins — Tic-Tac-Toe agent achieves perfect play
- Designed a heuristic evaluation function scoring board positions
  across 4 directions with asymmetric threat weighting: +100 win,
  +10 near-win, -80 opponent threat, +3 center control — enabling
  strong play on variable-sized boards (up to 12x14)
- Supported 5 game variants through polymorphic agent dispatch:
  standard Tic-Tac-Toe, Connect 4 Basic/Extended/Multiplayer
  (3 players), and Hidden Multiplayer with partial observability
  where opponent tokens are masked — agent reasons under uncertainty
- Built GameAgent vs RandomAgent benchmarking with GameStats tracking
  win/loss/draw outcomes, timing, and move counts per variant

Market Simulation — Quantitative Evaluation Framework
github.com/n8mauer/Marketsim
- Built a portfolio simulation engine processing time-ordered BUY/SELL
  actions across multiple securities with transaction cost modeling
  (fixed commission + percentage market impact) and forward/back-fill
  for missing market dates
- Computed 4 quantitative metrics (Sharpe ratio √252, cumulative
  return, average daily return, std of daily returns) benchmarked
  against SPY for relative performance assessment
- Designed a pytest-parametrized automated grading framework (456
  lines) with 12 namedtuple-defined test cases across 3 cost model
  groups, tolerance-based assertions (0.1%), timeout enforcement, and
  NaN detection — a pattern directly applicable to agent evaluation

PROFESSIONAL EXPERIENCE

Booz Allen Hamilton — AI/ML Engineering Leader        2019 - Present
- Led development of a multi-pass LLM code generation pipeline
  transforming legacy codebases into executables using a 4-stage
  orchestration layer on AWS SageMaker endpoints in GovCloud
- Designed and deployed agentic AI systems with 4 PPO reinforcement
  learning agents for multi-strategy coordination, cross-strategy
  data flow with convergence detection, and adversarial hardening
  cycles alternating probe and adversary training
- Fine-tuned CodeLlama-7B via QLoRA achieving 30+ point quality
  improvements over frontier models; built synthetic data generation
  pipelines with weighted category mixes and deduplication
- Designed model evaluation frameworks: probe reliability scoring
  (composite of sample size, train-test gap, layer position, PCA
  separability), automated eval over 500+ decoding configurations,
  and A/B model comparisons with adversarial prompt testing
- Built agent harness infrastructure: idempotent state machines with
  JSON-persisted state, cross-strategy communication architectures,
  and dual-format observability journals (JSONL + Markdown)
- Deployed production generative AI applications using Claude API,
  GPT-4o, and HuggingFace with structured output validation,
  context compression, and response caching — 90% improvement in
  delivery timelines
- Led 100s of onsite customer engagements from ideation to production,
  empowering federal teams to leverage applied AI
- Built and led cross-functional AI engineering teams; facilitated
  training programs on PyTorch, HuggingFace, and MLOps tooling

Mauer Consulting, LLC — Founder                      2017 - 2019
- Delivered data intelligence solutions; applied Monte Carlo
  simulation and quantitative analysis across 4 emerging markets

America's Future Workforce — CEO & Chairman           2011 - 2017
- Founded B2B talent platform with ML-powered skills assessment
  and real-time analytics dashboards for workforce forecasting

The White House, Presidential Correspondence — Associate      2009
- Implemented text classification and automated content tagging
  for 200+ daily communications, reducing backlog by 20,000+ letters

EDUCATION

  M.S. Computer Science, Machine Learning
    Georgia Institute of Technology (Expected 2026)
  Post-Graduate Certificate AI/ML
    University of California, Berkeley (2024)
  Post-Graduate Certificate Algorithmic Trading
    University of Oxford (2022)
  M.A. International Business & Policy
    Georgetown University (2020)
  B.A. Political Science
    Wheaton College (2009)
