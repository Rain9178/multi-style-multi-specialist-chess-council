# Architecture

> **Status: Concept / Unimplemented / Untested**
>
> This document defines the proposed system-level architecture.
> It does not describe a completed implementation or a validated system.

---

## 1. Purpose

The project investigates whether several normally separate functions can coexist in one chess system:

1. strong general chess competence;
2. distinguishable strategic Style;
3. measurable specialist Expertise;
4. population-based training pressure;
5. online opponent inference;
6. controlled opponent-specific exploitation;
7. selective adaptation across multiple timescales.

The architecture is intentionally decomposable.

Each major component should have:

- a distinct function;
- explicit inputs and outputs;
- a simpler baseline;
- an ablation;
- a failure condition.

A component that does not provide measurable value should be removable.

---

## 2. Provenance notation

This repository uses three provenance labels:

- `[A]` — Prior Art / Established Technique
- `[B]` — Project Adaptation / Composition
- `[C]` — Project-Specific Hypothesis

These labels answer:

> Where does this idea come from?

They do **not** answer:

> Has this project proved that it works?

Unless explicitly updated in the future, all project-level integrations remain unimplemented and untested.

---

## 3. Architectural overview

The proposal operates across multiple timescales.

These timescales are conceptual.

A future implementation may combine, subdivide, or omit some of them.

### 3.1 Online / fast loop

Used continuously or nearly continuously for:

- analysing the current position;
- base-engine search;
- maintaining CandidateSet;
- updating opponent evidence;
- maintaining Shadow hypotheses;
- estimating vulnerabilities;
- maintaining a lightweight Plan;
- controlling opponent-specific strategic influence.

### 3.2 Event-triggered deliberation loop

Activated selectively when ordinary incremental processing may no longer be sufficient.

Possible triggers include:

- major evaluation shock;
- tactical refutation;
- repeated prediction failure;
- severe Shadow disagreement;
- Plan invalidation;
- unexpected Mode Shift;
- newly supported high-impact vulnerability;
- evidence that the system itself may be entering an unfamiliar failure regime.

Possible responses include:

- deeper or broader base search;
- increased Shadow compute;
- renewed Challenger / Sentinel coverage;
- Plan reconstruction;
- current-position rollout;
- temporary strategic-response exploration;
- requests for more expensive adaptation.

This is not intended to impose a fixed amount of “emergency thinking.”

Additional computation should be justified by expected value relative to:

- remaining clock time;
- compute cost;
- uncertainty;
- potential position recovery;
- risk of overfitting.

### 3.3 Match-scale / intermediate adaptation

Some adaptation may occur across:

- several moves;
- one game;
- a short match;
- repeated encounters with the same opponent.

Possible mechanisms include:

- Style / Expertise reweighting;
- opponent-model refinement;
- local adapter or latent adjustment;
- temporary response branches;
- opponent-specific summaries;
- selective policy or specialist activation.

This layer should remain bounded and experimentally controlled.

### 3.4 Offline / slow training loop

Used for:

- learning general chess competence;
- developing Style and Expertise;
- evaluating specialist capability;
- optional population / League training;
- active self-red-teaming;
- maintaining historical policies;
- training Exploiters;
- validating candidate specialists;
- regression testing;
- maintaining training archives.

Conceptually:

```text
                    OFFLINE / SLOW TRAINING LOOP

┌──────────────────────────────────────────────────────────┐
│                                                          │
│  Shared Chess Backbone                                   │
│          ↓                                               │
│  Style / Expertise Conditioning                          │
│          ↓                                               │
│  Specialist & Behaviour Evaluation                       │
│          ↓                                               │
│  Optional Population / League Training                   │
│          ↓                                               │
│  Main / Exploiter / Historical Policies                  │
│          ↓                                               │
│  Evaluator → Payoff State → Matchmaking                  │
│          ↓                                               │
│  Validated / Archived Policies                           │
│                                                          │
└──────────────────────────────────────────────────────────┘
                         ↑
                         │
            validated training requests
                         │
                         │
┌──────────────────────────────────────────────────────────┐
│                                                          │
│               ONLINE / MATCH-SCALE SYSTEM                │
│                                                          │
│  Position + Game History                                 │
│          ↓                                               │
│  Base Engine / Search → CandidateSet                     │
│          ↓                                               │
│  Opponent Modelling                                      │
│     ├─ Recognizer                                        │
│     └─ Shadow Ensemble                                   │
│          ↓                                               │
│  OpponentModelState                                      │
│          ↓                                               │
│  Vulnerability Hypotheses                                │
│          ↓                                               │
│  Router + PlanState                                      │
│          ↕                                               │
│  Base Search / Verification                              │
│          ↓                                               │
│  Quality-Constrained Candidate Selection                 │
│          ↓                                               │
│  Move                                                    │
│          ↓                                               │
│  Real Opponent Response → New Evidence                   │
│                                                          │
│  High-value discovery                                    │
│          ↓                                               │
│  Optional TRAIN_REQUEST / ADAPTATION_REQUEST              │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

The loops interact, but they are not the same system.

In particular:

> **Offline League policies are not the same thing as online Shadow hypotheses.**

And:

> **League roles such as Main, Exploiter, and Historical policy are training roles or policy lineages, not online Style / Expertise labels.**

---

## 4. Shared Chess Backbone `[A+B]`

The architecture assumes a strong existing source of general chess competence.

The initial neural candidate is Leela Chess Zero (Lc0), although the architecture should not permanently depend on one particular engine.

Other strong engines may be used as:

- alternative foundations;
- baselines;
- evaluators;
- candidate generators.

The shared chess layer is responsible for:

- legal chess behaviour;
- board representation;
- tactical competence;
- positional competence;
- policy/value estimation where applicable;
- search;
- candidate generation;
- base move quality.

The architecture does not require every Style, Expert, Shadow, or League policy to relearn fundamental chess knowledge from scratch.

### Design principle

> Share what should remain universal.
>
> Specialize only what needs to differ.

---

## 5. Style × Expertise `[B+C]`

A strategic policy context may be represented conceptually as:

\[
P_t=(\mathbf{s}_t,\mathbf{e}_t)
\]

where:

- \(\mathbf{s}_t\) represents Style;
- \(\mathbf{e}_t\) represents Expertise.

### Style

Represents preferred ways of solving a position among objectively competitive alternatives.

### Expertise

Represents demonstrated capability in particular position classes or strategic tasks.

These are conceptually distinct factors.

They are **not** assumed to be mathematically orthogonal or statistically independent.

Interactions are explicitly allowed.

For example:

\[
Style
\rightarrow
State\ Distribution
\rightarrow
Training\ Experience
\rightarrow
Expertise
\]

is a project hypothesis.

Style / Expertise describe strategic conditioning.

They do not define League roles such as:

- Main;
- Exploiter;
- Historical policy.

A Main policy may itself support many Style × Expertise configurations.

---

## 6. Quality-Constrained Candidate Set `[B]`

Higher-level strategic mechanisms should not have unrestricted authority over move quality.

Let:

\[
q_{\text{base}}(m|x)
\]

be a quality score produced by one internally consistent base evaluation system for move \(m\) in position \(x\).

Let:

\[
m^*=\arg\max_m q_{\text{base}}(m|x)
\]

A conceptual near-optimal set is:

\[
\mathcal C_\epsilon(x)
=
\left\{
m:
q_{\text{base}}(m^*|x)
-
q_{\text{base}}(m|x)
\le
\epsilon(x)
\right\}
\]

Higher-level mechanisms normally operate only on:

\[
m\in\mathcal C_\epsilon(x)
\]

Possible higher-level preferences include:

- Style;
- Expert relevance;
- opponent exploitation;
- Plan continuity;
- optional information value.

### Important boundary

This formula defines an architectural principle, not a fixed scoring unit.

A future implementation may use:

- expected score;
- WDL-derived values;
- normalized Q;
- centipawn-derived measures;
- another calibrated base-quality representation.

Scores from incompatible evaluation systems should not be mixed naively.

---

## 7. CandidateSet

A future implementation should expose a structured candidate interface between chess search and higher-level reasoning.

A conceptual `CandidateSet` might store:

```text
move
base_quality
principal_variation
search_stability
style_features
expert_relevance
opponent_exploitation_score
plan_compatibility
optional_information_value
```

A first prototype does not need every field.

The purpose of this structure is to prevent the Router from becoming an undefined system that somehow receives “everything.”

---

## 8. Two implementation depths

The proposal supports two substantially different engineering routes.

### 8.1 Black-box orchestration `[B]`

Existing chess engines remain externally controlled.

Possible interfaces include:

- UCI;
- MultiPV;
- engine evaluation;
- principal variations;
- controlled node or time budgets.

An external controller may then:

- construct a CandidateSet;
- apply the Quality Gate;
- score Style;
- estimate Expert relevance;
- maintain PlanState;
- maintain opponent models;
- rerank eligible candidates.

This is the preferred route for an initial implementation because it avoids modifying the internal neural chess engine.

### 8.2 White-box conditional chess model `[B+C]`

A deeper implementation may modify a neural chess model and/or training pipeline.

Candidate mechanisms include:

- Style embeddings;
- Expert embeddings;
- adapters;
- conditional policy heads;
- learned gating;
- partially shared specialization.

This route is substantially more difficult.

A successful black-box prototype does not imply that white-box specialization will automatically work.

---

## 9. Parameter-efficient specialization `[A+B]`

A generic conditional policy can be represented as:

\[
\pi(m|x,\mathbf{s},\mathbf{e})
=
F_{\theta_B,\theta_A}
(x;\mathbf{s},\mathbf{e})
\]

where:

- \(x\) is the current chess state;
- \(\theta_B\) represents shared chess competence;
- \(\theta_A\) represents optional specialized parameters.

The architecture does not require one particular specialization technique.

Candidates may include:

- embeddings;
- adapters;
- small conditional heads;
- mixture weights;
- other parameter-efficient methods.

The value of factorized specialization should be tested against parameter-matched alternatives.

---

## 10. Minimal online data objects

To keep the architecture implementable, the following conceptual objects should remain distinguishable.

### PositionContext

Possible contents:

```text
board / FEN
move history
side to move
game phase
time information, if relevant
```

### CandidateSet

Contains objectively evaluated candidate moves.

### OpponentBelief

Contains current probabilistic opponent hypotheses and their evidence.

### OpponentModelState

Contains lightweight meta-information about the reliability and stability of the opponent model.

### VulnerabilityHypothesis

Represents one uncertain, context-dependent exploitable weakness.

### PlanState

Represents a temporally persistent strategic intention.

### ControlState

Possible contents:

```text
active Style mixture
active Expertise mixture
opponent-specific intervention strength
active Plan
Shadow compute allocation
fallback state
adaptation mode
```

### TrainingRequest

An optional message indicating that an observed pattern may justify more expensive training-side investigation.

Possible contents:

```text
trigger reason
target opponent / policy family
relevant vulnerability
evidence strength
estimated reusable value
urgency
current compute context
```

A `TrainingRequest` is not itself a training action.

These are conceptual interfaces, not mandatory software classes.

---

## 11. Opponent inference

Opponent inference is separated from strategic control.

A future system may maintain:

### OpponentBelief

Probabilistic hypotheses about:

- Style preferences;
- competence patterns;
- long-term Trait;
- current Mode.

### OpponentModelState

A lightweight state may contain:

\[
(P_t,T_t,D_t,R_t)
\]

where:

- \(P_t\) = opponent predictability;
- \(T_t\) = trust in the current model;
- \(D_t\) = disagreement among plausible Shadows;
- \(R_t\) = strategy / Mode-shift risk.

These values should not be collapsed into one universal confidence score.

---

## 12. Belief, compute, and control are separate

The architecture requires three distinct quantities.

### Belief

How plausible is a hypothesis?

### Compute allocation

How much computation should be spent testing or refining it?

### Control commitment

How strongly should it influence actual chess decisions?

Therefore:

\[
\boxed{
Belief
\neq
Compute
\neq
Control
}
\]

A low-probability Challenger may receive substantial compute because it is highly informative.

That extra compute does not itself increase the probability of the hypothesis.

The same separation should also apply to training requests:

> Receiving more training compute must not itself increase the evidential credibility of the hypothesis that triggered the request.

---

## 13. Router `[A+B+C]`

The Router is a strategic **meta-controller**.

It should not become a second, weaker chess engine.

Possible inputs include:

- PositionContext;
- CandidateSet;
- active Style/Expert state;
- OpponentBelief;
- OpponentModelState;
- vulnerability hypotheses;
- PlanState;
- switching cost;
- search exceptions;
- available compute information.

Possible outputs include:

- Style weights;
- Expert weights;
- target vulnerability;
- Plan continue / reconsider / terminate;
- opponent-specific intervention strength;
- compute-budget guidance;
- optional `TRAIN_REQUEST` or `ADAPTATION_REQUEST`.

### Router boundary

The Router should normally decide:

> **how existing chess capabilities should be used**

rather than:

> **what the objectively best chess move is from scratch.**

That remains the responsibility of the base chess system and its search.

Likewise, the Router may request more expensive adaptation, but it should not directly:

- run League training;
- overwrite the shared backbone;
- promote candidate policies;
- control the training archive.

Those functions belong to the training-side process, evaluator, or equivalent budget / deployment logic.

---

## 14. PlanState `[A+B]`

The architecture does not require a separate Plan AI.

A Plan is a lightweight temporally extended control state.

Possible fields include:

```text
goal
target
active_style
active_expertise
start_ply
minimum_tenure
continuation_conditions
termination_conditions
break_conditions
confidence
```

The Plan need not contain a natural-language explanation.

The base engine remains responsible for concrete move calculation and verification.

A Plan may persist for several plies, but it must terminate when sufficiently strong chess evidence invalidates it.

---

## 15. Dynamic Vulnerability Hypothesis Graph `[B+C]`

Opponent weaknesses should be represented as uncertain, contextual hypotheses.

A vulnerability is not simply:

\[
1-\text{Opponent Expertise}
\]

Its exploitation value may depend on:

- evidence;
- current position;
- our corresponding capability;
- reachability;
- objective chess cost;
- likely opponent repair;
- downstream consequences.

A proposed extension represents transitions such as:

```text
W1
↓
Opponent repair
↓
Resource reallocation
↓
W2
```

For example, repairing a kingside weakness may consume defensive resources and expose another region.

The graph stores **hypotheses**, not claims that the system knows the opponent's true weaknesses.

A later-stage extension may also ask whether a confirmed weakness can be amplified through targeted training.

That distinction is important:

```text
Weakness detection
≠
Weakness amplification
```

The second requires separate experimental evidence.

---

## 16. Online Shadow Ensemble vs offline League

This distinction is architectural.

### League policy

A trained or archived policy used primarily in the population-training ecosystem.

Examples:

- Main policy;
- Exploiter;
- historical policy;
- specialist policy.

### Shadow

An opponent hypothesis used during online or replay-based opponent modelling.

A Shadow may reuse representations learned offline, but:

> a Shadow is not automatically a League Agent.

### League roles vs strategic conditioning

Main / Exploiter / Historical describe training roles or policy lineages.

Style / Expertise describe strategic conditioning or capability dimensions.

Therefore:

```text
Main
≠
Style

