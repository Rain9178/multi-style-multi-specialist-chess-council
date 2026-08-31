# Experiments and Validation

> **Status: Proposed Experimental Programme / No Experiments Completed**
>
> This document defines how the architecture could be tested, falsified, simplified, or rejected.
>
> Architecture complexity is not evidence of value.

---

## 1. Scientific principle

Every major component should be testable against a simpler alternative.

The full architecture should **not** be implemented first and evaluated only as one indivisible system.

The preferred logic is:

```text
Hypothesis
↓
Minimal implementation
↓
Baseline
↓
Controlled comparison
↓
Ablation
↓
Failure analysis
↓
Keep / revise / remove
```

The project should be willing to remove mechanisms that fail.

---

## 2. What counts as success?

The project does not define success as:

> highest possible Elo.

Possible success dimensions include:

- chess strength;
- Style distinctiveness;
- specialist competence;
- strategic coherence;
- population robustness;
- opponent-prediction quality;
- opponent-specific exploitation;
- compute efficiency.

A mechanism may improve one dimension while harming another.

Those trade-offs should be reported explicitly.

---

## 3. Primary hypotheses

### H1 — Style

A Style-conditioned system can produce reproducibly different strategic preferences without unacceptable chess-strength loss.

### H2 — Expertise

Expert-conditioned policies can outperform appropriate controls on held-out specialist position distributions.

### H3 — Factorization

Style × Expertise factorization provides useful behavioural or specialist structure beyond:

- Style-only;
- Expert-only;
- parameter-matched non-factorized alternatives.

### H4 — Persistent routing

Temporal persistence improves strategic coherence relative to independent per-move routing.

### H5 — League

Population training adds measurable:

- robustness;
- specialist development;
- strategic diversity;
- regression resistance;

relative to simpler self-play or checkpoint-based training.

### H6 — Opponent modelling

Recognizer + Shadow modelling improves out-of-sample prediction of opponent behaviour.

### H7 — Cross-correction

Recognizer + Shadow performs better than:

- Recognizer-only;
- Shadow-only;
- generic prediction.

### H8 — Exploitation

Improved opponent modelling translates into measurable playing advantage against the modelled opponent.

### H9 — Dynamic vulnerability

A Dynamic Vulnerability Hypothesis Graph adds value beyond a static weakness profile.

### H10 — Adaptive trust

Predictability, ModelTrust, dynamic intervention strength, and asymmetric control hysteresis reduce damage from incorrect opponent models.

### H11 — Active probing

Information-seeking tie-breaking among already near-optimal moves provides enough additional information to justify its complexity.

All hypotheses remain untested.

---

## 4. Experiment ordering

The project should validate simple components before advanced ones.

Recommended order:

```text
Base engine
↓
Quality Gate
↓
Style
↓
Expertise
↓
Style × Expert
↓
Persistent Router
↓
Parameter-efficient learning
↓
Mini-League
↓
Opponent prediction
↓
Recognizer × Shadow
↓
Static weakness exploitation
↓
Dynamic vulnerability graph
↓
Adaptive intervention
↓
Optional active probing
```

A failed early hypothesis should block unnecessary later complexity.

---

## 5. Baseline ladder

### B0 — Strong base engine

A fixed strong engine configuration such as:

- Lc0;
- Stockfish;
- another reproducible strong engine.

This is the fundamental reference.

---

### B1 — Random near-optimal reranker

Generate a Quality-Gated candidate set and choose randomly inside it.

Purpose:

> determine whether apparent Style diversity is merely random near-optimal variation.

---

### B2 — Rule-based Style reranker

Use explicit Style scoring over Quality-Gated candidates.

No learned Style model.

Purpose:

> test whether the basic Style concept produces visible behaviour before training.

---

### B3 — Style-only conditional system

Use Style conditioning without specialist Expertise.

---

### B4 — Expert-only conditional system

Use specialist conditioning without Style.

---

### B5 — Parameter-matched generic conditional model

Use similar additional model capacity without explicit Style × Expertise factorization.

Purpose:

> determine whether improvement comes from factorization or merely additional parameters.

---

### B6 — Style × Expertise system

Joint Style and Expert conditioning without advanced population or opponent modelling.

---

### B7 — Style × Expertise + persistent Router

Add:

- minimum tenure;
- switching thresholds;
- PlanState;
- continuity.

Compare against independent per-move routing.

