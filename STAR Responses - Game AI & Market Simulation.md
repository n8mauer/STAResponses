STAR Responses: Game AI & Market Simulation

Mapped to Repository Evidence (tic-tac-toeandconnectfour, Marketsim)

---
STAR 16: Game-Playing Agent Design with Adversarial Search

JD Requirement: "Have experience developing complex agentic systems"
/ "Ideate, develop, and compare the performance of different agent
harnesses" / "Multi-agent systems" / "Design and implement rigorous
quantitative benchmarks for large scale agentic tasks"

Situation: CS 7637 Knowledge-Based AI required building game-playing
agents that could handle increasing complexity — from solved games
(Tic-Tac-Toe) to games with large branching factors (Connect 4 on
variable-sized boards), multiple opponents, and partial observability
where the agent cannot see the full board state. Each variant demanded
a different search strategy, and the agent architecture needed to
scale across all five without per-variant rewrites.

Task: Design and implement a game-playing agent framework that uses
adversarial search algorithms (minimax, alpha-beta pruning) with
heuristic evaluation to play optimally across 5 game variants of
increasing complexity — including a 3-player multiplayer mode and a
hidden multiplayer mode with partial observability.

Action:
- Implemented full minimax search for Tic-Tac-Toe (GameAgent.py lines
  41-59): exhaustive game tree traversal with depth-penalized scoring
  (+10-depth for wins, depth-10 for losses) to prefer faster wins and
  delay losses. The agent never loses — it plays optimally against any
  opponent.
- Built alpha-beta pruning for Connect 4 (GameAgent.py lines 96-133):
  depth-limited search (depth 5 for standard 6x7 boards, depth 4 for
  extended boards up to 12x14) with alpha-beta cutoffs that prune
  branches that cannot improve the current best move — reducing the
  effective search space dramatically.
- Designed a heuristic evaluation function with two components:
  `_evaluate` (lines 166-180) scores board positions by scanning
  windows across 4 directions (horizontal, vertical, both diagonals)
  and adding a center-column bonus (+3 per center piece).
  `_score_window` (lines 183-193) scores individual window patterns:
  100 for a winning sequence, 10 for one-away-from-win with an empty
  slot, 5 for two-away with two empties, -80 for opponent one-away
  threats, producing an asymmetric evaluation that prioritizes
  blocking over building.
- Supported 5 game variants with increasing complexity through a
  polymorphic `make_move()` dispatch: (1) Tic-Tac-Toe 3x3 — full
  minimax, (2) Connect 4 Basic 6x7 — alpha-beta depth 5, (3) Connect
  4 Extended (random 6-12 rows x 7-14 cols, variable sequence length)
  — alpha-beta depth 4, (4) Multiplayer (3 players on 9x10 board) —
  alpha-beta treating all non-self players as opponents, (5) Hidden
  Multiplayer (9x10, 3 players) — alpha-beta under partial
  observability.
- Implemented partial observability for Hidden Multiplayer mode
  (Connect4Game.py lines 152-154, 182-187): opponent tokens are
  dynamically replaced with 'H' placeholders before the agent sees
  the board — the agent must reason about the game state without
  knowing where opponents have played. The agent still applies
  alpha-beta search but with degraded information, making its
  heuristic evaluation critical for decision quality.
- Built a `GameAgent` vs `RandomAgent` baseline comparison framework
  with `GameStats` tracking (94 lines): win/loss/draw outcomes,
  per-game timing, and move counts — enabling quantitative benchmarks
  across agent strategies and game variants.

Result: A game-playing agent framework where a single `GameAgent`
class (193 lines) plays 5 game variants using algorithmically
appropriate search strategies — full minimax for small state spaces,
depth-limited alpha-beta with heuristic evaluation for large ones. The
Tic-Tac-Toe agent achieves perfect play (never loses). The Connect 4
agent uses asymmetric threat weighting (-80 for opponent threats vs
+10 for own opportunities) to play strong defense-first strategy. The
partial observability variant demonstrates agent reasoning under
uncertainty — a foundational capability for real-world agentic systems
where agents rarely have complete information. The architecture
directly maps to Anthropic's work on agent harness design (polymorphic
dispatch across task types), multi-agent coordination (3-player
adversarial search), and quantitative benchmarking (GameStats outcome
tracking across agent configurations).

