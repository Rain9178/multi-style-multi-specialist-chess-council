# Training Ecosystem

> **Status: Concept / Unimplemented / Untested**
>
> This document describes the proposed offline training and population ecosystem.
>
> The project does not assume that an AlphaStar-style League is necessary for chess.
> Whether population training adds value beyond simpler self-play and checkpoint-based training is an experimental question.

---

## 1. Purpose

The training ecosystem is intended to investigate whether a deliberately diverse population can help develop or preserve:

- robust general chess policies;
- distinguishable Styles;
- genuine specialist Expertise;
- useful counter-strategies;
- historical regression resistance;
- strategically meaningful diversity.

The League is therefore not the foundation of the project.

It is a later experimental layer.

A simpler Style × Expert system should be tested before population complexity is introduced.

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
- PSRO-style population expansion.

These ideas are prior art.

This project does not claim to have invented population training or League-based reinforcement learning.

---

## 3. Project adaptation `[B+C]`

The proposed chess population may combine:

- general Main policies;
- Style-conditioned policies;
- Expert-conditioned policies;
- joint Style × Expert policies;
- targeted Exploiters;
- historical snapshots;
- strategically unusual archived policies.

The goal is not:

> create one complete agent for every possible Style × Expert coordinate.

The goal is:

> retain policies that add measurable strategic, specialist, behavioural, or regression value.

---

## 4. Why a League is not automatically necessary

Chess differs substantially from environments for which very large population systems were originally developed.

Strong chess self-play already works extremely well.

Therefore the correct research question is not:

> How do we reproduce AlphaStar in chess?

It is:

> Does a reduced population ecology provide measurable benefits for the specific Style, Expertise, robustness, or exploitability objectives of this project?

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

A Main may itself use Style × Expert conditioning.

### Exploiter

An Exploiter is trained primarily to expose weaknesses in:

- a particular Main;
- a group of policies;
- the current population.

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
training role

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

---

## 7. Training and evaluation must remain separate

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

---

## 8. Chess payoff representation

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

## 9. Color and opening control

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

## 10. Evaluator

The Evaluator produces new empirical evidence.

Possible responsibilities include:

- scheduled cross-play;
- active-policy evaluation;
- historical regression checks;
- Exploiter tests;
- Style/Expert performance tests.

Evaluation frequency need not be uniform.

Policies in the Archive may be evaluated less frequently than policies in the Active Pool.

---

## 11. Population Payoff State

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

## 12. Active Pool

The **Active Pool** contains policies currently considered useful for substantial training or evaluation attention.

Possible reasons include:

- similar competitive strength;
- current counter-strategy value;
- useful specialist capability;
- current Exploiter relevance;
- strategic uniqueness;
- re-emerging historical threat.

Active status should be evidence-driven.

---

## 13. Archive

The **Archive** stores policies that are worth retaining but not worth frequent training.

Examples include:

- historical snapshots;
- former Main policies;
- old Exploiters;
- strategically unusual policies;
- rare Style variants;
- regression-test opponents.

The key principle is:

> Saving history can be cheap.
>
> Training continuously against all history can be expensive.

Therefore:

\[
Archive\neq ActiveTrainingPool
\]

---

## 14. Archive revival

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

## 15. Matchmaking `[A+B]`

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
- diversity-aware sampling.

---

## 16. Training value is not identical to difficulty

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

## 17. Exploiter training

Exploiters may be trained against:

- one specific Main;
- recent Main snapshots;
- selected population regions;
- known specialist weaknesses.

An Exploiter's success criterion is not necessarily:

> highest global Elo.

Possible success criteria include:

- discovering a reproducible counter-strategy;
- increasing Main policy failure rate in a particular domain;
- exposing a previously hidden strategic blind spot.

---

## 18. Exploiter vs online exploitation

Two concepts must remain separate.

### League Exploiter

An offline population-training role.

### Opponent-specific exploitation

An online strategic process that attempts to exploit the currently observed opponent.

A League Exploiter may contribute learned capabilities to the online system.

But:

\[
LeagueExploiter
\neq
OnlineVulnerabilityMechanism
\]

---

## 19. Style and Expertise training before the League

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

## 20. Style-induced state distribution `[C]`

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

## 21. Expert curriculum `[A+B+C]`

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

---

## 22. Style-preservation pressure `[B+C]`

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

## 23. Style collapse monitoring

Possible indicators include:

- policy-distribution distance;
- controlled-position preference distance;
- behavioural feature distance;
- held-out Style classifier accuracy;
- Style representation drift.

No single metric is sufficient.

A Style classifier trained from the same labels used during training should not be the sole evidence of genuine diversity.

---

## 24. Non-transitive population structure

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

## 25. PSRO-inspired population expansion `[A+B]`

The Style × Expert space can generate a potentially enormous number of possible configurations.

The project should not instantiate them all.

A PSRO-inspired design principle is:

> add or preserve a policy when it contributes new strategic value to the current population.

Possible evidence includes:

- new counter-strategy;
- significant payoff change;
- unique specialist performance;
- new behavioural region;
- restored forgotten capability.

This principle is intended to constrain population growth.

It is not a requirement to implement full PSRO exactly.

---

## 26. Optional Style lineage / mutation `[C]`

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

## 27. Mutation must remain bounded

Unrestricted mutation may cause:

- identity loss;
- population explosion;
- convergence;
- unstable evaluation.

A branch should normally represent a local variation around an existing useful policy.

Large strategic changes may be better represented by a separate new policy.

---

## 28. External opponents

A future evaluation programme may include external engines or bots where technically and legally appropriate.

Two cases must be distinguished.

### Direct external opponent

A playable engine or agent participates in actual matches.

### Learned surrogate

A model attempts to reproduce behaviour inferred from historical games.

A static opponent profile is not itself a playable policy.

---

## 29. League metadata

Each population member should eventually have enough metadata for experimental interpretation.

Possible fields include:

```text
policy_id
parent_policy
training_role
style_condition
expert_condition
training_data_scope
training_step
creation_reason
active_or_archive
evaluation_summary
known_counters
known_specialist_domains
```

This helps preserve the difference between:

- what a policy was intended to be;
- what it actually became.

---

## 30. Compute discipline

Population systems can consume large amounts of compute.

Therefore the project should separately report:

- training games;
- evaluation games;
- total search nodes;
- GPU time;
- CPU time;
- population size;
- archive size;
- active-pool size.

A population method should not receive hidden compute advantages over a baseline.

---

## 31. Mini-League first

If the project reaches League training, the first experiment should remain deliberately small.

A conceptual example might contain:

```text
small number of Main policies
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

## 32. Stage gates

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

### Gate 5 — Scale

Only after measurable benefit should the population expand.

---

## 33. League success criteria

A League is useful only if it produces measurable improvement in one or more intended objectives, such as:

- robustness;
- specialist capability;
- strategic diversity;
- resistance to historical regression;
- useful counter-strategy coverage.

Higher compute consumption is not itself success.

---

## 34. League failure conditions

The League branch should be reduced or abandoned if:

- ordinary self-play performs equally well;
- Style diversity collapses;
- Specialists remain fake labels;
- Exploiters add no measurable pressure;
- archived policies add no regression value;
- population expansion produces redundant policies;
- training costs grow much faster than useful coverage;
- diversity exists only because policies are made objectively weaker.

The League is therefore:

> **optional infrastructure whose value must be demonstrated, not assumed.**