---

### B8 — Mini-League

Add:

- Main policies;
- Exploiters;
- historical policies;
- evaluation;
- payoff-based scheduling.

---

### B9 — Opponent model

Add:

- Recognizer;
- Shadow Ensemble;
- OpponentModelState.

Do not yet use opponent-specific exploitation.

---

### B10 — Static opponent exploitation

Use a simple static weakness profile.

---

### B11 — Dynamic opponent exploitation

Add:

- vulnerability transitions;
- dynamic Plan changes;
- adaptive intervention.

Later stages should only be built if earlier stages produce measurable signal.

---

## 6. Compute fairness

Comparisons should control compute.

A more complex system should not silently receive:

- more search nodes;
- more wall-clock time;
- more GPU time;
- larger parameter count;
- more training games;

than its baseline without reporting the difference.

At minimum, experiments should report:

```text
search nodes / time
hardware
model size
training games
evaluation games
active Shadow count
population size
memory use
```

---

## 7. Engine-match protocol

Where practical, engine matches should use:

- fixed engine versions;
- fixed neural-network weights;
- fixed hardware;
- fixed node budgets or carefully controlled time;
- paired openings;
- reversed colors;
- reproducible opening suites;
- sufficient game counts;
- confidence intervals.

A single match or short winning streak should not be treated as strong evidence.

---

## 8. Color control

Chess results are affected by color.

Where appropriate, use paired games:

```text
Game A:
Policy X = White
Policy Y = Black

Game B:
Policy Y = White
Policy X = Black
```

Prefer identical or paired opening conditions.

---

## 9. Opening control

Different opening distributions may favour different Styles or Experts.

Experiments should distinguish:

- natural self-selected openings;
- controlled opening suites;
- specialist starting positions.

The project should avoid accidentally proving:

> “Style A is stronger”

when the real result is:

> “the opening suite favoured Style A.”

---

## 10. Strength metrics

Possible strength measurements include:

- expected match score;
- local Elo estimate;
- centipawn or normalized evaluation loss;
- blunder rate;
- tactical failure rate;
- conversion rate;
- defensive survival;
- strength loss relative to the base engine.

No single metric is sufficient for every experiment.

---

## 11. Style evaluation

Style should be measured through more than one method.

Candidate approaches include:

- pre-registered controlled preference positions;
- policy-distribution differences;
- strategic-feature differences;
- blind human classification;
- held-out behaviour classifiers;
- cross-opening consistency;
- cross-opponent consistency.

---

## 12. Controlled Style positions

A Style test position should ideally contain several objectively competitive moves with different strategic characteristics.

For example:

```text
Candidate A:
open tactical continuation

Candidate B:
restriction / manoeuvring

Candidate C:
simplifying continuation
```

If all candidates are objectively competitive, repeated preferences become more informative.

Forced positions are poor Style tests.

---

## 13. Random near-optimal control

An important control is:

> random selection among near-optimal candidates.

If human observers can distinguish the proposed Styles no better than they distinguish random near-optimal variation, the Style mechanism has not demonstrated strong value.

---

## 14. Human Style evaluation

Blind human evaluation may ask reviewers to:

- classify Style;
- identify likely switching points;
- judge strategic coherence.

Reviewers should not see:

- internal labels;
- Router logs;
- intended Style identity;

before making their judgment.

---

## 15. Classifier-based Style evaluation

A held-out classifier may provide an auxiliary metric.

However:

> a classifier trained on the same labels used to construct the Style system cannot by itself validate meaningful Style.

Prefer:

- held-out data;
- independent features;
- multiple evaluation methods.

---

## 16. Style-switch evaluation

For games containing routing changes, measure whether observed behavioural changes correspond to internal switch logs.

Possible metric:

```text
human-detected transition
vs
logged Router transition
```

The objective is to test whether switching is visible in chess behaviour rather than existing only as an internal label.

---

## 17. Expertise evaluation

Expertise requires held-out target-domain testing.

Possible position suites include:

- kingside attack;
- defence;
- tactical calculation;
- closed centres;
- open centres;
- rook endings;
- minor-piece endings;
- imbalanced material;
- pawn structures;
- advantage conversion.

---

## 18. Expert controls

A specialist should be compared against:

- shared base model;
- wrong specialist;
- parameter-matched generic model;
- other specialists;
- equal search budget.

A specialist is meaningful only if it demonstrates target-domain advantage.

