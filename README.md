# BCG Platinion — Principal Architect, AI Platforms
## Portfolio Evidence: Technical Summary

Thank you for the opportunity. I'm a research engineer who builds multi-agent systems, agent evaluation environments, and LLM training pipelines — from RL reward functions to production infrastructure. I don't just design systems on whiteboards; I ship them.
---

Four repositories demonstrate end-to-end coverage of the Principal Architect – AI Platforms competency model. The work spans the full AI system lifecycle: model training and fine-tuning, inference architecture, RAG pipeline design, agentic orchestration, cloud-native deployment, and production operation — implemented in Python, TypeScript, and IaC (Terraform/CloudFormation) across 17 AWS services.

### AI Architecture & LLM Integration

**studyassistant** implements a complete RAG pipeline — document ingestion, semantic chunking (300-800 tokens, 15% overlap), vector embedding (sentence-transformers/ChromaDB), similarity retrieval with cross-encoder re-ranking, and context-budgeted prompt assembly — driving Claude API inference with structured output parsing, confidence scoring, and evidence citation. Latency-budgeted at <4s E2E across a 10-stage processing pipeline with parallel retrieval paths.

**cookbook-club** integrates GPT-4o at 7 production touchpoints (vision OCR, allergen substitution, shopping list aggregation, recommendation engines) behind SHA-256 hash-keyed response caching with TTL-managed invalidation in PostgreSQL. Serves live traffic at `cookbookclub.vip`.

### Model Training, Fine-Tuning & Interpretability

**HiddenSignalTest** fine-tunes CodeLlama-7B via QLoRA (4-bit NF4 quantization, LoRA r=64/α=16, paged_adamw_32bit, BF16 mixed precision) on synthetically generated instruction-tuning data across 8 weighted business-logic categories. Deployed as SageMaker Serverless Inference behind API Gateway + Lambda, orchestrated via Step Functions (GenerateDataset → TrainModel → DeployModel). Achieves 84/100 output quality at <$1/100 files for mainframe-to-Python modernization.

**residualstreambackdoor** operates at the frontier of LLM interpretability — probing residual stream activations across all transformer layers of a 671B-parameter model (DeepSeek V3) to detect fine-tuned backdoor triggers. Implements per-layer linear probes (L2/L1/CART ensembles), CCS unsupervised labeling, PCA separability diagnostics, and 4 PPO reinforcement learning agents (solver, trigger discovery, adversary, monitor) with adversarial hardening cycles. Cross-strategy data flow between memory extraction (TF-IDF + DBSCAN clustering over 500+ decoding configurations) and activation probing converges via a meta-ensemble solver loop.

### Cloud Infrastructure & MLOps

AWS infrastructure spans EC2 (GPU: g5.2xlarge/p5.48xlarge), RDS, S3, SES, SNS, Lambda, API Gateway, CloudFront, SageMaker, Step Functions, ECR, CloudFormation, SSM, CloudWatch, IAM, Budgets, and STS. IaC via Terraform (modules for cost optimization: spot instances, budget alerts, idle auto-termination) and CloudFormation (332-line stack with launch templates, lifecycle rules, IAM roles). GovCloud deployment documented for FedRAMP/ITAR compliance (FIPS 140-2, data residency controls).

CI/CD enforced via GitHub Actions (6-job parallel pipeline: ruff, mypy strict, bandit, pip-audit, pytest, formatting) gating 205 tests with moto-mocked AWS, pre-commit hooks, and dual-format observability journaling (JSONL + Markdown).

### Full-Stack & Production Systems

**cookbook-club** is a production TypeScript monolith (React 18 + Express 5 + PostgreSQL 16) with 22+ tables, 124+ REST endpoints, dual auth (OAuth sessions + JWT Bearer), Helmet.js CSP, presigned S3 uploads, and React Native/Expo mobile. **studyassistant** pairs a FastAPI async backend with a React + Vite + Tailwind frontend communicating over 9 WebSocket event types for real-time pipeline status.

### Alignment to Role

| Competency | Evidence |
|---|---|
| AI/LLM solution architecture | RAG pipelines, prompt orchestration, structured inference (studyassistant, cookbook-club) |
| Agentic AI & multi-agent systems | 4 PPO agents with adversarial hardening (residualstreambackdoor), 8-agent dev framework (studyassistant) |
| Model training & fine-tuning | QLoRA on CodeLlama-7B, synthetic dataset generation (HiddenSignalTest) |
| LLM interpretability & safety | Residual stream probing, CCS, weight forensics on 671B model (residualstreambackdoor) |
| Cloud architecture (AWS) | 17 services across 3 repos, Terraform + CloudFormation IaC, GPU autoscaling, cost optimization |
| MLOps / LLMOps | CI/CD pipelines, probe reliability scoring, training orchestration, observability dashboards |
| Vector DBs & embeddings | ChromaDB, sentence-transformers, PCA activation analysis |
| API design & integration | REST, WebSocket, SSE, event-driven, serverless Lambda — 4 paradigms across 4 repos |
| Full-stack delivery | Production React + TypeScript apps, dual auth, real-time WebSocket, mobile |
| Technical communication | 400KB+ documentation, LaTeX paper, demo guides with talk tracks, architecture decision records |
| Business value | 1000x cost reduction (HiddenSignalTest), live production platform (cookbook-club), <4s latency (studyassistant) |

