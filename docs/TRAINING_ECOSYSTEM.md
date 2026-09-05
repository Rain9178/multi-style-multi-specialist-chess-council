# Training Ecosystem

> **Status: Concept / Unimplemented / Untested**
>
> This document describes the proposed offline and training-side population ecosystem.
>
> The project does not assume that an AlphaStar-style League is necessary for chess.
> Whether population training adds value beyond simpler self-play and checkpoint-based training is an experimental question.

---

## 1. Purpose

The training ecosystem is intended to investigate whether deliberately structured training can help develop or preserve:

- robust general chess policies;
- distinguishable Styles;
- genuine specialist Expertise;
- useful counter-strategies;
- historical regression resistance;
- strategically meaningful diversity;
- validated responses to reproducible system weaknesses.

The League is therefore not the foundation of the project.

It is a later experimental layer.

A simpler Style × Expert system should be tested before population complexity is introduced.

Likewise, online opponent modelling should not automatically imply expensive targeted training.

---

## 2. Prior-art boundary `[A]`

Relevant established research ideas include:

- self-play;
- population-based training;
- historical opponents;
- Main policies;
- Exploiters;
- payoff-based matchmaking;
- Prioritized Fictitious Self-Play (PFSP);
- policy populations;
- strategy / counter-strategy dynamics;
- PSRO-style population expansion;
- curriculum learning;
- continual learning;
- adversarial training;
- replay and historical-policy retention.

These ideas are prior art.

This project does not claim to have invented population training, League-based reinforcement learning, continual learning, or adversarial training.

---

## 3. Project adaptation `[B+C]`

The proposed chess training population may combine:

- general Main policies;
- Style-conditioned policies;
- Expert-conditioned policies;
- joint Style × Expert policies;
- targeted Exploiters;
- candidate specialists;
- historical snapshots;
- strategically unusual archived policies.

The goal is not:

> create one complete agent for every possible Style × Expert coordinate.

The goal is:

> retain and train policies that add measurable strategic, specialist, behavioural, robustness, exploit, or regression value.

A later extension also allows selected online discoveries to request training-side investigation.

---

## 4. Why a League is not automatically necessary

Chess differs substantially from environments for which very large population systems were originally developed.

Strong chess self-play already works extremely well.

Therefore the correct research question is not:

> How do we reproduce AlphaStar in chess?

It is:

> Does a reduced population ecology provide measurable benefits for the specific Style, Expertise, robustness, exploitability, or adaptation objectives of this project?

The League must justify:

- its compute;
- its engineering complexity;
- its population size;
- its evaluation overhead.

---

## 5. Population roles

Population roles describe **training purpose**, not personality.

### Main Policy

A Main policy attempts to maintain broad performance and robustness.

It is not required to have a neutral Style.

A Main may itself support Style × Expert conditioning.

### Exploiter

An Exploiter is trained primarily to expose weaknesses in:

- a particular Main;
- a group of policies;
- the current population;
- a reproducible system failure.

An Exploiter does not need to be globally strong against every opponent.

### Historical Policy

A frozen or archived earlier policy preserved for:

- regression testing;
- strategic coverage;
- forgotten-strategy detection;
- population diversity.

### Specialist Policy

A policy whose purpose is to demonstrate superior performance in a defined position/task distribution.

A Specialist is not automatically an Exploiter.

### Style Policy

A policy or conditional configuration used primarily to preserve or test behavioural preference.

A Style policy is not automatically a Specialist.

---

## 6. Main, Exploiter, Style, and Expert are different concepts

These labels belong to different conceptual dimensions.

```text
Main / Exploiter / Historical
        ↓
training role / policy lineage

Style
        ↓
behavioural preference

Expertise
        ↓
demonstrated competence
```

A single policy may therefore be:

```text
Main
+
high restriction Style
+
closed-centre Expertise
```

or:

```text
Exploiter
+
high complexity Style
+
tactical Expertise
```

The architecture should not collapse these meanings into one label.

Therefore:

\[
Main\neq Style
\]

and:

\[
Exploiter\neq Expert
\]

---

## 7. One Main vs multiple Main lineages `[C]`

The architecture does not require multiple Main policies.

A useful initial baseline is:

```text
one Main
+
Style × Expertise conditioning
+
Exploiters
+
historical checkpoints
```

Multiple Main lineages should be considered only if experiments show that they provide additional value.

Possible reasons might include:

