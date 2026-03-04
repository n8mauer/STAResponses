# STAR Responses: BCG Platinion Principal Architect - AI Platforms
## API Design Across Paradigms (Cross-Repository)

---

## STAR 15: API Design Across Paradigms

**JD Requirement:** *"Understanding of cloud architecture, data management, and API-based system integration" / "Integration of agent workflows with enterprise APIs, knowledge bases" / "Design AI integration patterns... for external system connectivity"*

**Situation:** Across four independent projects, I encountered fundamentally different integration requirements — a multi-tenant web/mobile platform needing synchronous REST with real-time streaming, an AI pipeline requiring bidirectional WebSocket communication for live progress, a GPU-scale research orchestrator needing asynchronous event-driven coordination with human-in-the-loop approval, and a serverless inference platform needing stateless request/response with multi-provider backend abstraction. Each demanded a distinct API paradigm, but all shared common design principles around contract-first design, boundary validation, structured error propagation, and context-appropriate authentication.

**Task:** Design and implement API layers across 4 projects using the right paradigm for each context — REST + SSE streaming, async REST + WebSocket event bus, event-driven pub-sub with approval protocol, and serverless Lambda with multi-provider inference — while maintaining consistent quality standards for validation, error handling, and security.

**Action:**

### 1. REST + SSE Streaming (cookbook-club)
- Designed **124+ REST endpoints** across 14 resource domains (families, members, cookbooks, recipes, meetings, circles, connections, badges, AI features) following RESTful conventions with consistent URL patterns and HTTP verb semantics.
- Implemented **Zod schema validation** at every API boundary using Drizzle-Zod `.parse()`, ensuring type-safe request/response contracts shared between client and server via a single schema definition.
- Built **dual authentication middleware** (`isAuthenticated`) transparently handling both session-based auth (web, via `connect-pg-simple`) and JWT Bearer tokens (mobile, HS256 with 7-day access / 30-day refresh) in a single middleware path — enabling one API surface to serve both web and mobile clients.
- Implemented **SSE (Server-Sent Events) streaming** for OpenAI GPT-4o responses, allowing the frontend to display AI-generated content (recipe suggestions, allergen substitutions, shopping lists) progressively as tokens arrive rather than waiting for full completion.
- Designed a **Repository Pattern** with an `IStorage` interface (70+ methods) backed by `DatabaseStorage` using Drizzle ORM, cleanly separating API route handlers from data access logic.
- Applied **Helmet.js CSP headers** with strict Content Security Policy, verified by automated integration tests — preventing XSS even if API responses contain user-generated content.
- Implemented **presigned S3 URL flow** for image uploads: client requests a time-limited PUT URL via the API, uploads directly to S3, then stores a proxy path — keeping the S3 bucket fully private while avoiding server-side file handling.
- Built a **global error handler** middleware with structured error responses and a `/health` endpoint for operational monitoring.

### 2. Async REST + WebSocket Event Bus (studyassistant)
- Built a **FastAPI async backend** with auto-generated OpenAPI documentation, enabling contract-first development where the API schema serves as the living specification.
- Defined **Pydantic models** for all request/response types with automatic validation, serialization, and OpenAPI schema generation — eliminating manual validation code.
- Designed **9 typed WebSocket event types** (`WSEventTypes`: `capture_started`, `preprocessing_complete`, `ocr_complete`, `parsing_complete`, `retrieval_complete`, `answer_ready`, `error`, `document_indexed`, `settings_updated`) providing real-time pipeline progress to the frontend as each processing stage completes.
- Implemented **contract-first design** using shared Python dataclasses (`CaptureResult`, `ParsedMCQ`, `RetrievalResult`, `GeneratedAnswer`) that flow between pipeline stages, with mirrored TypeScript interfaces (`frontend/src/types/index.ts`) ensuring full-stack type safety.
- Built **per-stage error propagation** with an `error_stage` enum, so the frontend can display exactly where in the 10-stage pipeline a failure occurred and what the last successful stage was — enabling targeted debugging rather than generic error messages.

