# Opening Statement

---

Thank you for the opportunity. I'm an AI platforms architect who builds end-to-end — from model internals to production infrastructure. I don't just design systems on whiteboards; I ship them.

I've fine-tuned LLMs using QLoRA on SageMaker, built RAG pipelines with vector retrieval and cross-encoder re-ranking, and deployed multi-agent systems with PPO-trained reinforcement learning agents — all backed by infrastructure I authored in Terraform and CloudFormation across 17 AWS services.

To give you a sense of range and depth:

I built a **legacy code modernization platform** that fine-tunes CodeLlama-7B to translate mainframe business logic into production Python — reducing transformation cost by 1000x. The full pipeline runs on SageMaker with Step Functions orchestration, serverless inference behind API Gateway, and Terraform-managed cost controls including spot training and idle endpoint auto-termination. I documented the GovCloud deployment path for FedRAMP compliance because that's the conversation enterprise clients actually need to have.

I architected a **backdoor detection system** for 671-billion parameter LLMs — probing residual stream activations with per-layer linear classifiers, unsupervised CCS labeling, and custom CART ensembles, hardened by adversarial PPO agents. That codebase has 205 tests, strict mypy, and a 6-job CI/CD pipeline because research code that can't be trusted produces results that can't be trusted.

I designed and shipped a **production AI-integrated platform** serving live users — React 18, Express 5, PostgreSQL, GPT-4o wired into 7 distinct features with hash-keyed response caching, multi-provider OAuth, and presigned S3 uploads. It's live at cookbookclub.vip. Not a demo. Not a proof-of-concept.

And I built a **RAG-powered study assistant** with a 10-stage processing pipeline — screen capture, OCR, semantic chunking, ChromaDB vector retrieval, context merging, and Claude API generation — latency-budgeted at under 4 seconds, with every module designed against explicit interface contracts so components can be swapped without touching the rest of the system.

What ties all of this together is how I think about architecture: modular boundaries with strict contracts, options analysis with quantified tradeoffs, and a bias toward shipping working systems over producing slide decks. I evaluate technology choices the way this role demands — not "what's newest" but "what's right for the constraint set." I've written the decision matrices, the cost models, the implementation paths, and then built the thing.

I'm comfortable at every layer — PyTorch model internals, cloud infrastructure provisioning, API design, frontend delivery — and I'm effective at translating between them for technical and non-technical stakeholders alike. That's the value I'd bring to BCG Platinion's clients: an architect who can walk into a room with a CTO, understand the business problem, design the AI platform strategy, and then validate it with working code.