---

## 19. Preference–competence separation experiment

One useful experiment explicitly tests:

\[
Preference\neq Competence
\]

For example:

1. measure which positions a Style tends to enter;
2. separately measure performance inside those positions;
3. test whether entry frequency predicts actual capability.

This evaluates whether Style and Expertise provide distinct information.

---

## 20. Factorization ablation

Compare:

```text
Style-only
Expert-only
Style × Expert
Generic conditional model
```

while controlling:

- parameter count;
- compute;
- training data.

The objective is to test whether explicit factorization contributes useful structure.

---

## 21. Quality Gate experiment

Test multiple Quality Gate thresholds.

Questions include:

- Does Style disappear when the gate is too strict?
- Does strength deteriorate when the gate is too permissive?
- Does an adaptive threshold outperform a fixed threshold?
- Does the answer depend on tactical vs strategic positions?

---

## 22. Quality Gate failure analysis

A move may appear near-optimal under shallow evaluation but be strategically inferior at greater search depth.

Therefore experiments should examine sensitivity to:

- search depth;
- node budget;
- evaluation noise;
- tactical instability.

The Quality Gate is not assumed to provide perfect safety.

---

## 23. Router evaluation

Possible Router metrics include:

- switch frequency;
- average tenure;
- unnecessary switch rate;
- evaluation loss around switches;
- Plan completion;
- forced Plan termination;
- recovery after model failure;
- strategic oscillation.

---

## 24. Persistent routing ablation

Compare:

### Independent routing

Select Style/Expert configuration every move independently.

### Persistent routing

Use:

- minimum tenure;
- switching margin;
- PlanState;
- hysteresis.

The hypothesis is that persistence improves coherence without preventing necessary strategic change.

---

## 25. PlanState experiment

Compare:

- no PlanState;
- minimal PlanState;
- more complex PlanState.

Possible measurements:

- strategic oscillation;
- move quality;
- Plan duration;
- frequency of emergency break conditions.

A simpler Plan representation should be preferred if it performs equally well.

---

## 26. League baseline comparisons

A Mini-League should be compared against:

- ordinary self-play;
- latest-checkpoint self-play;
- uniform historical sampling;
- simple prioritized history sampling.

The League should not receive automatic credit merely for using multiple agents.

---

## 27. League evaluation

Possible measurements include:

- current-vs-history performance;
- Exploiter success;
- forgotten-strategy regression;
- population diversity;
- payoff non-transitivity;
- specialist development;
- total compute.

---

## 28. Population-diversity evaluation

Useful diversity should be distinguished from random weakness.

Potential signals include:

- behaviour distance;
- different specialist strengths;
- distinct counter-strategies;
- payoff cycles;
- Style preference differences.

A diverse population that is simply weaker is not necessarily useful.

---

## 29. Historical-regression experiment

Test whether a current policy has forgotten how to handle an older strategy.

Conceptual protocol:

```text
Old policy archived
↓
Current policy evolves
↓
Periodic reevaluation
↓
Regression detected?
↓
Does renewed training restore capability?
```

This tests the practical value of the Archive.

---

## 30. Exploiter experiment

An Exploiter should demonstrate more than unusual play.

Possible success criterion:

> it reliably exposes a reproducible weakness in a Main policy.

Then test:

1. Main before Exploiter exposure;
2. training against Exploiter;
3. Main after training;
4. regression against unrelated opponents.

This tests whether the Exploiter produces useful learning rather than narrow overfitting.

---

## 31. Population expansion ablation

Compare:

- fixed population;
- unconditional expansion;
- value-triggered expansion.

The question is whether PSRO-inspired value-based expansion improves strategic coverage without uncontrolled population growth.

---

## 32. Opponent-model experiments

Opponent modelling should first be tested separately from move exploitation.

Possible opponent sources include:

- synthetic controlled policies;
- known engine configurations;
- historical engine games;
- human game datasets where appropriate.

---

## 33. Synthetic-opponent tests

Synthetic opponents are especially useful because their hidden parameters can be known.

A controlled opponent may have:

- known Style mixture;
- known specialist weakness;
- scheduled Mode Shift;
- injected decision noise.

This allows direct measurement of inference quality.

---

## 34. Opponent-prediction metrics

Possible metrics include:

- negative log-likelihood;
- Brier score;
- top-k move prediction;
- calibration;
- strategic-class prediction;
- prediction improvement over time.