- reducing shared blind spots;
- preserving genuinely different training lineages;
- improving population robustness;
- providing strategic coverage that one Main cannot represent economically.

If multiple Main policies are used, their distinction should primarily arise from:

> different policy parameters, training histories, or population lineages

rather than from assigning each Main one fixed Style × Expertise personality.

Therefore:

> **Multiple Main is optional, not an architectural requirement.**

---

## 8. Training and evaluation must remain separate

This is a core invariant.

A Matchmaker cannot know that an old policy has become dangerous without new evidence.

The required logical loop is:

```text
Evaluation Matches
        ↓
Payoff Estimation
        ↓
Population Payoff State
        ↓
Matchmaker / Sampling Policy
        ↓
Training Matches
        ↓
Policy Update
        ↓
New Evaluation Matches
```

Evaluation produces evidence.

Matchmaking consumes that evidence.

The Matchmaker should not invent it.

The same principle applies to adaptive training:

```text
TrainingRequest
≠
validation
```

and:

```text
training output
≠
proof of usefulness
```

---

## 9. Chess payoff representation

Chess contains draws.

Therefore a population payoff should not be represented only as:

\[
P(i\text{ beats }j)
\]

A minimal expected-score representation is:

\[
S_{ij}
=
P(W)
+
0.5P(D)
\]

A richer implementation should retain:

\[
(W,D,L)
\]

statistics separately.

Possible metadata should also include:

```text
number_of_games
color_balance
opening_distribution
time_or_node_budget
engine_version
confidence_interval
last_evaluation_time
```

---

## 10. Color and opening control

Chess population evaluation is highly sensitive to experimental protocol.

Where practical, evaluation should use:

- paired games;
- reversed colors;
- controlled opening suites;
- equal search budgets;
- fixed engine versions;
- reproducible conditions.

Otherwise a payoff matrix may partly measure:

- White advantage;
- opening selection;
- unequal compute;

rather than true policy interaction.

---

## 11. Evaluator

The Evaluator produces new empirical evidence.

Possible responsibilities include:

- scheduled cross-play;
- active-policy evaluation;
- historical regression checks;
- Exploiter tests;
- Style/Expert performance tests;
- targeted-specialist validation;
- transfer tests;
- unrelated-opponent regression tests;
- candidate-deployment validation.

Evaluation frequency need not be uniform.

Policies in the Archive may be evaluated less frequently than policies in the Active Pool.

The Evaluator should remain distinct from:

- the Matchmaker;
- the Router;
- the training requester.

---

## 12. Population Payoff State

The system should treat payoff values as statistical estimates rather than permanent truth.

For a population of policies:

\[
\Pi=\{\pi_1,\pi_2,\ldots,\pi_n\}
\]

the system may maintain estimated pairwise outcomes such as:

\[
S_{ij}
\]

together with uncertainty.

The matrix may expose:

- transitive strength differences;
- local counters;
- non-transitive cycles;
- historical regressions.

---

## 13. Active Pool

The **Active Pool** contains policies currently considered useful for substantial training or evaluation attention.

Possible reasons include:

- similar competitive strength;
- current counter-strategy value;
- useful specialist capability;
- current Exploiter relevance;
- strategic uniqueness;
- re-emerging historical threat;
- recently validated response value.

Active status should be evidence-driven.

---

## 14. League / Training Archive

The **League / Training Archive** stores policies that are worth retaining but not worth frequent training.

Examples include:

- historical snapshots;
- former Main policies;
- old Exploiters;
- strategically unusual policies;
- rare Style variants;
- regression-test opponents;
- validated rare attack strategies;
- representative opponent families.

The key principle is:

> Saving history can be cheap.
>
> Training continuously against all history can be expensive.

Therefore:

\[
Archive\neq ActiveTrainingPool
\]

---

## 15. Archive terminology boundary

The wider Council architecture uses several different memory systems.

They should remain separate.

### Shadow Archive

Stores inactive opponent hypotheses used by online opponent modelling.

### Encounter Memory

Stores selected summaries of real opponent encounters.

### League / Training Archive

Stores policies or representatives used for:

- training;
- regression testing;
- population management;
- historical coverage.

Therefore:

\[
\boxed{
ShadowArchive
\neq
EncounterMemory
\neq
LeagueTrainingArchive
}
\]

The Training Ecosystem primarily owns the final category.

Information may flow between them through controlled interfaces.

---

## 16. Archive revival

