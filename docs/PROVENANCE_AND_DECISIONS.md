# Provenance and Design Decisions

> **Purpose**
>
> This document separates:
>
> 1. prior art;
> 2. project adaptations;
> 3. project-specific hypotheses;
> 4. historical design corrections;
> 5. explicitly postponed or rejected directions.
>
> Its purpose is to prevent architectural discussion from being mistaken for experimental validation or unsupported novelty.

---

## 1. Provenance and validation are different

Two questions must remain separate.

### Provenance

> Where does an idea come from?

### Validation

> Has this project implemented and experimentally demonstrated the idea?

A technique can be established prior art while remaining completely untested inside this project.

Likewise, a project-specific mechanism can be clearly described without being validated.

---

## 2. Provenance labels

### `[A] Prior Art / Established Technique`

An existing research concept, algorithmic family, open-source system, or established engineering technique.

Examples:

- self-play;
- population training;
- Bayesian inference;
- mixture-of-experts routing;
- temporal abstraction.

---

### `[B] Project Adaptation / Composition`

An existing technique adapted, recombined, or repositioned for this architecture.

---

### `[C] Project-Specific Hypothesis`

A mechanism proposed by this project that remains unimplemented and untested.

A component may receive multiple labels.

For example:

```text
[A+B]
```

can mean:

> the general technique already exists, while this specific integration is a project adaptation.

---

## 3. Provenance labels do not establish novelty

A `[C]` label does not mean:

> nobody has ever proposed something similar.

It means only:

> within this project, this mechanism is being treated as a project-level hypothesis rather than a borrowed mechanism.

Formal academic novelty requires dedicated literature review.

---

## 4. Current validation state

At the time of the initial architecture proposal:

| Item | State |
|---|---|
| Architecture | Proposed |
| Complete implementation | None |
| Full reference engine | None |
| Trained project models | None |
| League experiment | None |
| Shadow experiment | None |
| Vulnerability experiment | None |
| Empirical validation | None |
| Superiority claim | None |

---

## 5. AlphaZero `[A]`

Relevant prior-art ideas include:

- self-play reinforcement learning;
- neural policy/value estimation;
- search guided by learned networks;
- learning chess without requiring human chess games as the primary knowledge source.

The project does not claim to improve or replace the AlphaZero paradigm.

Its question is different:

> can additional Style, Expertise, population, and opponent-specific structure provide useful capabilities beyond a strong general chess system?

---

## 6. AlphaStar League `[A]`

Relevant prior-art ideas include:

- Main Agents;
- Exploiters;
- historical/frozen policies;
- payoff estimation;
- central coordination;
- evaluator workers;
- PFSP;
- strategy / counter-strategy population dynamics.

These mechanisms are not project inventions.

The project adapts selected ideas to chess at a much smaller and more experimental scale.

---

## 7. AlphaStar does not imply that chess needs a League

The existence of AlphaStar's League does not demonstrate that a comparable League is necessary or beneficial in chess.

The League in this proposal remains optional.

Its value must be tested against strong simpler chess baselines.

---

## 8. Leela Chess Zero `[A]`

Lc0 is considered a candidate shared neural chess foundation because it provides a strong neural chess ecosystem and existing engine/search infrastructure.

The proposal does not claim ownership of:

- Lc0;
- its network architecture;
- its training system;
- its search implementation.

A future implementation must comply with applicable Lc0 licensing.

---

## 9. Stockfish `[A]`

Stockfish may be used as:

- a strong baseline;
- an external evaluator;
- an alternative candidate generator;
- an independent comparison engine.

The architecture is not intended to reproduce Stockfish.

A future implementation must separately comply with Stockfish licensing.

---

## 10. Shared representation `[A]`

Sharing general representations across related tasks is established machine-learning practice.

The project uses this principle to avoid relearning fundamental chess competence for every Style, Expert, or opponent hypothesis.

---

## 11. Parameter-efficient specialization `[A]`

