# Contributing

> **Repository status: Architecture Proposal / Unimplemented / Untested**
>
> Contributions are welcome even if they do not implement the complete architecture.

---

## 1. Contribution philosophy

The goal of this repository is not to protect a fixed architecture from criticism.

The goal is to determine:

> which ideas are useful, which are unnecessary, and which are wrong.

A contribution that removes an unnecessary component may be as valuable as one that adds a new feature.

---

## 2. Useful contribution types

Examples include:

- architectural criticism;
- literature references;
- prior-art corrections;
- alternative designs;
- simplifications;
- partial prototypes;
- full independent implementations;
- benchmark suites;
- specialist-position datasets;
- negative experimental results;
- ablation studies;
- reproduction attempts;
- chess-engine engineering feedback;
- statistical-methodology corrections;
- documentation improvements.

---

## 3. You do not need to implement everything

The architecture is deliberately decomposable.

A contributor may implement only:

- Style;
- Expertise;
- Quality Gate;
- Router;
- Mini-League;
- Recognizer;
- Shadow Ensemble;
- vulnerability modelling;
- PlanState;
- one experimental baseline;
- one adaptation mode;
- one Archive experiment;
- one training-request experiment.

A partial implementation is useful if it produces interpretable evidence.

---

## 4. Independent implementations are welcome

An implementation does not need to become the official or canonical implementation of this repository.

Contributors may:

- fork the architecture;
- use another chess engine;
- replace modules;
- simplify the system;
- change training methods;
- test only one hypothesis;
- build a substantially modified architecture.

The architecture documentation is currently licensed under **CC BY 4.0**, subject to its attribution requirements.

Third-party engines, models, datasets, libraries, and future source code may be governed by separate licenses.

---

## 5. GitHub Discussions categories

The repository currently uses the following Discussions categories.

### Announcements

For:

- repository announcements;
- major documentation updates;
- project-status changes;
- important maintainer notices.

### Architecture Ideas

For:

- alternative architectures;
- proposed simplifications;
- module changes;
- conceptual criticism.

### Q&A

For:

- terminology;
- architecture intent;
- clarification;
- implementation questions.

### Experiments & Results

For:

- benchmarks;
- ablations;
- negative results;
- failed experiments;
- reproducibility reports.

### Implementations

For:

- forks;
- external repositories;
- partial prototypes;
- complete independent implementations.

---

## 6. Negative results are welcome

Examples of valuable findings include:

```text
"Style × Expert performs no better than a generic conditional model."
```

```text
"The League adds no measurable robustness."
```

```text
"Opponent prediction improves but does not improve match score."
```

```text
"The Dynamic Vulnerability Graph performs no better than a static weakness table."
```

```text
"Triggered adaptation consumes substantial compute but provides no measurable late-match improvement."
```

Such results directly improve the architecture.

---

## 7. Simplification is explicitly encouraged

A contribution showing:

> Component X is unnecessary.

is not hostile to the project.

It is a successful scientific contribution.

The architecture should become smaller when evidence permits.

---

## 8. Contribution attribution categories

Future work should clearly distinguish among:

### Original Architecture Proposal

The initial architecture described by this repository.

### Engineering Implementation

Code that turns part or all of the proposal into a working system.

### Experimental Contribution

Data, benchmarks, evaluations, or ablations.

### Architectural Modification

A later design change or extension.

### Independent Reproduction

An independently implemented attempt to reproduce a reported result.

These categories may overlap.

---

## 9. Credit should follow actual contribution

The original architecture proposal should not receive credit for:

- code it did not implement;
- experiments it did not run;
- improvements invented later by others.

Likewise, later implementations should preserve appropriate attribution to:

- prior architecture;
- prior research;
- third-party software.

---

## 10. Architecture discussions

When proposing a new mechanism, please try to answer:

1. What problem does it solve?
2. Which existing component currently handles that problem?
3. Is the new mechanism actually distinct?
4. What are its inputs?
5. What are its outputs?
6. What baseline should it beat?
7. What ablation would test it?
8. What result would cause us to remove it?

This helps prevent architecture bloat.

A proposed mechanism should not become a permanent architectural component merely because it sounds plausible.

---

## 11. Prior-art contributions

Corrections to research provenance are especially welcome.

Useful contributions include:

- identifying earlier related work;
- showing that a supposedly project-specific mechanism already exists;
- correcting an inaccurate description of AlphaStar, AlphaZero, Lc0, Stockfish, PSRO, MoE, opponent modelling, continual learning, or related methods;
- identifying stronger existing baselines.

