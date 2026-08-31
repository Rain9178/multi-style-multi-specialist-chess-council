# Multi-Style Multi-Specialist Chess Council

## An Open Architecture and Research Proposal

> **Status: Concept / Unimplemented / Untested**
>
> This repository currently contains an architecture and research proposal.
>
> - No complete implementation currently exists.
> - No training experiment described here has been completed.
> - No benchmark result is claimed.
> - No claim is made that this architecture outperforms Stockfish, Leela Chess Zero, or any other established chess engine.
> - No claim of academic novelty should be inferred merely because a mechanism appears in this proposal.

---

## 1. What is this project?

**Multi-Style Multi-Specialist Chess Council** is a proposed experimental architecture for chess AI.

The project asks a question that is related to, but different from, pure Elo maximization:

> Can a strong chess system preserve robust general chess competence while developing multiple distinguishable strategic styles and specialist capabilities, dynamically routing among them, modelling a particular opponent, and selectively exploiting opponent-specific weaknesses?

The project is intentionally being published as an **Architecture / Research Proposal Repository**, not as a finished engine.

Its purpose is to make the ideas concrete enough that chess-engine developers, reinforcement-learning researchers, multi-agent researchers, and interested open-source developers can independently:

- implement the full architecture;
- implement only one component;
- simplify the architecture;
- replace individual mechanisms;
- run controlled experiments;
- perform ablations;
- criticize the assumptions;
- publish positive or negative results;
- build modified systems without requesting case-by-case design approval from the original proposer.

A simpler system that disproves the need for part of this architecture would be a useful result.

---

## 2. Current project status

| Item | Status |
|---|---|
| Architecture proposal | Present |
| Reference implementation | None |
| Trained project models | None |
| Full training ecosystem | Not implemented |
| Opponent-modelling system | Not implemented |
| Experimental results | None |
| Benchmark results | None |
| Strength claim | None |
| Superiority claim | None |
| Formal novelty claim | None |

Nothing in these documents should be interpreted as evidence that the proposed system works.

---

## 3. Core research idea

The proposal separates several functions that are often collapsed into a single chess policy.

### 3.1 Base chess competence

A strong chess engine or neural chess model provides general chess competence:

- legal and tactical correctness;
- positional understanding;
- policy/value estimation;
- search;
- candidate-move quality.

The initial neural foundation considered by this proposal is **Leela Chess Zero (Lc0)**, although the architecture is not intended to depend permanently on one engine.

Stockfish or other strong engines may also be used as baselines, evaluators, or alternative components.

### 3.2 Style

Style answers:

> Among objectively competitive ways of handling a position, what kind of solution does the policy tend to prefer?

Examples might include:

- initiative;
- risk tolerance;
- complexity;
- restriction;
- prophylaxis;
- simplification;
- counterattack;
- asymmetry.

### 3.3 Expertise

Expertise answers:

> In which classes of positions or strategic tasks does the policy demonstrate superior capability?

Examples might include:

- kingside attack;
- defence;
- tactical calculation;
- closed centres;
- rook endgames;
- pawn structures;
- conversion of advantage.

Style and Expertise are conceptually separated, but they are **not assumed to be mathematically orthogonal or statistically independent**.

### 3.4 Quality-constrained routing

Style, specialization, opponent exploitation, and optional information-seeking should not freely override basic chess quality.

The proposal therefore uses a **Quality Gate**:

> higher-level preferences normally act only among moves that the base chess system considers objectively competitive.

### 3.5 Population / League training

A reduced AlphaStar-inspired population ecosystem may eventually contain:

- Main policies;
- Exploiters;
- Style- or Expert-specialized policies;
- historical policies;
- evaluators;
- payoff statistics;
- prioritized matchmaking;
- an Active Pool and Archive.

The project does **not** assume that chess needs an AlphaStar-like League.

Whether a League adds measurable value beyond simpler chess self-play is an experimental question.

### 3.6 Opponent modelling

During a game, an optional opponent-modelling layer may maintain:

- a **Recognizer**, which summarizes evidence from the opponent's real past moves;
- a **Shadow Ensemble**, which represents competing hypotheses and predicts future behaviour;
- uncertainty, disagreement, predictability, model trust, and possible strategy shifts.

Model-generated predictions are never treated as real evidence until an actual opponent move is observed.

### 3.7 Opponent-specific exploitation

If opponent-specific evidence becomes sufficiently reliable, a strategic Router may use:

- opponent hypotheses;
- specialist capabilities;
- vulnerability hypotheses;
- a lightweight multi-ply Plan state;
- base-engine search evidence;

to bias play toward potentially exploitable regions.

The base chess system retains a fallback role.

---

## 4. Three provenance labels

Every important mechanism should be interpreted using the following labels.

### `[A] Prior Art / Established Technique`

An existing research idea, algorithmic family, open-source system, or established engineering technique.

Examples include:

- self-play;
- AlphaStar-style League training;
- PFSP;
- PSRO;
- mixture-of-experts routing;
- Bayesian opponent modelling;
- temporal abstraction / Options;
- opponent weakness modelling.

`[A]` does **not** mean that the technique has been validated inside this project.

### `[B] Project Adaptation / Composition`

An existing technique adapted or combined for this proposed architecture.

Examples include:

- adapting population training to a Style × Expertise chess ecology;
- using temporal abstraction as lightweight chess Plan state;
- combining opponent beliefs with quality-constrained routing.

### `[C] Project-Specific Hypothesis`

A project-level mechanism or design hypothesis that remains unimplemented and untested.

Examples include:

- Focus / Challenger / Sentinel Shadow allocation;
- bounded online Shadow adaptation;
- repair-induced vulnerability transitions;
- the complete Recognizer × Shadow × vulnerability × Router loop.

A mechanism may carry more than one label.

These labels describe **provenance**, not academic novelty.

A future novelty claim would require a dedicated literature review.

---

## 5. Five architectural rules

The current proposal is organized around five rules.

### Rule 1 — Preference is not competence

\[
\text{Style} \neq \text{Expertise}
\]

Preferring tactical positions does not prove that an agent is tactically stronger.

### Rule 2 — Belief, compute, and control are different

\[
\text{Belief} \neq \text{Compute Allocation} \neq \text{Control Commitment}
\]

A hypothesis may be likely without receiving all compute.

A hypothesis may receive extra compute because it is informative without becoming more probable.

### Rule 3 — Prediction is not evidence until reality arrives

A Shadow's own prediction cannot validate that Shadow.

Only subsequent real opponent behaviour provides new evidence.

### Rule 4 — Opponent exploitation remains subordinate to chess quality

Opponent-specific ideas should normally operate inside quality-constrained choices and remain subject to strong chess verification.

### Rule 5 — Complexity must survive ablation

A mechanism that adds no measurable value over a simpler baseline should be removable.

---

## 6. What this project is not

This project is not:

- a completed chess engine;
- a claim of state-of-the-art Elo;
- a replacement for Stockfish or Lc0;
- a claim that multi-agent systems must beat single-agent systems;
- a historical-master imitation project;
- a requirement to train hundreds of full networks;
- a requirement to reproduce AlphaStar at full scale;
- a simple majority-voting council;
- an online chess cheating tool;
- an anti-cheat evasion system;
- a claim that the proposed opponent-exploitation mechanisms are already effective.

The word **Council** refers to the conceptual combination and routing of differentiated strategic capabilities.

It does not require ten independent full chess engines voting on every move.

---

## 7. Repository structure

```text
README.md
CONTRIBUTING.md

docs/
├── ARCHITECTURE.md
├── STYLES_AND_EXPERTS.md
├── TRAINING_ECOSYSTEM.md
├── OPPONENT_MODELING.md
├── ROUTING_AND_EXPLOITATION.md
├── EXPERIMENTS_AND_VALIDATION.md
├── RISKS_AND_OPEN_QUESTIONS.md
└── PROVENANCE_AND_DECISIONS.md