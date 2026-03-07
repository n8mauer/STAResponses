Nathaniel (Nate) Mauer
n8mauer@gmail.com
C: 703-966-1007

07/07/2025

Anthropic
San Francisco, CA

Re: Research Engineer, Agents

Dear Hiring Team,

I want to help make Claude a better agent. I've spent the last several years building the exact systems your JD describes — multi-agent coordination, agent harness design, quantitative benchmarks, automated evaluation, and LLM finetuning — and I've hit the problems that make this work hard. Partial cache hit ambiguity under multi-agent interleaving creates unpredictable latency variance across turns. Dependency-aware DAG orchestration breaks down when agent outputs invalidate upstream assumptions. Context compression forces tradeoffs between information density and retrieval fidelity that no single heuristic resolves. These aren't theoretical concerns for me; they're bugs I've debugged in my own systems, and the reason I want to work on them at the frontier.

My strongest signal is residualstreambackdoor — a multi-agent RL system where 4 PPO agents coordinate to detect hidden backdoor triggers in a 671B-parameter LLM. I designed the agent harness: a solver loop where residual stream probe results feed behavioral analysis, behavioral motifs feed probe trigger scanning, and a PPOMonitoringPolicy acts as a quality gate with accept/reject/escalate actions over the other agents' outputs. The cross-strategy communication architecture required solving exactly the kind of agent coordination problems your team works on — when do agents share findings, how do you detect convergence without premature termination, and how do you harden agents against adversarial inputs through iterative training cycles. That codebase has 10,400+ lines of Python, 205 tests, strict mypy, and a 6-job CI/CD pipeline because agent systems that can't be tested produce results that can't be trusted.

I've also built agent systems beyond LLMs. My game-playing agent framework implements minimax and alpha-beta pruning across 5 game variants — including a 3-player Hidden Multiplayer mode where the agent only sees its own tokens and must reason under partial observability. The polymorphic dispatch architecture (one GameAgent class handling all 5 variants through algorithmic selection) is a small-scale version of the same design problem your team faces: how do you build a single agent harness that adapts its strategy to task complexity? I designed heuristic evaluation functions with asymmetric threat weighting that enable strong play on variable-sized boards — the kind of quantitative evaluation design that transfers directly to benchmarking agentic task performance.

On the evaluation side, I've built quantitative benchmarking at two scales. For the backdoor detection system, I designed probe reliability scoring that combines 4 independent signals (sample size, train-test gap, layer position, PCA separability) into a composite metric — it caught false confidence in early experiments before wasting compute on the 671B model. For my market simulation work, I built a pytest-parametrized automated grading framework with 12 namedtuple-defined test cases, tolerance-based assertions at 0.1%, and multi-metric validation (Sharpe ratio, cumulative return, daily return statistics). That evaluation pattern — structured test case definitions, parametrized execution across scenarios, quantitative threshold validation — is directly applicable to rigorous agent benchmarking.

For data mix optimization, I built a 4,323-line synthetic dataset generator producing instruction-tuning data across 8 weighted categories and fine-tuned CodeLlama-7B via QLoRA, achieving 84/100 output quality. The dataset sizing analysis confirmed that category weighting matters more than volume — the same insight that applies to optimizing training data for agentic task performance.

I build production LLM systems too. My RAG study assistant runs a 10-stage pipeline — screen capture through Claude API answer in under 4 seconds — with an 8-agent dependency graph, typed I/O contracts, context compression (semantic chunking, cross-encoder re-ranking, 2000-token budget trimming), and 9 typed WebSocket events for real-time agent communication. My production platform at cookbookclub.vip integrates GPT-4o at 7 touchpoints with distinct prompt strategies per feature, hash-keyed response caching, and structured output validation. These aren't demos — they're systems serving real users, with the reliability constraints that imposes.

What draws me to Anthropic specifically is the intersection of agent capability and safety. My backdoor detection work exists because I believe deployed models need governance — the same conviction that drives Anthropic's mission. I want to make Claude more capable as an agent while ensuring that capability remains interpretable and steerable. I'm pursuing my M.S. in Computer Science (Machine Learning) at Georgia Tech, I hold a Top Secret clearance, and I'm comfortable at every layer from PyTorch model internals to production infrastructure.

I would welcome the opportunity to discuss how my experience building multi-agent systems, evaluation frameworks, and agent harnesses maps to what the Agents team is working on.

Sincerely,
Nathaniel (Nate) Mauer