The repository should prefer accurate attribution over novelty theatre.

---

## 12. Implementation reports

A useful implementation report should specify:

```text
repository / commit
implemented components
base engine
engine version
network version
hardware
operating system
search settings
training method
known missing components
known deviations from proposal
```

For adaptive implementations, also specify where applicable:

```text
adaptation mode
persistent-state behaviour
within-game adaptation
between-game adaptation
training-side components
archive implementation
```

A modified implementation should state clearly where it differs from the original proposal.

---

## 13. Experimental reports

A useful experiment report should include, where applicable:

- implementation commit;
- engine/network version;
- hardware;
- compute budget;
- node or time limits;
- opening suite;
- color protocol;
- opponent pool;
- baseline;
- ablations;
- seeds;
- game count;
- metrics;
- uncertainty estimates;
- positive results;
- negative results;
- known limitations.

For experiments involving adaptation or persistent opponent modelling, reports should also specify:

- adaptation mode;
- whether model weights were frozen or mutable;
- whether within-game adaptation was permitted;
- whether between-game adaptation was permitted;
- whether opponent-specific state persisted across games;
- whether external or distributed compute was used;
- adaptation or training compute budget;
- adaptation or training wall-clock budget;
- whether the opponent was itself allowed to adapt.

These details are necessary to distinguish:

> static-engine strength

from:

> adaptive-system performance.

---

## 14. Static and adaptive results should not be mixed

A static match and an adaptive match answer different questions.

A static protocol may require:

```text
fixed weights
fixed state
fixed compute
```

An adaptive protocol may intentionally allow:

```text
persistent opponent state
policy adaptation
targeted training
distributed training
```

Both may be scientifically useful.

However, their results should not be presented as if they measure the same system property.

When applicable, use explicit labels such as:

```text
OFF
OBSERVE
ADAPT
```

or an equivalent clearly documented scheme.

---

## 15. Avoid result-only reporting

Please avoid reporting only:

> “Our version won 60%.”

without describing:

- sample size;
- opponents;
- colors;
- openings;
- compute;
- statistical uncertainty;
- adaptation conditions, where applicable.

Chess experiments can be highly sensitive to protocol.

Adaptive experiments can additionally be sensitive to what information or learned state is allowed to persist between games.

---

## 16. Reproducibility

Where practical, experimental contributions should provide:

- configuration files;
- fixed seeds;
- opening lists;
- engine versions;
- model hashes;
- scripts;
- raw or summarized logs.

For adaptive systems, reproducibility may additionally require:

- initial policy checkpoints;
- initial opponent-model state;
- archive state or archive version;
- training-budget configuration;
- adaptation-mode configuration;
- ordering of games where encounter order matters.

The goal is to make results independently checkable.

---

## 17. Logging opponent-specific experiments

For opponent-modelling experiments, useful logs may include:

```text
real opponent moves
Recognizer beliefs
Shadow weights
Shadow predictions
Predictability
ModelTrust
Disagreement
ShiftRisk
vulnerability hypotheses
Router intervention
selected move
```

Where adaptation or additional compute is active, useful logs may also include:

```text
adaptation mode
training / adaptation request
trigger reason
compute allocation
candidate specialist / response branch
model or policy change
deployment decision
archive action
persistent opponent state
```

This allows the inference and adaptation loops to be audited.

---

## 18. Prediction is not evidence

A contributor should not use model-generated predictions as independent evidence that the generating model is correct.

For example:

```text
Recognizer hypothesis
↓
Shadow prediction
↓
prediction agrees with hypothesis
```

does not create a new real observation.

The external anchor must remain:

> real opponent behaviour.

Likewise:

```text
vulnerability hypothesis
↓
candidate specialist generated
```

does not prove the vulnerability.

Training output must not validate the premise that caused the training.

---

## 19. Belief, compute, and control should remain separate

Contributions should avoid collapsing:

```text
Belief
Compute Allocation
Control Commitment
```

into one quantity.

A low-probability hypothesis may receive extra compute because it is informative.

That extra compute must not itself increase belief.

Likewise, a high-belief hypothesis does not automatically justify strong strategic control.

---

## 20. Training requests are not deployment decisions

A `TRAIN_REQUEST` or equivalent message should mean only:

> this pattern may justify further training-side investigation.

It should not automatically imply:

- the hypothesis is true;
- training must occur;
- large compute must be allocated;
- a new specialist should be deployed;
- an existing Main should be overwritten.