Existing machine-learning families include:

- adapters;
- embeddings;
- conditional heads;
- low-parameter specialization;
- selective parameter sharing.

The project does not claim to have invented parameter-efficient specialization.

---

## 12. Project adaptation: shared chess backbone + Style/Expert conditioning `[B+C]`

The project proposes using shared chess competence together with separately represented:

- strategic preference;
- specialist competence.

A generic project representation is:

\[
\pi(m|x,\mathbf{s},\mathbf{e})
=
F(x;\mathbf{s},\mathbf{e})
\]

The particular Style/Expert factorization remains unvalidated.

---

## 13. Mixture-of-Experts and gating `[A]`

Dynamic routing among experts is established prior art.

Recent chess research has also explored learned routing among strong chess engines.

Therefore this project must not claim novelty for:

> using a Router in chess.

---

## 14. Project adaptation: strategic Router `[B+C]`

The project-specific question is whether routing can combine:

- Style;
- Expertise;
- Plan continuity;
- opponent belief;
- vulnerability hypotheses;
- dynamic intervention;

while remaining subordinate to a shared strong chess search.

That full composition remains untested.

---

## 15. Quality-constrained decision making `[A+B]`

Constrained selection and lexicographic objective design are general established ideas.

The project adapts this principle as a **Quality Gate**:

> Style or exploitation should normally operate among objectively competitive moves rather than override base chess quality.

The exact gate definition is project-specific and unvalidated.

---

## 16. Style representation `[B+C]`

The candidate Style dimensions include concepts such as:

- initiative;
- risk;
- complexity;
- openness;
- closure;
- restriction;
- prophylaxis;
- counterattack;
- simplification;
- asymmetry.

This specific vector is a project representation.

It is not claimed to be a standard scientific taxonomy.

---

## 17. Expertise representation `[B+C]`

Candidate Expertise domains include:

- kingside attack;
- defence;
- tactics;
- closed centres;
- open centres;
- rook endings;
- minor-piece endings;
- imbalanced material;
- pawn structure;
- conversion.

This representation is also provisional.

---

## 18. Style × Expertise factorization `[B+C]`

The project originally used an intuitive two-axis model:

\[
P=(x_{\text{style}},y_{\text{expert}})
\]

This later became:

\[
P=(\mathbf{s},\mathbf{e})
\]

The current interpretation is:

> conceptually distinct factors with allowed interaction.

The project no longer assumes mathematical orthogonality.

---

## 19. Style-induced state distribution `[B+C]`

The project hypothesizes:

\[
Style
\rightarrow
StateDistribution
\rightarrow
Experience
\rightarrow
Expertise
\]

The general idea that policy affects encountered state distributions is not new.

Its use as a mechanism linking proposed chess Style and Expertise is a project hypothesis.

---

## 20. Temporal abstraction / Options `[A]`

Temporally extended actions and Options are established reinforcement-learning concepts.

Therefore the project's lightweight Plan should not be presented as a new general theory of planning.

---

## 21. Project adaptation: PlanState `[A+B]`

The project adapts temporal persistence into a lightweight chess control state containing concepts such as:

- target;
- active Style/Expert mixture;
- minimum tenure;
- termination;
- break conditions.

The Plan is not a separate full AI.

---

## 22. Hysteresis and adaptive control `[A]`

Hysteresis and asymmetric control thresholds are established engineering/control ideas.

The project adapts them to:

- Router intervention;
- Plan persistence;
- Shadow activation;
- vulnerability commitment.

---

## 23. Project adaptation: asymmetric strategic commitment `[B+C]`

The project repeatedly applies the principle:

> build strategic commitment cautiously;
> reduce commitment faster under strong counter-evidence.

This is a control design principle, not a novel general theory of inference.

---

## 24. Opponent modelling `[A]`

Opponent modelling is a broad established research area.

Relevant families include:

