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
- one experimental baseline.

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

Future license terms will define the formal legal permissions.

---

## 5. Intended GitHub Discussions categories

The planned Discussions categories are:

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

---

## 11. Prior-art contributions

Corrections to research provenance are especially welcome.

Useful contributions include:

- identifying earlier related work;
- showing that a supposedly project-specific mechanism already exists;
- correcting an inaccurate description of AlphaStar, AlphaZero, Lc0, Stockfish, PSRO, MoE, or opponent modelling;
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

---

## 14. Avoid result-only reporting

Please avoid reporting only:

> “Our version won 60%.”

without describing:

- sample size;
- opponents;
- colors;
- openings;
- compute;
- statistical uncertainty.

Chess experiments can be highly sensitive to protocol.

---

## 15. Reproducibility

Where practical, experimental contributions should provide:

- configuration files;
- fixed seeds;
- opening lists;
- engine versions;
- model hashes;
- scripts;
- raw or summarized logs.

The goal is to make results independently checkable.

---

## 16. Logging opponent-specific experiments

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

This allows the inference loop to be audited.

---

## 17. Ablations are strongly encouraged

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
- no active probing.

---

## 18. Baselines matter

Please compare new mechanisms against strong and simple alternatives.

Depending on the experiment, useful baselines may include:

- Stockfish;
- Lc0;
- random near-optimal reranking;
- Style-only;
- Expert-only;
- parameter-matched generic model;
- ordinary self-play;
- static opponent profiles.

---

## 19. Do not claim novelty casually

A new repository contribution should not use phrases such as:

> “first ever”

unless a serious literature review supports the claim.

Prefer:

> “we propose”

or:

> “we did not find an equivalent mechanism in the sources we reviewed.”

---

## 20. Do not claim effectiveness without experiments

Documentation should not convert:

```text
proposed mechanism
```

into:

```text
effective mechanism
```

unless controlled evidence exists.

---

## 21. AI-assisted contributions

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

## 22. Online competitive-use boundary

The intended project scope includes:

- offline research;
- engine-vs-engine play;
- local experimentation;
- controlled tournaments;
- analysis of recorded games.

Unauthorized real-time assistance to a human during competitive online chess is outside the intended project scope.

---

## 23. Third-party software

This repository's documentation license does not override licenses belonging to:

- Lc0;
- Stockfish;
- other chess engines;
- machine-learning libraries;
- datasets;
- third-party code.

Contributors are responsible for complying with all relevant dependency licenses.

---

## 24. Documentation licensing direction

The current intended license for the architecture documentation is:

> **Creative Commons Attribution 4.0 International — CC BY 4.0**

The intended effect is to allow:

- copying;
- modification;
- redistribution;
- research use;
- commercial use;

without requiring case-by-case permission, while retaining an attribution requirement.

The preferred attribution name has not yet been selected.

The final license should be formalized through a repository `LICENSE` file before public release.

---

## 25. Future source-code licensing

If code is later added, it may use a separate software license.

Possible permissive candidates include:

- MIT;
- Apache-2.0;

depending on the implementation and dependency situation.

No final software license is selected by this document.

---

## 26. Attribution style

Once the preferred attribution name is selected, the repository should provide a simple recommended form.

For example:

```text
Architecture based on the
Multi-Style Multi-Specialist Chess Council proposal
by [Preferred Attribution Name].
```

The final exact wording may be added later.

---

## 27. Modifications should be identified clearly

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

## 28. Independent forks

Independent forks are welcome.

A fork does not need approval merely because it:

- removes modules;
- changes architecture;
- changes research direction;
- targets commercial use;
- focuses on only one experimental question.

The final license governs the legal permission.

---

## 29. Commercial implementations

The intended documentation-license direction permits commercial reuse subject to the final license terms and required attribution.

Commercial implementers remain separately responsible for:

- third-party licenses;
- engine licenses;
- model licenses;
- datasets;
- platform rules.

---

## 30. Discussion conduct

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

## 31. Good architectural criticism

A particularly useful critique may identify:

- duplicate module responsibility;
- hidden circular evidence;
- compute unfairness;
- undefined data flow;
- missing failure condition;
- missing baseline;
- impossible implementation assumption;
- prior-art overlap.

---

## 32. Good experimental criticism

Useful experimental criticism may identify:

- insufficient game count;
- color imbalance;
- opening bias;
- compute mismatch;
- training/test leakage;
- cherry-picked positions;
- poor calibration;
- missing confidence intervals.

---

## 33. Versioned results

If the architecture changes significantly, experiments should identify which architecture version they tested.

A result against an older design should not automatically be treated as evidence about a later substantially changed design.

---

## 34. Reproduction before escalation

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

---

## 35. Repository scope

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

## 36. Final contribution principle

> **Implement what is testable.**
>
> **Measure what is claimed.**
>
> **Credit what actually contributed.**
>
> **Remove what the evidence does not support.**