An archived policy should not return to high-frequency training because a coordinator “remembers” that it used to matter.

A more defensible loop is:

```text
Archive Policy
↓
Low-frequency evaluation
↓
Evidence of renewed relevance
↓
Payoff update
↓
Possible promotion to Active Pool
```

This preserves the separation between evidence and scheduling.

---

## 17. Archive admission should be value-driven `[B+C]`

The strongest opponent is not automatically the most valuable policy to preserve.

A candidate may have Archive value because it provides:

- exploit value;
- regression value;
- strategic novelty;
- coverage;
- transfer value;
- representative value;
- model-failure detection;
- rare but important counter-strategy coverage.

A conceptual relationship may be written:

\[
ArchiveValue
=
f(
ExploitValue,
Novelty,
RegressionValue,
Coverage,
TransferValue,
Repeatability,
Cost
)
\]

This is not a fixed formula.

Possible future comparisons include:

```text
No Archive
```

vs:

```text
Store everything
```

vs:

```text
Value-gated Archive
```

vs:

```text
Representative / clustered Archive
```

Archive policy should survive ablation like every other mechanism.

---

## 18. Matchmaking `[A+B]`

PFSP-inspired matchmaking is one candidate family.

The general idea is:

> allocate more training against opponents with useful current learning value.

The project should not automatically copy a historical AlphaStar sampling percentage.

Possible comparison schedulers include:

- pure self-play;
- latest-policy self-play;
- uniform historical sampling;
- near-equal-strength sampling;
- hard-opponent-biased sampling;
- payoff-based PFSP-inspired sampling;
- diversity-aware sampling;
- regression-aware sampling.

---

## 19. Training value is not identical to difficulty

A very weak opponent may provide little learning signal.

A nearly unbeatable opponent may also provide little useful short-term signal.

A near-even opponent may often be informative.

However, a strategically important Exploiter may deserve substantial training even when its match score is unusual.

Therefore:

\[
TrainingValue
\neq
f(\text{win rate only})
\]

A future Matchmaker may use several signals.

---

## 20. Predictability is not TrainingValue

Opponent-model Predictability asks:

> How forecastable does this opponent appear?

TrainingValue asks:

> How valuable would additional training on this behaviour likely be?

These are different quantities.

For example:

```text
weak opponent
+
highly random behaviour
```

may be:

```text
hard to predict
```

but still:

```text
low TrainingValue
```

Conversely:

```text
moderately strong opponent
+
reproducible structural exploit against the Council
```

may have high TrainingValue.

A conceptual decomposition may include:

\[
TrainingValue
=
f(
Strength,
Novelty,
Reproducibility,
ExploitValue,
ReusableValue,
ExpectedGain,
ComputeCost
)
\]

No final equation is fixed.

---

## 21. Exploiter training

Exploiters may be trained against:

- one specific Main;
- recent Main snapshots;
- selected population regions;
- known specialist weaknesses;
- reproducible vulnerabilities;
- Council-level failure patterns.

An Exploiter's success criterion is not necessarily:

> highest global Elo.

Possible success criteria include:

- discovering a reproducible counter-strategy;
- increasing Main policy failure rate in a particular domain;
- exposing a previously hidden strategic blind spot;
- revealing a regression;
- producing transferable training pressure.

---

## 22. Exploiter vs online exploitation

Two concepts must remain separate.

### League Exploiter

An offline or training-side population role.

### Opponent-specific exploitation

An online strategic process that attempts to exploit the currently observed opponent.

A League Exploiter may contribute learned capabilities to the online system.

But:

\[
LeagueExploiter
\neq
OnlineVulnerabilityMechanism
\]

Likewise, an online vulnerability hypothesis is not automatically an Exploiter-training target.

---

## 23. TrainingRequest interface `[B+C]`

The wider architecture may occasionally generate a:

```text
TRAIN_REQUEST
```

or equivalent message.

A request may originate from:

- opponent-modelling evidence;
- vulnerability analysis;
- Router strategic relevance;
- self-red-team discovery;
- regression detection.

A conceptual request may contain:

```text
request_id
trigger_reason
source_checkpoint
target_opponent_or_policy_family
target_vulnerability
evidence_summary
strategic_relevance
novelty
estimated_reusable_value
uncertainty
urgency
request_generation
```

A TrainingRequest means only:

> this pattern may justify further training-side investigation.

It does **not** mean:

- the hypothesis is true;
- training must occur;
- large compute must be allocated;
- a new policy should be deployed.