- behavioural modelling;
- Bayesian opponent inference;
- online adaptation;
- inverse planning;
- opponent-type inference.

The project does not claim to have invented opponent modelling.

---

## 25. Human chess move prediction `[A]`

Existing research has explicitly modelled human chess move selection.

This is an important baseline if the proposed Shadow system is ever evaluated against human opponents.

A strong superhuman chess engine should not automatically be assumed to be the best model of human behaviour.

---

## 26. Inverse planning `[A]`

Inferring hidden preferences or objectives from observed actions is established prior art.

The project may use inverse-planning ideas to interpret opponent choices among competitive moves.

---

## 27. Bayesian updating `[A]`

Bayesian belief updating is established prior art.

The project may use Bayesian or likelihood-based inference to reweight opponent hypotheses.

However, correlated Shadows may require calibration beyond naive independent likelihood multiplication.

---

## 28. Information gain `[A]`

Information gain and active information gathering are established concepts.

The project may use information value to:

- weight opponent evidence;
- choose Challenger hypotheses;
- optionally break ties among near-optimal moves.

---

## 29. Opponent weakness modelling `[A]`

Research has previously studied learning and exploiting opponent weaknesses rather than fully reconstructing a perfect opponent model.

Therefore:

> modelling where an opponent performs poorly

is prior art.

---

## 30. Relative weakness `[A]`

Existing work also motivates the idea that an opponent weakness matters relative to the exploiting agent's own capabilities.

The project should not claim ownership of the basic relative-weakness concept.

---

## 31. Project adaptation: Dynamic Vulnerability Hypothesis Graph `[B+C]`

The project extends weakness modelling into a dynamic uncertain graph.

The proposed structure includes transitions such as:

\[
W_1
\rightarrow
OpponentRepair
\rightarrow
ResourceReallocation
\rightarrow
W_2
\]

The general concept of opponent adaptation is not new.

The specific proposed chess integration remains an untested project mechanism.

---

## 32. PSRO `[A]`

Policy-Space Response Oracles are established multi-agent/game-theoretic prior art.

The project does not propose a separate full PSRO subsystem.

---

## 33. Project adaptation: value-based population expansion `[A+B]`

The project borrows the PSRO-like principle:

> do not add a population member merely because a coordinate exists;
> add one when it contributes useful strategic coverage.

This is used as a population-management principle inside the broader League concept.

---

## 34. Recognizer `[A+B+C]`

Opponent recognition itself is prior art.

The project-specific design gives the Recognizer the role:

> explain real observed past behaviour probabilistically.

It should not generate synthetic evidence.

---

## 35. Shadow Ensemble `[B+C]`

The project proposes multiple competing opponent hypotheses that predict future behaviour.

The shared-backbone conceptual shorthand was originally:

\[
Shadow_i
=
B
+
z_i
+
A_i
\]

The current technical interpretation is better represented as:

\[
p_i(m_{t+1}|h_t)
=
G_{\theta_B,\phi_i}(h_t;z_i)
\]

The earlier additive notation is only conceptual.

---

## 36. Recognizer × Shadow cross-correction `[B+C]`

The project-specific division is:

```text
Recognizer:
explain the real past

Shadows:
predict the real future

Reality:
correct both
```

A critical rule is:

> model-generated predictions do not become evidence until a real opponent action is observed.

This integration remains untested.

---

## 37. Focus / Challenger / Sentinel `[C]`

The project proposes three Shadow-resource roles:

- Focus;
- Challenger;
- Sentinel.

These are resource-allocation roles rather than separate required network architectures.

They remain project hypotheses.

---

## 38. Belief vs compute `[B+C]`

The project explicitly separates:

\[
p_i
=
\text{belief}
\]

from:

\[
q_i
=
\text{compute allocation}
\]

This prevents additional compute from being mistaken for increased posterior probability.

The general exploration/exploitation principle is established.

The specific Shadow-control formulation is project-specific.

---