Top-1 accuracy alone is insufficient.

---

## 35. Recognizer / Shadow ablations

At minimum compare:

- generic prediction;
- Recognizer only;
- Shadows only;
- Recognizer + Shadows;
- no Challenger;
- no Sentinel;
- fixed Shadow allocation;
- dynamic Shadow allocation;
- no bounded adaptation;
- bounded adaptation.

---

## 36. Cross-correction experiment

A central hypothesis is that:

```text
Recognizer explains past
+
Shadows predict future
+
reality corrects both
```

outperforms one-sided inference.

Compare:

- full loop;
- Recognizer without Shadow feedback;
- Shadows without Recognizer prior;
- self-confirming invalid control as a diagnostic failure case.

---

## 37. Calibration experiment

The opponent model should not merely rank hypotheses correctly.

It should also avoid overconfidence.

For example:

> events predicted with probability near 70% should occur roughly at the expected frequency under repeated comparable tests.

Calibration is especially important because opponent-specific exploitation depends on confidence.

---

## 38. Mode-shift experiment

Use opponents that change policy during a game or match.

Measure:

- detection delay;
- false alarms;
- recovery time;
- Trait corruption;
- Router fallback.

Compare systems with and without explicit Mode / Trait separation.

---

## 39. Predictability experiment

Compare resource allocation using:

- no Predictability signal;
- raw predictive entropy only;
- full OpponentModelState.

Measure:

- prediction accuracy;
- compute efficiency;
- false confidence;
- exploitation performance.

---

## 40. Static weakness baseline

Before building a dynamic graph, test a simple weakness representation.

Example:

```text
domain
evidence
confidence
estimated opponent error rate
```

If static targeting provides no useful signal, the dynamic graph should not be built.

---

## 41. Dynamic vulnerability experiment

Compare:

1. no opponent targeting;
2. static weakness profile;
3. context-dependent relative weakness;
4. Dynamic Vulnerability Hypothesis Graph.

Measure:

- targeted match-score change;
- steering cost;
- false-positive weakness rate;
- ability to detect downstream weaknesses;
- recovery after incorrect hypotheses.

---

## 42. Injected-weakness opponents

One useful controlled experiment is to create opponents with known deliberate weaknesses.

Examples:

- reduced quality in selected endgame classes;
- predictable simplification bias;
- systematically weak defence after certain structures.

This helps test whether the architecture can discover and exploit known weaknesses before evaluating uncontrolled real opponents.

---

## 43. Repair-transition experiment

Construct situations where an opponent systematically repairs one weakness by reallocating resources.

Test whether the system can distinguish:

```text
continue attacking W1
```

from:

```text
allow repair
→ switch to W2
```

The experiment should compare the dynamic graph with a static target policy.

---

## 44. Opponent-specific intervention experiment

Compare:

- \(\alpha=0\): no opponent-specific influence;
- fixed low \(\alpha\);
- fixed high \(\alpha\);
- dynamically controlled \(\alpha_t\).

Measure:

- match score;
- strength loss;
- recovery after model failure;
- sensitivity to deceptive or changing opponents.

---

## 45. Asymmetric-hysteresis experiment

Compare:

### Symmetric control

Commitment rises and falls under similar evidence thresholds.

### Asymmetric control

Commitment builds more cautiously and falls faster under strong contradiction.

Measure:

- overreaction;
- underreaction;
- recovery delay;
- total match performance.

---

## 46. Base-search exception experiment

Compare:

- Router-only one-way control;
- Router with base-search exception feedback.

Use positions where a high-level strategic Plan is refuted tactically.

Measure whether bidirectional feedback reduces catastrophic strategic persistence.

---

## 47. Active probing experiment

Active probing should be tested only after opponent modelling already works.

Compare:

- no probing;
- information-gain tie-breaker;
- random tie-breaker among near-optimal candidates.

Measure:

- information gained;
- prediction improvement;
- eventual match benefit;
- objective chess cost.

If probing opportunities are rare or the effect is negligible, remove the mechanism.

---

## 48. Adversarial model-failure tests

The architecture should be tested against opponents that intentionally or structurally break its assumptions.

Examples:

- abrupt Mode Shift;
- deceptive early behaviour;
- highly stochastic near-optimal choices;
- opponents outside the Shadow hypothesis family;
- strategically unusual engines.