Useful implementations should preserve a separation such as:

```text
Discovery
↓
Request
↓
Independent evaluation
↓
Optional training
↓
Independent validation
↓
Optional deployment
```

---

## 21. Ablations are strongly encouraged

A new component is more convincing when tested against:

```text
full system
vs
same system without component X
```

Useful ablations include:

- no Quality Gate;
- no PlanState;
- no Challenger;
- no Sentinel;
- no League;
- no vulnerability graph;
- fixed intervention;
- dynamic intervention;
- no active probing;
- no persistent opponent state;
- no triggered training;
- fixed compute allocation;
- event-triggered compute allocation.

Additional mechanisms should receive their own ablations if and when they become part of an implemented system.

---

## 22. Baselines matter

Please compare new mechanisms against strong and simple alternatives.

Depending on the experiment, useful baselines may include:

- Stockfish;
- Lc0;
- random near-optimal reranking;
- Style-only;
- Expert-only;
- parameter-matched generic model;
- ordinary self-play;
- static opponent profiles;
- the same system with adaptation disabled;
- the same system without persistent opponent-specific state.

A complex adaptive system should not receive credit for gains that a simpler frozen baseline can reproduce.

---

## 23. Compute fairness

A more complex system should not silently receive more:

- search nodes;
- wall-clock time;
- GPU time;
- training games;
- distributed compute;
- model capacity;

than its baseline.

Unequal-compute experiments may still be useful.

However, the unequal compute must be reported explicitly.

For adaptive systems, useful reporting may include:

```text
within-game adaptation compute
between-game adaptation compute
distributed / external compute
training wall-clock time
policy-update count
archive size
```

Compute is part of the experimental condition.

---

## 24. Archive contributions

The project distinguishes several forms of memory.

Examples include:

### Shadow Archive

Stores inactive opponent hypotheses.

### Encounter Memory

Stores selected summaries from real encounters.

### League / Training Archive

Stores historical policies, Exploiters, representative opponents, or other training-relevant policies.

Contributions should avoid treating these as one undifferentiated storage system.

A useful Archive proposal should explain:

1. what is stored;
2. why it is stored;
3. how long it is retained;
4. how it is retrieved;
5. how redundancy is controlled;
6. what experiment demonstrates its value.

Permanent storage of every encountered opponent is not a requirement.

---

## 25. Population and Exploiter contributions

An Exploiter should demonstrate more than unusual behaviour.

A useful Exploiter should ideally:

- expose a reproducible weakness;
- produce useful training pressure;
- survive independent evaluation;
- avoid being merely a weak random policy.

Population expansion should not occur automatically merely because a new policy can be created.

Possible criteria may include:

- strategic novelty;
- exploit value;
- regression value;
- coverage value;
- transfer value.

The exact criterion remains experimental.

---

## 26. Event-triggered computation

A future system may allocate additional compute after:

- major evaluation shock;
- tactical refutation;
- severe prediction failure;
- major Shadow disagreement;
- Plan invalidation;
- unfamiliar strategic transition.

Contributions should not assume that such an event requires a fixed amount of additional thinking time.

The intended principle is:

> additional computation should continue only while its expected marginal value justifies the cost.

A useful experiment should compare event-triggered allocation against a simpler fixed-compute baseline.

---

## 27. Do not claim novelty casually

A new repository contribution should not use phrases such as:

> “first ever”

unless a serious literature review supports the claim.

Prefer:

> “we propose”

or:

> “we did not find an equivalent mechanism in the sources we reviewed.”

---

## 28. Do not claim effectiveness without experiments

Documentation should not convert:

```text
proposed mechanism
```

into:

```text
effective mechanism
```

unless controlled evidence exists.

Likewise:

```text
plausible adaptation mechanism
```

does not imply:

```text
proven strength improvement
```

---

## 29. AI-assisted contributions

AI-assisted:

- coding;
- literature organization;
- documentation;
- experiment design;

are acceptable.

However, contributors remain responsible for:

- technical correctness;
- licensing;
- attribution;
- reproducibility;
- scientific claims.

AI-generated text or code is not evidence of correctness.

---

## 30. Online competitive-use boundary

The intended project scope includes:

- offline research;
- engine-vs-engine play;
- local experimentation;
- controlled tournaments;
- analysis of recorded games.

Unauthorized real-time assistance to a human during competitive online chess is outside the intended project scope.

Adaptive engine-vs-engine research should follow the applicable rules of the experimental platform or tournament.

---