## 39. Predictability vs ModelTrust `[B+C]`

The project originally treated opponent predictability as one broad control signal.

This was later refined.

The current distinction is:

```text
Predictability:
How forecastable does the opponent appear?

ModelTrust:
How much should we trust our current model?
```

These are related but not identical.

---

## 40. OpponentModelState `[B+C]`

A lightweight project representation is:

\[
M_t=(P_t,T_t,D_t,R_t)
\]

where:

- \(P_t\) = Predictability;
- \(T_t\) = ModelTrust;
- \(D_t\) = disagreement;
- \(R_t\) = Mode-shift risk.

The exact representation remains open.

---

## 41. Bounded online Shadow adaptation `[C]`

The project proposes allowing limited local adaptation of Shadow-specific state.

Conceptually:

\[
\phi_i(t)
=
\phi_i^0+\delta_i(t)
\]

subject to a bounded local region.

The goal is:

> local refinement without identity collapse.

This remains untested.

---

## 42. Active probing / dual-control principle `[A+B+C]`

Gathering information while acting is established prior art.

The project adapts this idea narrowly:

> only among already quality-qualified chess moves, optionally prefer a move with higher opponent-discrimination value.

This mechanism is explicitly optional.

---

# Historical Design Corrections

---

## 43. Correction: Style and Expertise as orthogonal axes

### Original idea

Style and Expertise were described as orthogonal axes.

### Problem

They may interact causally and statistically.

### Current position

They are:

> conceptually distinct factors with allowed interaction.

---

## 44. Correction: many complete Style × Expert networks

### Original idea

A large set of separate full models.

### Problem

- duplicated chess knowledge;
- combinatorial cost;
- difficult training.

### Current position

Prefer:

```text
shared chess backbone
+
conditional specialization
```

unless experiments show stronger separation is necessary.

---

## 45. Correction: simple majority voting

### Original idea

Multiple personalities or Experts vote on moves.

### Problem

- majority does not imply specialist correctness;
- errors may be correlated;
- continuity is poor.

### Current position

Equal voting may remain a baseline.

It is not the proposed final controller.

---

## 46. Correction: full additional coach engine

### Original idea

Use another complete AlphaZero-like controller above the other agents.

### Problem

- expensive;
- duplicates chess intelligence.

### Current position

Use a lightweight Router / meta-controller first.

---

## 47. Correction: Style reward without sufficient constraint

### Original idea

Use:

\[
R
=
R_{\text{outcome}}
+
\lambda R_{\text{style}}
\]

as the main conceptual description.

### Problem

An unconstrained Style reward may create objectively poor caricature moves.

### Current position

Prefer the conceptual ordering:

```text
Quality constraint
↓
Style preference
```

Training may still use auxiliary losses, but Style should not freely override chess quality.

---

## 48. Correction: hard Evidence Gate

### Original idea

Forced move:

> little evidence.

Many alternatives:

> more evidence.

### Problem

Candidate count alone is insufficient.

### Current position

Use continuous discriminative evidence based on:

- candidate quality;
- hypothesis disagreement;
- observed choice;
- forcedness.

---

## 49. Correction: Plan as a major AI module

### Original idea

Plan risked becoming another large intelligent component.

### Problem

It duplicates:

- Router;
- search;
- existing temporal control concepts.

### Current position

Use lightweight structured PlanState.

---

## 50. Correction: static weakness map

### Original idea

Store a list of opponent weaknesses.

### Problem

A static list cannot represent:

- context;
- repair;
- resource movement;
- new downstream weaknesses.

### Current position

First test a static weakness profile.

Only if useful, test a Dynamic Vulnerability Hypothesis Graph.

---

## 51. Correction: Shadow copies of one engine

### Original idea

Multiple Shadows could simply reuse the same strong chess system.

### Problem

Identical conditioning creates redundant predictions.

### Current position

Shadows need meaningfully different hypotheses or specializations.

---

## 52. Correction: Recognizer controls all Shadow generation