Therefore:

\[
Evidence
\neq
TrainingRequest
\neq
TrainingCompute
\neq
Deployment
\]

---

## 24. Training-side admission and budget decision `[B+C]`

The Router and opponent model should not own training.

A training-side admission process may decide whether a request should be:

```text
Rejected
Deferred
Evidence-Seeking
Budgeted
Accepted for Training
```

Possible inputs include:

- evidence strength;
- reproducibility;
- TrainingValue;
- existing-capability coverage;
- expected transfer;
- urgency;
- compute availability;
- request freshness;
- regression risk.

This function may be implemented through:

- rules;
- a scheduler;
- a training coordinator;
- another bounded mechanism.

It does not need to become a large intelligent Agent.

The important architectural boundary is:

> **online discovery may request training, but training-side logic decides whether training happens.**

---

## 25. Triggered Exploiter Training `[B+C]`

A later-stage adaptive path may be:

```text
real opponent / online encounter
↓
reproducible weakness or system failure
↓
TrainingRequest
↓
training-side admission
↓
targeted Exploiter / specialist branch
↓
training
↓
independent evaluation
↓
deploy / archive / reject
```

The target may be:

- one opponent;
- an opponent family;
- one Main weakness;
- a recurring structural vulnerability;
- a regression class.

Triggered training should not occur because of one lucky win or one anecdotal failure.

Useful evidence may include:

- repeated reproduction;
- transfer across related starting positions;
- independent evaluator confirmation;
- failure across more than one Council policy.

---

## 26. Candidate policy lifecycle `[B+C]`

A policy created through targeted training should move through explicit states.

For example:

```text
Requested
↓
Admitted
↓
Candidate
↓
Training
↓
Evaluation
↓
Validated
↓
Active / Archive / Reject
```

Possible additional states include:

```text
Deferred
Stale
Superseded
Regression-Failed
```

This prevents:

> “training completed”

from being confused with:

> “deployment justified.”

A candidate should not directly overwrite a stable Main.

---

## 27. Candidate validation

A targeted candidate may be evaluated on several axes.

### Target efficacy

Does it actually improve against the intended opponent or vulnerability?

### Replication

Does the effect repeat?

### Transfer

Does it help against related opponents or states?

### Regression

Does it damage unrelated capability?

### Compute efficiency

Was the gain worth the training cost?

### Robustness

Does the candidate still work when:

- openings change;
- colors reverse;
- search budget changes;
- the opponent responds differently?

Only after sufficient validation should deployment be considered.

---

## 28. Weakness Detection vs Weakness Amplification `[B+C]`

The training ecosystem should distinguish:

```text
Weakness Detection
```

from:

```text
Weakness Amplification
```

### Weakness Detection

Asks:

> Is there reliable evidence of an exploitable vulnerability?

### Weakness Amplification

Asks:

> Can targeted training make the Council substantially better at reaching or exploiting that vulnerability?

A useful decision sequence is:

```text
supported vulnerability
↓
existing capability sufficient?
├─ yes → use existing capability
└─ no / potentially valuable
       ↓
   TrainingRequest
       ↓
   targeted candidate
       ↓
   independent evaluation
```

The project should not train a new Specialist merely because a weakness exists.

Training should be justified only when the expected gain exceeds:

- objective steering cost;
- overfitting risk;
- compute cost;
- regression risk.

---

## 29. Active self-red-teaming `[A+B]`

The Council may use the population ecosystem to deliberately search for ways to break itself.

This is an extension of:

- self-play;
- Exploiter training;
- adversarial training;
- historical regression testing.

Possible targets include:

- current Main policies;
- Specialists;
- Router assumptions;
- opponent-model assumptions;
- historical blind spots;
- newly deployed adaptive capabilities.

A conceptual loop is:

```text
Current Council
↓
Exploiters / adversarial population
↓
candidate failure
↓
replication
↓
independent evaluation
↓
repair training
↓
regression testing
```

This does not require a separate `SelfRedTeam Agent`.

It is a training mode within the broader population ecosystem.

---

## 30. Self-red-team diversity

Self-red-teaming can itself collapse into a narrow internal ecology.

If all attackers share:

- backbone;
- training data;
- search;
- policy lineage;

they may share the same blind spots.

Useful diversity may therefore require combinations of:

- historical checkpoints;
- different Exploiter objectives;
- unusual Style conditions;
- distinct training lineages;
- external engines;
- out-of-distribution policies;
- strategically unusual archived opponents.

Robustness against the project's own population does not establish global robustness.

---

## 31. Style and Expertise training before the League

The project should not introduce population complexity before Style and Expertise themselves demonstrate useful signal.

A recommended logical order is:

```text
Shared Backbone
↓
Style validation
↓
Expertise validation
↓
Joint Style × Expert routing
↓
Only then: Mini-League
```

Otherwise population training may multiply poorly defined labels.

---

## 32. Style-induced state distribution `[C]`

One project hypothesis is:

\[
Style
\rightarrow
StateDistribution
\rightarrow
Experience
\rightarrow
Expertise
\]

For example:

```text
High complexity preference
↓
More dynamic tactical positions
↓
More training experience there
↓
Possible tactical specialization
```

A League may amplify this effect because different policies repeatedly create different state distributions.

This mechanism remains unverified.

---

## 33. Expert curriculum `[A+B+C]`

A specialist may require deliberate exposure to its target domain.

Candidate methods include:

- controlled position datasets;
- curriculum sampling;
- specialist self-play;
- replay of relevant positions;
- domain-weighted training;
- parameter-efficient fine-tuning.

The correct curriculum is open.

An Expert should not be declared successful merely because it received specialist training.

It must demonstrate held-out performance improvement.

The same rule applies to opponent-triggered specialists.

---

## 34. Style-preservation pressure `[B+C]`

Population competition creates a risk:

> every policy may converge toward similar objectively strong chess.

Candidate diversity-preservation mechanisms include:

- near-optimal Style preference;
- behavioural anchors;
- conditional modules;
- diversity regularization;
- preference consistency;
- controlled Style seeds.

All must be tested against the possibility that they merely force artificial weakness.

---

## 35. Style collapse monitoring

Possible indicators include:

- policy-distribution distance;
- controlled-position preference distance;
- behavioural feature distance;
- held-out Style classifier accuracy;
- Style representation drift.

No single metric is sufficient.

A Style classifier trained from the same labels used during training should not be the sole evidence of genuine diversity.

---

## 36. Non-transitive population structure

A useful population may contain relationships such as:

```text
A beats B
B beats C
C beats A
```

Such cycles can reveal strategically distinct policies that would be hidden by a single scalar rating.

The project should therefore inspect:

- pairwise payoff structure;
- local counters;
- non-transitive cycles;

rather than reducing the entire population to one Elo ordering.

---

## 37. PSRO-inspired population expansion `[A+B]`

The Style × Expert space can generate a potentially enormous number of possible configurations.

The project should not instantiate them all.

A PSRO-inspired design principle is:

> add or preserve a policy when it contributes new strategic value to the current population.

Possible evidence includes:

- new counter-strategy;
- significant payoff change;
- unique specialist performance;
- new behavioural region;
- restored forgotten capability;
- validated exploit value;
- regression value.

This principle is intended to constrain population growth.

It is not a requirement to implement full PSRO exactly.

---

## 38. Optional Style lineage / mutation `[C]`

A future League may permit useful policies to branch locally.

Example:

```text
A
├── A1: slightly more complexity
├── A2: more prophylaxis
└── A3: different Expert mixture
```

Possible mutation targets include:

- Style coordinates;
- Expert mixture;
- adapter parameters;
- Router conditioning.

This is a project-specific extension.

It should not be described as an original AlphaStar mechanism.

---

## 39. Mutation must remain bounded

Unrestricted mutation may cause:

- identity loss;
- population explosion;
- convergence;
- unstable evaluation.

A branch should normally represent a local variation around an existing useful policy.

Large strategic changes may be better represented by a separate new policy.

---

## 40. Temporary response branches `[C]`

The online architecture may optionally create temporary response branches after a major strategic surprise.

These branches are not automatically new Main policies.

If a response branch appears useful, the training ecosystem may later receive it as:

- a candidate policy;
- a TrainingRequest attachment;
- an opponent-specific specialist candidate.

Possible outcomes include:

```text
discard
archive
continue targeted training
broader evaluation
```

A temporary response branch should not bypass the normal evaluation and deployment lifecycle.

---

## 41. External opponents

A future evaluation programme may include external engines or bots where technically and legally appropriate.

Two cases must be distinguished.

### Direct external opponent

A playable engine or agent participates in actual matches.

### Learned surrogate