## 31. Third-party software

This repository's documentation license does not override licenses belonging to:

- Lc0;
- Stockfish;
- other chess engines;
- machine-learning libraries;
- datasets;
- third-party code.

Contributors are responsible for complying with all relevant dependency licenses.

---

## 32. Documentation license

The architecture documentation in this repository is licensed under:

> **Creative Commons Attribution 4.0 International — CC BY 4.0**

The license permits, subject to its terms:

- copying;
- modification;
- redistribution;
- research use;
- commercial use;

while retaining an attribution requirement.

The repository `LICENSE` file provides the governing documentation-license notice.

---

## 33. Future source-code licensing

If code is later added, it may use a separate software license.

Possible permissive candidates include:

- MIT;
- Apache-2.0;

depending on the implementation and dependency situation.

No final software license is selected by this document.

The documentation license should not be assumed to govern future source code unless explicitly stated.

---

## 34. Attribution style

The preferred attribution name for the architecture proposal is:

> **Unimplemented Bishop**

A simple attribution form may be:

```text
Architecture based on the
Multi-Style Multi-Specialist Chess Council proposal
by Unimplemented Bishop.
```

Equivalent attribution that satisfies the applicable CC BY 4.0 requirements is also acceptable.

---

## 35. Modifications should be identified clearly

If an implementation materially changes the architecture, it is helpful to say so.

For example:

```text
Based on the original architecture proposal,
with the following major modifications:
- no League;
- different opponent model;
- Stockfish instead of Lc0;
- static weakness model.
```

This makes comparisons easier.

---

## 36. Independent forks

Independent forks are welcome.

A fork does not need approval merely because it:

- removes modules;
- changes architecture;
- changes research direction;
- targets commercial use;
- focuses on only one experimental question.

The documentation license governs reuse of the architecture documentation.

Any source code, models, datasets, or third-party dependencies remain subject to their own applicable licenses.

---

## 37. Commercial implementations

The CC BY 4.0 documentation license permits commercial reuse of the architecture documentation subject to its terms and required attribution.

Commercial implementers remain separately responsible for:

- third-party licenses;
- engine licenses;
- model licenses;
- datasets;
- platform rules;
- any future source-code license.

---

## 38. Discussion conduct

Technical disagreement is encouraged.

Useful criticism focuses on:

- evidence;
- architecture;
- implementation;
- experiments;
- prior art.

The project should prefer:

> “This mechanism is unnecessary because baseline X performs equally well.”

over:

> “This idea feels wrong.”

---

## 39. Good architectural criticism

A particularly useful critique may identify:

- duplicate module responsibility;
- hidden circular evidence;
- compute unfairness;
- undefined data flow;
- missing failure condition;
- missing baseline;
- impossible implementation assumption;
- prior-art overlap;
- unnecessary permanent state;
- uncontrolled architecture growth.

---

## 40. Good experimental criticism

Useful experimental criticism may identify:

- insufficient game count;
- color imbalance;
- opening bias;
- compute mismatch;
- training/test leakage;
- cherry-picked positions;
- poor calibration;
- missing confidence intervals;
- adaptation-budget mismatch;
- hidden cross-game state;
- unequal access to external compute;
- unfair differences in what each side is allowed to learn.

---

## 41. Versioned results

If the architecture changes significantly, experiments should identify which architecture version they tested.

A result against an older design should not automatically be treated as evidence about a later substantially changed design.

Where adaptation is involved, reports should also identify the relevant adaptation configuration.

For example:

```text
Architecture version
Adaptation mode
Archive version
Base-engine version
Training checkpoint
```

---

## 42. Reproduction before escalation

Before expanding a promising mechanism into a large system, independent reproduction is encouraged where feasible.

For example:

```text
small Style effect
↓
reproduce
↓
larger Style experiment
```

is preferable to:

```text
small Style effect
↓
immediately build entire League
```

Similarly:

```text
one successful training trigger
```

should not immediately justify:

```text
full distributed continual-learning infrastructure
```

without smaller controlled evidence.

---

## 43. Repository scope

The architecture repository itself is intended primarily for:

- design;
- research framing;
- experiment plans;
- provenance;
- discussion.

A large implementation may eventually live:

- in this repository;
- in a subdirectory;
- in a separate repository.

The architecture does not require one repository layout forever.

---

## 44. Final contribution principle

> **Implement what is testable.**
>
> **Measure what is claimed.**
>
> **Credit what actually contributed.**
>
> **Remove what the evidence does not support.**