### Original idea

The Recognizer determines which Shadows exist.

### Problem

An early Recognizer error can dominate the entire hypothesis family.

### Current position

Use:

- Focus;
- Challenger;
- Sentinel;

with some coverage outside the dominant belief region.

---

## 53. Correction: fixed inverse Shadow allocation

### Original idea

If Recognizer strongly supports A, deliberately give more Shadow compute to B/C.

### Problem

This may overcorrect after A becomes reliably validated.

### Current position

Use dynamic Focus–Challenge balance.

---

## 54. Correction: compute as evidence

### Invalid implication

More compute assigned to a Shadow makes it more credible.

### Current position

\[
Belief\neq Compute
\]

Compute can produce new predictions or evidence.

It is not evidence itself.

---

## 55. Correction: Predictability as universal confidence

### Original idea

One Predictability number controls most online adaptation.

### Problem

Opponent predictability and model correctness can diverge.

### Current position

Separate:

- Predictability;
- ModelTrust;
- Disagreement;
- ShiftRisk.

---

## 56. Correction: hysteresis on statistical belief

### Problem

Artificially delaying belief updates would distort inference.

### Current position

Beliefs should respond to evidence.

Hysteresis applies mainly to:

- control commitment;
- intervention;
- switching;
- resource allocation.

---

## 57. Correction: unlimited Shadow adaptation

### Problem

A Shadow may change identity completely.

### Current position

Use bounded local adaptation.

Large behavioural changes should trigger:

- new Shadow;
- alternative hypothesis;
- Archive revival.

---

## 58. Correction: League Agent and Shadow as the same thing

### Problem

Their roles are different.

### Current position

```text
League Agent
=
offline training / evaluation policy

Shadow
=
online opponent hypothesis
```

Learned resources may be shared, but the roles remain separate.

---

## 59. Correction: Exploiter and vulnerability exploitation as the same thing

### Current distinction

```text
League Exploiter
=
training role

Opponent-specific exploitation
=
online strategic process
```

They may interact but are not identical.

---

## 60. Correction: payoff as win probability only

### Original simplification

\[
P(i\text{ beats }j)
\]

### Problem

Chess contains draws and color effects.

### Current position

Prefer:

\[
S_{ij}
=
P(W)+0.5P(D)
\]

and retain full:

\[
(W,D,L)
\]

where practical.

---

## 61. Correction: archived policy automatically becomes relevant

### Invalid logic

The Matchmaker somehow knows that an old policy is useful again.

### Current position

```text
Evaluation
↓
new payoff evidence
↓
Matchmaker
↓
possible reactivation
```

---

## 62. Correction: Style mutation as an AlphaStar mechanism

### Problem

This overstates provenance.

### Current position

Style-coordinate mutation is:

> a project / PBT-inspired optional extension.

It is not described as original AlphaStar functionality.

---

## 63. Correction: fixed final number of personalities

### Original intuition

Approximately ten representative personalities.

### Current position

No exact final number is architectural.

Population size and active strategic configurations should be evidence-driven.

---

# Explicitly Postponed or Rejected Directions

---

## 64. Hundreds of complete independent networks

Not part of the current core architecture.

---

## 65. Per-move equal voting

Retained only as a possible baseline.

---

## 66. Full second chess engine inside Router

Not recommended as the initial architecture.

---

## 67. Separate Predictability Agent

Rejected as unnecessary module proliferation.

Predictability is a derived signal.

---

## 68. Separate large Plan AI

Not part of the current core architecture.

Use lightweight PlanState.

---

## 69. Unlimited in-game network training

Not recommended.

Early online adaptation should remain lightweight and bounded.

---

## 70. Treating predictions as observations

Explicitly forbidden by the inference design.

---

## 71. Permanent active training against all history

Rejected because:

> preserving policies and frequently training against all policies are different costs.

---

## 72. Fixed 10×10 Style × Expert grid