A model attempts to reproduce behaviour inferred from historical games.

A static opponent profile is not itself a playable policy.

External opponents are especially important for testing whether self-red-team robustness transfers beyond the project's own ecology.

---

## 42. League metadata

Each population member should eventually have enough metadata for experimental interpretation.

Possible fields include:

```text
policy_id
parent_policy
training_role
policy_lineage
style_condition
expert_condition
training_data_scope
training_step
creation_reason
source_request_id
source_checkpoint
target_opponent_or_family
target_vulnerability_version
active_or_archive
evaluation_summary
known_counters
known_specialist_domains
creation_time
last_validation_time
```

This helps preserve the difference between:

- what a policy was intended to be;
- what it actually became;
- which system state it was trained against.

---

## 43. Request and policy freshness

Asynchronous training creates a staleness problem.

A request may be generated against:

```text
Main checkpoint A
Opponent model B
Vulnerability W1
```

but training may finish after:

```text
Main → A+5
Opponent model changed
W1 disappeared or was repaired
```

Therefore requests and candidates may require bindings such as:

```text
source_checkpoint
opponent_or_model_version
vulnerability_version
request_generation
creation_time
```

Before deployment, the Evaluator should confirm that:

- the target still exists;
- the candidate still provides value;
- the candidate remains compatible with the current system.

A stale candidate may be:

- rejected;
- archived;
- retargeted;
- reevaluated.

---

## 44. Multi-timescale training boundary `[A+B+C]`

The wider architecture distinguishes several adaptation timescales.

### Fast online control

Examples:

- Router updates;
- Shadow belief updates;
- Style/Expert activation;
- PlanState;
- intervention strength.

These are not normally responsibilities of the Training Ecosystem.

### Intermediate / match-scale adaptation

Possible examples:

- bounded adapter changes;
- temporary response branches;
- current-position rollout;
- asynchronous targeted training.

The Training Ecosystem may participate where the experimental protocol permits it.

### Slow / offline training

Examples:

- Main training;
- Exploiter training;
- Specialist training;
- self-red-teaming;
- Archive consolidation;
- broad regression repair.

This is the primary responsibility of the Training Ecosystem.

The project does not require full shared-backbone retraining during every game.

---

## 45. Adaptation modes and training permissions

Experiments may distinguish:

### OFF

```text
persistent opponent-specific adaptation = disabled
targeted training = disabled
```

### OBSERVE

```text
opponent modelling may update
persistent summaries may exist
targeted playing-policy training = disabled
```

### ADAPT

```text
TrainingRequests may be admitted
targeted training may occur
validated policies may be deployed
```

The Training Ecosystem is therefore primarily relevant to the `ADAPT` condition.

The exact naming is not architectural, but equivalent distinctions should be explicit.

---

## 46. Compute discipline

Population and adaptive-training systems can consume large amounts of compute.

Therefore the project should separately report:

- training games;
- evaluation games;
- total search nodes;
- GPU time;
- CPU time;
- population size;
- archive size;
- active-pool size;
- triggered-training compute;
- between-game adaptation compute;
- distributed / external compute;
- training wall-clock time;
- number of TrainingRequests;
- number of admitted requests;
- number of deployed candidates.

A population method should not receive hidden compute advantages over a baseline.

Unequal-compute experiments may still be useful, but the inequality must be explicit.

---

## 47. TrainingValue should account for cost

A training opportunity may be strategically interesting yet economically poor.

Conceptually:

\[
NetTrainingValue
=
ExpectedCapabilityGain
-
ComputeCost
-
RegressionRisk
-
OpportunityCost
\]

This is not a prescribed optimization equation.

It states the principle that:

> additional training should be justified by expected value, not merely by novelty.

The system should prefer cheap adaptation when cheap adaptation solves the problem.

Escalation to expensive training should occur only when expected benefit warrants it.

---

## 48. Mini-League first

If the project reaches League training, the first experiment should remain deliberately small.

A conceptual example might contain:

```text
one or a small number of Main policies
+
small number of Exploiters
+
limited historical snapshots
+
one Evaluator pipeline
+
simple payoff-based Matchmaker
```

The exact counts are not architectural requirements.

The purpose is to test whether useful population dynamics appear at all.

---

## 49. Stage gates

### Gate 1 — Style

Are Style differences measurable?

If no:

> do not build a Style-based League.

### Gate 2 — Expertise

Are Experts genuinely specialized?

If no:

> do not multiply Expert policies.

### Gate 3 — Joint routing

Is Style × Expert control coherent?

If no:

> fix routing before population expansion.

### Gate 4 — Mini-League

Does a small population add robustness or diversity?

If no:

> retain simpler training.

### Gate 5 — Archive

Does historical retention provide measurable regression or coverage value?

If no:

> simplify Archive policy.

### Gate 6 — TrainingRequest

Can high-value online discoveries be identified without excessive false-positive requests?

If no:

> do not connect online discovery to expensive targeted training.

### Gate 7 — Triggered adaptation

Does targeted training outperform observation-only adaptation after accounting for compute?

If no:

> remove or simplify triggered training.

### Gate 8 — Self-red-team

Does active adversarial training expose and repair failures that simpler self-play misses?

If no:

> retain simpler population training.

### Gate 9 — Scale

Only after measurable benefit should the population or distributed training budget expand.

---

## 50. Triggered-training experiment ladder

Before large adaptive training is attempted, compare:

```text
No opponent-specific adaptation
```

vs:

```text
Observation / opponent modelling only
```

vs:

```text
Existing specialist selection
```

vs:

```text
Triggered targeted training
```

Useful measurements include:

- target-opponent improvement;
- late-match improvement;
- training cost;
- time to useful capability;
- transfer;
- regression;
- false-positive trigger rate.

This isolates whether new training actually adds value beyond better routing of existing capability.

---

## 51. Archive-selection experiment ladder

Possible comparisons include:

```text
No Archive
```

```text
Store everything
```

```text
Value-gated Archive
```

```text
Representative / clustered Archive
```

Measure:

- regression resistance;
- strategic coverage;
- Archive size;
- retrieval usefulness;
- storage cost;
- evaluation cost;
- rare-threat retention.

A more complex Archive policy should not receive automatic credit for sophistication.

---

## 52. Self-red-team experiment ladder

Compare:

```text
ordinary self-play
```

vs:

```text
Main + standard Exploiters
```

vs:

```text
active self-red-team population
```

Possible measurements include:

- newly discovered failures;
- replication rate;
- robustness after repair;
- regression elsewhere;
- external-OOD transfer;
- compute cost.

A self-red-team system that only learns to beat its own internal ecology should not be treated as globally robust.

---

## 53. League success criteria

A League or adaptive training ecosystem is useful only if it produces measurable improvement in one or more intended objectives, such as:

- robustness;
- specialist capability;
- strategic diversity;
- resistance to historical regression;
- useful counter-strategy coverage;
- reduction of repeated exploitability;
- transferable targeted capability;
- efficient recovery from discovered failures.

Higher compute consumption is not itself success.

---

## 54. TrainingRequest success criteria

A TrainingRequest mechanism is useful only if it identifies a subset of discoveries with meaningful downstream value.

Possible measurements include:

- request precision;
- request recall;
- admitted-request success rate;
- average compute per useful deployment;
- false-positive training cost;
- transfer value;
- late-match or future-match improvement.

A request system that triggers expensive training on every surprising event should be considered unsuccessful.

---

## 55. League and adaptive-training failure conditions

This branch should be reduced or abandoned if:

- ordinary self-play performs equally well;
- Style diversity collapses;
- Specialists remain fake labels;
- Exploiters add no measurable pressure;
- archived policies add no regression value;
- population expansion produces redundant policies;
- training costs grow much faster than useful coverage;
- diversity exists only because policies are made objectively weaker;
- TrainingRequests have excessive false-positive rates;
- triggered training does not outperform OBSERVE-style adaptation;
- targeted Specialists overfit one opponent without transfer;
- adaptive training causes unacceptable regression;
- self-red-teaming discovers only narrow internal tricks;
- multiple Main lineages add no value over one Main;
- Archive complexity exceeds its regression benefit;
- asynchronous candidates are frequently stale before deployment.

The League and adaptive training ecosystem are therefore:

> **optional infrastructure whose value must be demonstrated, not assumed.**

---

## 56. Final training principle

The intended training discipline is:

```text
Discover
↓
Request
↓
Evaluate whether training is worthwhile
↓
Train only when justified
↓
Validate independently
↓
Deploy / Archive / Reject
```

The system should prefer:

> **the cheapest mechanism that solves the problem.**

Training is not evidence.

Training is not deployment.

Population size is not success.

Adaptation is useful only when it produces measurable value beyond a simpler, cheaper baseline.