Exploiter
≠
Expert

Historical policy
≠
Shadow
```

A Main policy may use multiple Style × Expertise configurations.

An Exploiter may also use any Style or Expertise configuration useful for exposing a weakness.

### Active self-red-teaming `[A+B]`

A later-stage League may deliberately train policies whose objective is to expose weaknesses in:

- current Main policies;
- specialist policies;
- Router assumptions;
- opponent-modelling assumptions;
- historical regression resistance.

A successful attack should not automatically be treated as proof of a permanent defect.

It should instead become:

```text
candidate exploit
↓
replication
↓
evaluation
↓
training pressure / archive decision
```

The purpose is to reduce known exploitability, not to claim that all possible counter-strategies have been exhausted.

### Online-training boundary

The online system should not require full policy retraining during every game.

Ordinary play should normally rely on:

- base search;
- existing policies;
- Router control;
- opponent-model updates;
- bounded local adaptation.

However, sufficiently important events may optionally trigger:

- asynchronous targeted training;
- temporary response branches;
- limited specialist adaptation;
- current-position rollout.

Such adaptation should remain:

- bounded;
- separately logged;
- experimentally controlled;
- reversible or safely bypassable.

Unrestricted in-game replacement of the shared chess backbone is not a design requirement.

---

## 17. Router ↔ Base Search

The relationship is bidirectional.

### Router → Base Search

Possible requests include:

- compare selected strategic branches;
- allocate additional search to selected candidates;
- validate a vulnerability hypothesis;
- test continuation of the current Plan.

### Base Search → Router

Possible strategic exceptions include:

- major tactical refutation;
- large evaluation deterioration;
- dramatically superior alternative;
- unstable evaluation;
- major phase transition;
- evidence that the current Plan is unsound.

A black-box UCI prototype may approximate these signals using:

- MultiPV gaps;
- evaluation swings;
- principal-variation changes;
- search-depth or node sensitivity.

A deeper implementation may expose richer internal search information.

---

## 18. Opponent-specific intervention strength `[B+C]`

Let:

\[
\alpha_t\in[0,1]
\]

represent how strongly opponent-specific strategic reasoning may influence the final decision.

It does **not** mean:

\[
\alpha_t
=
\text{percentage of total chess intelligence}
\]

The base engine remains present at all times.

A qualitative control pattern might be:

```text
Low evidence / low ModelTrust
→ low opponent-specific intervention