Not required.

The early 3×3 scale is an experimental prototype size, not a permanent ontology.

---

## 73. Spatial Reflex Computing

Separate project concepts involving Spatial Reflex Computing are not part of this chess architecture unless independently reintroduced and justified.

They should not be silently inserted into this repository.

---

# Research Families Requiring Citation in Public Release

---

## 74. AlphaZero

Relevant for:

- self-play;
- neural chess learning;
- search.

---

## 75. AlphaStar

Relevant for:

- League training;
- Exploiters;
- historical policies;
- PFSP;
- evaluator/coordinator logic.

---

## 76. Leela Chess Zero

Relevant as:

- neural chess-engine foundation;
- training ecosystem;
- implementation substrate.

---

## 77. Stockfish

Relevant as:

- strong baseline;
- independent engine;
- evaluation reference.

---

## 78. Mixture-of-Experts / dynamic routing

Relevant for:

- expert selection;
- gating;
- dynamic allocation.

Recent direct chess-routing work should be cited before public release.

---

## 79. Parameter-efficient specialization

Relevant for:

- adapters;
- embeddings;
- conditional specialization.

---

## 80. Hierarchical reinforcement learning

Relevant for:

- meta-control;
- higher-level decision structure.

---

## 81. Options / temporal abstraction

Relevant for:

- persistent multi-step PlanState.

---

## 82. Opponent modelling

Relevant for:

- behavioural inference;
- online adaptation;
- opponent type modelling.

---

## 83. Bayesian inference

Relevant for:

- hypothesis reweighting;
- uncertainty.

---

## 84. Inverse planning / inverse decision modelling

Relevant for:

- inferring preferences from observed choices.

---

## 85. Human chess move prediction

Relevant as a behavioural baseline, particularly for human-opponent experiments.

---

## 86. Opponent weakness and relative weakness modelling

Relevant for:

- selective opponent modelling;
- exploitability;
- relative weakness.

---

## 87. PSRO and population game theory

Relevant for:

- population expansion;
- response policies;
- strategic coverage.

---

## 88. Active information gathering / dual control

Relevant for:

- optional active probing;
- information-sensitive action choice.

---

# Claim Boundary

---

## 89. Preferred language

Until empirical evidence exists, documentation should use phrases such as:

- proposes;
- hypothesizes;
- adapts;
- combines;
- may;
- could;
- candidate mechanism;
- experimental question.

---

## 90. Language to avoid without evidence

Avoid:

- proves;
- guarantees;
- solves;
- achieves superior performance;
- is state-of-the-art;
- is the first;
- demonstrates effectiveness.

---

## 91. AI-assisted origin

The architecture developed through extended human–AI discussion, criticism, comparison, and iterative redesign.

This fact should remain transparent.

AI participation does not convert:

- ideas into implementation;
- architecture into experiments;
- speculation into evidence.

---

## 92. Attribution of future work

Future contributions should distinguish:

- original architecture proposal;
- engineering implementation;
- experimental contribution;
- later architectural modification;
- independent reproduction.

The original proposal should not receive credit for later implementation or experiments it did not perform.

Likewise, later work should not erase the provenance of earlier architecture or prior art.

---

## 93. Current licensing direction

The current intended documentation-license direction is:

> **CC BY 4.0**

The intention is to permit:

- reuse;
- modification;
- redistribution;
- commercial use;

while requiring appropriate attribution.

The preferred attribution name is still to be selected.

A formal `LICENSE` file should be added before public release.

Future code may use a separate software license.

---

## 94. Final provenance principle

Whenever a new mechanism is proposed, ask:

1. Is this already established prior art?
2. Is it a project adaptation of prior art?
3. Is it a genuinely new project hypothesis?
4. Has it actually been implemented?
5. Has it actually been experimentally validated?
6. Does it add enough value to justify its complexity?

If these questions cannot be answered clearly, the mechanism should not silently become part of the core architecture.