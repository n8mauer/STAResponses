Nate Mauer
AI/ML Research Engineering | Multi-Agent Systems | LLM Interpretability & Finetuning
C: (703) 966-1007 | E: n8mauer@gmail.com | GH: github.com/n8mauer
LI: www.linkedin.com/in/natemauer | Top Secret Clearance

SUMMARY

Research engineer building multi-agent RL systems, agent evaluation
environments, and LLM finetuning pipelines. Built two OpenEnv-compliant
agent environments — a multi-agent negotiation simulation with MCTS
planning, CART behavioral probes, and Claude tool-use integration, and
a long-horizon HR simulation with GRPO RL training and 150-200+ step
episodes. Implemented 4 PPO agents for backdoor detection on a
671B-parameter model with residual stream probing. Fine-tuned
CodeLlama-7B via QLoRA achieving 84/100 output quality. Built
production LLM applications with Claude API and GPT-4o serving live
users. 15 years of applied AI experience spanning PyTorch RL, MCTS,
agent harness design, model interpretability, and quantitative
benchmarking.

TECHNICAL SKILLS

  Agent Systems & Search:   PPO (PyTorch), MCTS (UCB1), GRPO (TRL),
                            multi-agent coordination, adversarial
                            hardening, agent harness design, solver
                            loop orchestration, minimax, alpha-beta
                            pruning, OpenEnv environments, convergence
                            detection, phase-gated state machines

  Model Training & Eval:    QLoRA/LoRA finetuning (Unsloth, SFTTrainer),
                            GRPO reward function design, synthetic data
                            generation, dataset mix optimization, probe
                            reliability scoring, CART behavioral probes,
                            pytest-parametrized evaluation frameworks

  LLM Integration:          Claude API (chat + tool-use), GPT-4o,
                            Anthropic SDK, prompt orchestration,
                            structured output validation, RAG
                            (ChromaDB, sentence-transformers,
                            cross-encoder re-ranking), context
                            compression, experience replay buffers

  Model Interpretability:   Residual stream probing, linear probes
                            (L1/L2/CART), CCS unsupervised labeling,
                            PCA separability diagnostics, attention
                            divergence analysis, weight forensics

  Infrastructure:           Python, PyTorch, scikit-learn, HuggingFace,
                            FastAPI, Pydantic, OpenEnv, AWS (SageMaker,
                            Lambda, Step Functions, EC2 GPU),
                            Terraform, Docker, GitHub Actions CI/CD

PROJECTS

Multi-Agent Negotiation Environment — OpenEnv Hackathon SF (Meta/HF)
github.com/n8mauer/OpenEnv_Hackathon
- Built Nexus, a multi-agent compute cluster negotiation simulation
  where 2-8 agents trade scarce resources (GPU/CPU/memory/bandwidth),
  form coalitions, and complete jobs under deadlines — with a 7-phase
  round loop (Event > Observe > Negotiate > Action > Execute > Score >
  Oversight) and partial observability via natural language rendering
- Implemented Monte Carlo Tree Search (MCTS) with UCB1 selection for
  strategic negotiation planning under hidden information — agents
  simulate opponent responses to find optimal bid/offer/coalition moves
- Designed Mixture-of-Experts (MoE) voting for coalition resource
  allocation: confidence-weighted member voting with proposer bonus,
  elimination pre-pass, and validation against job requirements
- Built CART behavioral probes (RandomForest, 20 trees) for real-time
  anomaly detection: 8-dim feature vectors (trade frequency, price
  deviation, hoarding ratio, coalition frequency, resource asymmetry,
  message volume, deadline miss rate, budget velocity) with pair-wise
  collusion detection for off-market trades
- Integrated Claude via Anthropic tool-use SDK with 8 structured tools,
  experience replay buffer (best-N + recent-N memory injection), and
  behavioral policy adaptation with learning rate decay (0.995x)
- 7 agent types: Random, Greedy, Strategic/MCTS, LLM/Claude,
  Supervisor, CTO, Worker; market physics with price impact and
  commission; Sharpe ratio + Gini coefficient fairness metrics
- 4,255 lines Python + 795 lines pytest across 6 test modules

Long-Horizon Agent Evaluation — OpenEnv RL Environment
github.com/n8mauer/HCM-21
- Built HCM:21, an OpenEnv-compliant simulation where AI agents manage
  200-500 employees across 5 departments over 6 quarters (150-200+
  steps) with sparse delayed rewards — decisions in Q1 show ROI in Q3+
- Designed a 4-phase state machine (Scan > Plan > Produce > Control)
  gating 16 action types with minimum action enforcement per phase —
  agents cannot skip strategic steps (no hiring without planning)
- Implemented Cobb-Douglas production functions per department with
  logistic flight risk modeling, 8 stochastic event types with
  cross-quarter cascading, and Fitz-enz HR metrics (HCVA, HCROI, QIPS,
  Five Indexes of Change, Employee Value)
- Built a 6-component scoring system calibrated across agent baselines:
  Random 0.15-0.25, Heuristic 0.50-0.65, Strong LLM agent 0.70+ —
  combining HCVA trajectory trend, HCROI improvement, QIPS consistency,
  employee value, five indexes, and financial health
- Trained agents via GRPO (TRL + Unsloth) on Qwen3-0.6B with LoRA
  rank=16, 4-bit quantization — custom reward function executes full
  episodes in environment, returns terminal score as RL signal
- Claude-based agent with phase-aware prompting, quarterly LLM-generated
  memory compression (500 tokens per quarter), and rolling context
  window management for 100K+ token episode histories
- 2,478 lines Python + 400 lines pytest; deployed on HuggingFace Spaces

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

Federal Code Refactoring Pipeline — Agent-Orchestrated LLM Platform
github.com/n8mauer/HiddenSignalTest
- 3-person team coded and sold a code refactoring pipeline to the
  Federal government, processing 2M+ lines of legacy mainframe code
  (COBOL/Assembly) through a 200K token context window into production
  Python — fine-tuned CodeLlama-7B via QLoRA achieving 84/100 output
  quality, 85-90% business logic accuracy, and 6,435 lines generated
- Step Functions sense-plan-act agent harness: the training lifecycle
  (GenerateDataset > TrainModel > DeployModel) senses current state
  (checkpoint status, spot instance availability), plans conditional
  deployment decisions, and acts with checkpoint recovery for spot
  interruptions — the same autonomous loop governing long-horizon agents
- Intelligent document chunking agents: metadata extraction > section
  detection (numbered requirements, headers, markdown structure) >
  context-preserving overlap splits > MD5-cached per-chunk processing >
  coherent result merging — processing 2M+ LOC through 200K token window
- Multi-run seed curriculum strategy: 5 seeded runs (seeds 42-46) x
  10K examples = 50K training examples with weighted category mixing
  across 8 domains — data mix optimization maximizing pattern diversity
  with different parameter variations and UUID values per seed
- Dual deployment (SageMaker Serverless + HuggingFace Endpoints) +
  Terraform cost optimization (spot training 90% savings, budget alerts,
  idle auto-termination) + GovCloud/FedRAMP compliance

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