Key files: GameAgent.py (193 lines), Connect4Game.py (282 lines),
GameDriver.py (89 lines), GameStats.py (94 lines),
docs/TIC_TAC_TOE_PRD.md (348 lines)

---
STAR 17: Quantitative Market Simulation & Evaluation Framework

JD Requirement: "Design and implement rigorous quantitative benchmarks
for large scale agentic tasks" / "Assist with automated evaluation of
Claude models and prompts across the training and product lifecycle" /
"Software engineering and ML experience"

Situation: CS 7646 ML for Trading required building a portfolio
simulation engine that could process sequential trading actions across
multiple securities, model realistic transaction costs, and produce
standardized financial metrics — then validate correctness against
known-good outputs across 12 test scenarios with different cost
models. The simulation needed to handle real market data with missing
dates, corporate actions, and multi-asset portfolios.

Task: Build a market simulation engine that computes portfolio values
from ordered trading actions with transaction cost modeling, produces
industry-standard quantitative metrics, and includes an automated
grading framework that validates simulation accuracy to 0.1% tolerance
across multiple test configurations.

Action:
- Built a portfolio simulation engine (`compute_portvals` in
  marketsim.py lines 57-122) that processes time-ordered BUY/SELL
  actions across multiple securities: reads orders from CSV, fetches
  historical price data, constructs a trades matrix with per-trade
  cost adjustments, computes cumulative holdings via running sum, and
  calculates daily portfolio value as the dot product of holdings and
  prices — with forward-fill and backward-fill to handle missing
  market dates.
- Modeled two independent transaction cost components: fixed
  commission (default $9.95 per trade, deducted from cash) and
  percentage market impact (default 0.5%, applied as price adjustment
  — BUY prices increase by impact factor, SELL prices decrease) —
  enabling isolated testing of each cost model's effect on portfolio
  performance.
- Implemented 4 quantitative evaluation metrics
  (`get_portfolio_stats`, lines 48-54): Sharpe ratio (annualized with
  sqrt(252) for daily trading frequency), cumulative return
  (final/initial - 1), average daily return, and standard deviation of
  daily returns (ddof=1 for sample std) — all benchmarked against SPY
  over the same date range for relative performance assessment.
- Built an automated grading framework (`grade_marketsim.py`, 456
  lines) using pytest with parametrized test cases: 12 test scenarios
  defined as `MarketsimTestCase` namedtuples organized into 3 groups
  — basic (9 cases, no costs), commission (1 case, $9.95 per trade),
  and impact (1 case, 0.5% market impact), plus a combined cost case.
  Each test validates: number of trading days (exact match), final
  portfolio value (0.1% relative tolerance for basic, $0.001 absolute
  for cost tests), Sharpe ratio (0.1% absolute tolerance), and average
  daily return (0.1% tolerance).
- Designed the test framework with production-grade robustness:
  10-second timeout enforcement per test, DataFrame/Series return type
  validation, NaN detection in portfolio values, dynamic module import
  for student submissions, and detailed error messaging with filtered
  stack traces — the same patterns used in automated model evaluation
  pipelines.

Result: A market simulation engine that models realistic portfolio
dynamics including transaction costs, producing 4 industry-standard
quantitative metrics benchmarked against SPY. The automated grading
framework validates correctness to 0.1% tolerance across 12 test
scenarios — a rigorous evaluation pipeline that catches both
implementation errors and numerical precision issues. The evaluation
methodology — namedtuple test case definitions, parametrized test
execution, tolerance-based assertions, multi-metric validation — maps
directly to the kind of automated evaluation framework Anthropic needs
for benchmarking agent performance: define expected outcomes as
structured test cases, run agents through parametrized scenarios,
and validate results against quantitative thresholds with appropriate
tolerances.

Key files: marketsim.py (171 lines), grade_marketsim.py (456 lines),
orders/ (12 CSV test datasets)