Repeated calibrated evidence
→ intervention may increase

Strong contradiction / model failure
→ intervention decreases rapidly
```

The exact control law remains experimental.

---

## 19. Asymmetric hysteresis `[A+B+C]`

Hysteresis applies primarily to **control commitment**, not to suppressing statistical evidence.

Opponent beliefs should update when real evidence arrives.

Control may respond asymmetrically:

> strategic commitment builds cautiously;
> strong counter-evidence can reduce commitment faster.

Possible applications include:

- Style/Expert switching;
- Plan persistence;
- opponent-specific intervention;
- Shadow retirement and revival;
- Mode-shift response;
- adaptation escalation and de-escalation.

This is a design principle, not a separate global module.

---

## 20. Multiple adaptation timescales

The architecture should not assume that every variable changes at the same speed.

### Slow variables

Examples include:

- shared chess competence;
- trained Style/Expert modules;
- League population;
- historical policies;
- validated specialists;
- durable opponent Trait estimates when sufficient evidence exists.

### Intermediate variables

Examples may include:

- match-level opponent summaries;
- temporary response branches;
- bounded adapter state;
- opponent-specific specialist activation;
- targeted training candidates.

### Fast variables

Examples include:

- current opponent Mode;
- Shadow weights;
- Predictability;
- ModelTrust;
- disagreement;
- ShiftRisk;
- active vulnerability hypotheses;
- PlanState;
- intervention strength.

A short-term Mode change should not automatically rewrite a long-term Trait estimate.

Likewise:

> a short-term online success should not automatically rewrite a long-term Main policy.

---

## 21. Per-turn online lifecycle

A conceptual turn may proceed as follows:

1. Receive current position and game history.
2. Run base search and construct CandidateSet.
3. Update Recognizer using only real observed opponent behaviour.
4. Generate or update Shadow predictions.
5. Update OpponentModelState.
6. Update vulnerability hypotheses.
7. Check whether the current Plan remains valid.
8. Estimate whether ordinary incremental processing remains sufficient.
9. If necessary, increase deliberation or adaptation budget.
10. Router determines strategic control context.
11. Base search validates or rejects strategically biased alternatives.
12. Apply the Quality Gate.
13. Select and play a move.
14. Observe the real opponent response.
15. Use that real response as new evidence.
16. If a high-value reusable pattern is detected, optionally emit a training-side request.

This sequence is conceptual.

A practical implementation may combine several steps.

---

## 22. Prediction is not evidence

A critical invariant is:

> A model-generated prediction cannot validate the model that generated it.

The correct loop is:

```text
Real history
↓
Hypothesis
↓
Prediction
↓
REAL opponent move
↓
Prediction evaluation
↓
Belief update
```

This prevents the Recognizer and Shadow system from creating self-confirming synthetic evidence.

The same principle applies to training:

> A policy generated because a vulnerability was hypothesized does not prove that the vulnerability is real.

Only external evaluation and real or controlled opponent behaviour can provide that evidence.

---

## 23. Fallback invariant

The system must preserve a safe route toward strong general chess.

When opponent-specific modelling becomes unreliable:

\[
\text{Opponent-Specific Influence}\downarrow
\]

while:

\[
\text{Base-Engine Dominance}\uparrow
\]

The opponent model does not need to be deleted.

It may continue gathering evidence while having less influence on actual moves.

Likewise, failed or uncertain adaptive branches should not require overwriting the stable base policy.

---

## 24. Event-triggered reconsideration and deep deliberation

Not every expensive strategic computation needs to restart from zero every ply.

Cheap incremental updates may happen continuously.

Major reconsideration may be triggered by:

- tactical refutation;
- large evaluation swing;
- major exchange;
- centre opening or closing;
- transition into an ending;
- Plan completion;
- prediction failure;
- Mode Shift;
- newly supported vulnerability;
- major Shadow disagreement;
- evidence of model-family failure;
- unexpectedly high-value opponent behaviour.

### Event-Triggered Deep Deliberation `[B+C]`

A stronger response may be justified when ordinary processing appears insufficient.

Possible escalation:

```text
Normal search
↓
Additional search / verification
↓
Strategic reconsideration
↓
Expanded Shadow / Challenger analysis
↓
Current-position rollout
↓
Bounded local adaptation
↓
Optional targeted training request
```

This escalation should be **value-driven**, not duration-driven.

The system should not contain a rule such as:

```text
"Emergency detected → think exactly 20 minutes."
```

Instead, additional computation should continue only while its expected marginal value remains justified.

Possible considerations include:

- remaining clock time;
- current evaluation;
- irreversibility;
- novelty;
- uncertainty;
- model failure;
- expected recovery;
- reusable training value;
- compute cost.

### Temporary response branches `[C]`

One possible implementation is to branch from a stable policy into temporary response candidates.

For example:

```text
stable policy
├─ response branch A
├─ response branch B
├─ response branch C
└─ response branch D
```

These branches may explore different reactions without immediately modifying the stable policy.

A temporary branch may later be:

- discarded;
- archived;
- retained as an opponent-specific specialist;
- promoted to broader evaluation.

This is a candidate implementation, not a required module.

---

## 25. Shared verification

One important project principle is that diversified higher-level modules may propose different strategic preferences while a common strong search mechanism verifies them.

Conceptually:

```text
Style / Expert / Opponent-specific proposals
                    ↓
             Shared CandidateSet
                    ↓
            Shared strong verification
                    ↓
           Quality-constrained choice