### 3. Event-Driven + Two-Way Approval Protocol (residualstreambackdoor)
- Designed an **SNS pub-sub notification system** with 6 distinct notification methods: iteration results, trigger candidates, error alerts, phase transitions, trigger confirmation requests, and completion summaries — each with structured JSON payloads.
- Implemented an **SSM Parameter Store approval gate** for human-in-the-loop decisions: the orchestrator publishes a trigger candidate via SNS, writes the candidate to SSM, then polls for human approval — with a configurable timeout-based auto-continue that prevents the pipeline from blocking indefinitely.
- Built an **8-phase idempotent state machine** (`init → validated → provisioned → launched → bootstrapped → executing → collected → cleaned_up`) with JSON-persisted state and `at_least()` guard checks, ensuring any phase can be safely retried without side effects.
- Designed **graceful degradation** where notification failures (SNS publish errors, SSM write errors) are logged but never block pipeline orchestration — the research workload continues even if the notification subsystem is unreachable.
- Created a **CLI interface** (`dormant-aws`) via Click with commands mapping to state machine phases (`run`, `provision`, `launch`, `execute`, `solve`, `collect`, `status`, `cleanup`, `destroy`), providing both automated and interactive operation modes.

### 4. Serverless Lambda + Multi-Provider Inference (HiddenSignalTest)
- Authored a **SAM/CloudFormation template** defining API Gateway with CORS configuration, X-Ray tracing, and structured logging — infrastructure-as-code ensuring the API definition is versioned and reproducible.
- Implemented `validate_request()` with **field-level bounds checking** (temperature 0.0–2.0, max_tokens 1–2048, top_p 0.0–1.0) and descriptive error messages, catching invalid inputs before they reach the inference backend.
- Built a `create_response()` **factory function** with conditional CORS headers, standardizing all Lambda responses with consistent status codes, content types, and cross-origin headers.
- Designed a **dual backend abstraction** behind a single API endpoint: SageMaker `invoke_endpoint` for the self-hosted fine-tuned model and HuggingFace Inference API with Bearer token authentication — selected via environment variable, enabling deployment-time provider switching without code changes.
- Implemented **environment-based staging** (dev/staging/prod) via CloudFormation parameters, with separate IAM roles, resource tags, and endpoint configurations per environment.

### Cross-Cutting Design Principles

| Principle | cookbook-club | studyassistant | residualstreambackdoor | HiddenSignalTest |
|-----------|-------------|---------------|----------------------|-----------------|
| **Contract-first** | Zod schemas (shared client/server) | Pydantic models + TS interfaces | State machine phase contracts | SAM template resource definitions |
| **Validation at boundary** | Drizzle-Zod `.parse()` | Pydantic auto-validation | `at_least()` state guards | `validate_request()` bounds checking |
| **Structured errors** | Global Express error middleware | `error_stage` enum per pipeline stage | Logged soft failures, never block | `create_response()` factory with status codes |
| **Auth per context** | Session + JWT dual-path (web/mobile) | Local-first (no auth needed) | IAM roles + SSM for machine-to-machine | IAM for Lambda, Bearer tokens for HuggingFace |
| **Real-time patterns** | SSE for streaming LLM responses | WebSocket for pipeline progress | SNS for async notifications | Request/response (stateless) |

**Result:** Four projects, four API paradigms — each selected for the specific integration context rather than defaulting to a single pattern. The consistent application of contract-first design, boundary validation, and structured error handling across all four demonstrates a principled approach to API architecture that adapts the paradigm to the problem. This directly addresses the JD's requirement for "API-based system integration" and "external system connectivity" — not as abstract knowledge, but as demonstrated implementation across REST, WebSocket, event-driven, and serverless patterns. The range shows the ability to advise BCG Platinion clients on the right API strategy for their specific integration landscape, whether that's a customer-facing web/mobile platform, a real-time AI pipeline, an async research orchestrator, or a serverless inference service.

---

## Key File References

| Repo | Key API Files |
|------|--------------|
| **cookbook-club** | `server/routes.ts` (124+ endpoints), `server/middleware/` (auth, error handling), `shared/schema.ts` (Zod schemas), `server/storage.ts` (IStorage interface) |
| **studyassistant** | `src/api/` (FastAPI routes), `src/api/websocket.py` (WSEventTypes), `docs/SHARED_MODELS.md` (contract definitions), `frontend/src/types/index.ts` (TS interfaces) |
| **residualstreambackdoor** | `src/orchestrator.py` (state machine), `src/notifications.py` (6 SNS methods), `infrastructure/cloudformation.yaml` (332-line stack) |
| **HiddenSignalTest** | `lambda/app.py` (validate_request, create_response), `template.yaml` (SAM/CloudFormation), `terraform/` (multi-environment IaC) |