Measure:

- ModelTrust collapse;
- Challenger activation;
- fallback speed;
- damage before recovery.

---

## 49. Out-of-distribution testing

The system should not be evaluated only against agents drawn from its own training ecology.

Test against:

- unseen engines;
- different network versions;
- different playing strengths;
- unfamiliar Style distributions;
- unusual openings.

This helps distinguish genuine robustness from population overfitting.

---

## 50. Statistical reporting

Future experimental reports should include, where appropriate:

- sample size;
- confidence intervals;
- seed variation;
- opening variation;
- color control;
- multiple comparison awareness;
- effect size.

A small positive result without uncertainty should be treated cautiously.

---

## 51. Logging requirements

A future implementation should log enough information to reconstruct decisions.

Possible fields include:

```text
position
candidate moves
base evaluations
Quality Gate
Style scores
Expert scores
Router state
PlanState
OpponentBelief
OpponentModelState
active Shadows
vulnerability hypotheses
selected move
actual opponent move
```

This enables debugging and scientific audit.

---

## 52. Phase-based programme

### Phase 0 — No training

Use:

- strong UCI engine;
- MultiPV;
- rule-based Style;
- rule-based Expert relevance;
- rule-based Router;
- logs.

Goal:

> determine whether visible behavioural differentiation exists at all.

---

### Phase 1 — Learned lightweight routing

Train:

- Style scorer;
- Expert relevance model;
- small Router.

Keep the base engine fixed.

---

### Phase 2 — Parameter-efficient specialization

Test:

- embeddings;
- adapters;
- conditional heads.

Keep most general chess competence shared.

---

### Phase 3 — Mini-League

Introduce a deliberately small population.

Goal:

> test whether population dynamics add measurable value.

---

### Phase 4 — Offline opponent modelling

Replay known games.

Test:

- Recognizer;
- Shadows;
- calibration;
- Mode shifts.

Do not yet allow models to affect actual moves.

---

### Phase 5 — Controlled online adaptation

Use engine-vs-engine experiments with opponent-specific routing.

---

### Phase 6 — Vulnerability exploitation

Test:

- static weakness;
- relative weakness;
- dynamic vulnerability transitions.

---

### Phase 7 — Full ablation

Only after individual components show value should the combined architecture be evaluated.

---

## 53. Stage gates

### Gate 1 — Style

Can Style be measured?

If no:

> stop or redesign the Style branch.

### Gate 2 — Expertise

Do Experts demonstrate target-domain advantage?

If no:

> stop or redesign specialist training.

### Gate 3 — Joint factorization

Does Style × Expert add value beyond simpler controls?

If no:

> simplify.

### Gate 4 — Persistent routing

Does persistence improve coherence without unacceptable strength loss?

If no:

> use simpler routing.

### Gate 5 — League

Does population training add useful robustness or diversity?

If no:

> retain simpler training.

### Gate 6 — Opponent prediction

Does opponent modelling improve prediction?

If no:

> do not build exploitation on top of it.

### Gate 7 — Exploitation

Does better prediction improve actual chess performance?

If no:

> opponent modelling may remain an analysis tool.

### Gate 8 — Dynamic vulnerability

Does the graph outperform a static model?

If no:

> remove the graph.

### Gate 9 — Active probing

Does probing produce meaningful net value?

If no:

> remove it.

---

## 54. Project-level falsification

The architecture should be considered unsuccessful in its current form if most of the following occur:

- Style cannot be reliably distinguished;
- Style requires excessive strength loss;
- Experts are not genuinely specialized;
- factorization adds no value;
- Router intervention harms chess quality;
- League adds compute without useful robustness;
- opponent modelling does not improve prediction;
- better prediction does not improve play;
- dynamic vulnerability adds no value over static targeting;
- model-specific compute dominates any practical benefit.

---

## 55. Negative results

Negative results should be treated as valid project outcomes.

Examples:

```text
"Style works, League does not."
```

```text
"Opponent prediction improves,
but exploitation does not."
```

```text
"Static weakness modelling performs as well as the dynamic graph."
```

```text
"Strong Lc0 already dominates every opponent-specific advantage."
```

Such results would meaningfully reduce the hypothesis space.

---

## 56. Final experimental principle

> **Do not ask whether the architecture sounds intelligent.**
>
> **Ask whether each mechanism produces measurable value over a simpler baseline under controlled conditions.**