```

This allows:

> diversity in strategic preference

without requiring:

> independent basic chess safety for every module.

The same principle should normally apply to candidate adaptive responses.

A proposed specialist or response branch should still be subject to strong chess verification.

---

## 26. Minimal architecture before learning

A first prototype may contain only:

```text
UCI base engine
+
MultiPV CandidateSet
+
Quality Gate
+
3 Style profiles
+
3 Expert profiles
+
rule-based Router
+
PlanState
+
structured decision logs
```

This prototype can already test:

- behavioural differentiation;
- Style/Expert interaction;
- switching coherence;
- objective strength loss.

It does not require:

- League training;
- Shadow opponents;
- vulnerability graphs;
- online neural adaptation;
- triggered training;
- self-red-teaming.

---

## 27. Suggested expansion order

Additional complexity should be introduced approximately in this logical order:

```text
Base engine
↓
Quality Gate
↓
Style
↓
Expertise
↓
Joint routing + continuity
↓
Parameter-efficient learning
↓
Optional Mini-League
↓
Opponent prediction
↓
Recognizer × Shadow
↓
Opponent-specific exploitation
↓
Dynamic vulnerability transitions
↓
Optional active probing
↓
Selective cross-game memory
↓
Value-gated training requests
↓
Triggered specialist / Exploiter training
↓
Active self-red-teaming
↓
Event-triggered adaptive deliberation
```

A later stage is not mandatory if an earlier hypothesis fails.

The ordering is conceptual rather than mandatory.

Some implementations may test later ideas in isolation.

---

## 28. Architectural invariants

Unless future experiments strongly justify a change:

1. Base chess competence remains available.
2. Style and Expertise remain conceptually distinct.
3. Preference is not treated as proof of competence.
4. Predictions do not count as observations.
5. Belief and compute allocation remain separate.
6. Belief and control commitment remain separate.
7. Plans cannot block overwhelmingly superior chess alternatives.
8. Low-probability hypotheses are preferably soft-pruned rather than permanently erased.
9. Evaluation and training remain distinct.
10. Offline League agents and online Shadows remain distinct.
11. League roles remain distinct from online Style / Expertise control.
12. Training requests do not themselves validate the hypotheses that triggered them.
13. A stable base policy should remain recoverable during bounded adaptation.
14. Complex modules must survive ablation.

---

## 29. Non-goals

The architecture does not require:

- 100 complete Style × Expert networks;
- equal voting on every move;
- a separate full engine for every Shadow;
- a second full engine inside the Router;
- natural-language strategic planning;
- unlimited in-game gradient updates;
- mandatory in-game training;
- a fixed emergency-thinking duration;
- exact reproduction of AlphaStar;
- a fixed number of Main policies;
- a fixed number of final personalities;
- permanent storage of every encountered opponent;
- recursive opponent modelling without bound.

---

## 30. Architectural failure condition

The architecture should be simplified if:

- additional components do not improve measurable outcomes;
- higher-level control frequently harms chess strength;
- multiple modules duplicate the same function;
- online opponent modelling does not improve prediction;
- improved prediction does not improve actual play;
- vulnerability modelling adds no value;
- targeted adaptation produces only narrow overfitting;
- archive growth produces more cost than robustness;
- event-triggered computation does not outperform simpler allocation;
- self-red-teaming adds compute without meaningful robustness;
- compute and engineering costs overwhelm the observed benefit.

The architecture is therefore intended to be:

> **decomposable, testable, and revisable — not sacred.**