# Opponent Modelling

> **Status: Concept / Unimplemented / Untested**
>
> This document describes the proposed online opponent-modelling architecture.
>
> The project does not claim that a reliable opponent profile can always be recovered from a single chess game.

---

## 1. Purpose

Opponent modelling asks several different questions:

1. What strategic preferences does the opponent appear to have?
2. In which kinds of positions does the opponent appear strong or weak?
3. What is the opponent likely to do next?
4. Has the opponent changed short-term strategy?
5. How uncertain is the current model?
6. How much should the rest of the system trust this model?
7. Is the observed opponent behaviour valuable enough to justify persistent memory or more expensive training-side investigation?

These questions should not be collapsed into one categorical opponent label.

Opponent modelling is an inference system.

It is not itself:

- the strategic Router;
- the League;
- the playing policy;
- the training coordinator.

---

## 2. Data limitation

A single chess game contains relatively few decisions.

Many moves may be:

- forced;
- theoretical;
- tactically obvious;
- strategically non-discriminative.

Therefore fine-grained opponent identification may be impossible in some games.

The architecture must support:

- broad priors;
- uncertainty;
- unresolved competing explanations.

If historical games of the same opponent are legally and experimentally available, they may provide additional prior evidence.

They are optional, not required.

### Selective cross-game memory `[B+C]`

A future adaptive system may preserve selected information across repeated encounters.

Possible retained information includes:

- compressed opponent summaries;
- calibrated Trait estimates;
- previously supported Mode families;
- recurring vulnerability hypotheses;
- opponent-family membership;
- previous model failures;
- previously useful specialist references.

This does **not** imply permanent storage of every encountered opponent.

A possible hierarchy is:

```text
raw encounter
↓
short-term record
↓
compressed opponent summary
↓
optional strategy-family representation
↓
possible training-archive candidate
```

Retention should be:

- selective;
- value-gated;
- experimentally justified.

---

## 3. Recognizer `[A+B+C]`

The **Recognizer** summarizes evidence from real observed opponent behaviour.

It should not behave like:

```text
Opponent = Aggressive
Confidence = 100%
```

A more appropriate output is a distribution over competing hypotheses.

Possible inferred variables include:

- Style preference;
- competence patterns;
- long-term Trait;
- short-term Mode.

The exact latent representation remains open.

The Recognizer primarily explains:

> **what has actually been observed.**

It should not manufacture evidence from its own predictions.

---

## 4. Preference evidence vs competence evidence

Two evidence channels must remain distinct.

### Preference evidence

Primarily asks:

> What does the opponent choose when several objectively competitive alternatives exist?

This is most relevant to **Style**.

### Competence evidence

Primarily asks:

> Where does the opponent's actual move quality or decision quality systematically deteriorate or improve?

This is most relevant to:

- Expertise;
- weakness;
- strength.

Therefore:

\[
PreferenceEvidence
\neq
CompetenceEvidence
\]

A preference for tactical positions does not prove tactical weakness or tactical strength.

Likewise, weakness in a domain does not imply that the opponent prefers or avoids that domain.

---

## 5. Forced moves provide weak identity evidence

Suppose all plausible opponent models predict the same move because it is nearly forced.

Then observing that move may have:

- high raw predictability;
- low opponent-identification value.

This distinction is important.

The system should not infer:

> “I predicted the move, therefore I understand the opponent.”

Sometimes the position itself was predictable.

---

## 6. Information value `[A+B]`

A move is especially informative when:

1. several objectively competitive alternatives exist;
2. competing opponent hypotheses assign meaningfully different probabilities to them;
3. the real opponent chooses one of those alternatives.

Conceptually, useful evidence depends on:

\[
\text{Hypothesis Disagreement}
+
\text{Observed Choice}
\]

rather than candidate count alone.

Information gain is one possible formal tool.

The proposal does not mandate one exact information-theoretic equation.

---

## 7. Evidence weighting

A future evidence-weighting function may consider:

- base-engine forcedness;
- candidate quality spread;
- strategic diversity of candidates;
- Shadow disagreement;
- evaluation uncertainty;
- observed move likelihood.

A hard binary Evidence Gate is not required.

A continuous information weight is preferred.

Evidence quality should remain distinct from:

- prediction confidence;
- compute allocation;
- training value.

---

## 8. Shadow Ensemble `[B+C]`

The **Shadow Ensemble** represents multiple coherent hypotheses about the current opponent.

A Shadow is not simply another copy of Lc0.

Conceptually:

\[
p_i(m_{t+1}|h_t)
=
G_{\theta_B,\phi_i}
(h_t;z_i)
\]

where:

- \(h_t\) = observed game history;
- \(\theta_B\) = shared chess representation;
- \(z_i\) = opponent-hypothesis conditioning;
- \(\phi_i\) = optional lightweight Shadow-specific parameters.

The earlier shorthand:

\[
Shadow_i
=
B_{Lc0}
+
z_i
+
A_i
\]

should be interpreted only as a conceptual decomposition.

It does not require literal neural-network parameter addition.

---

## 9. Shadow hypothesis identity

A Shadow may encode a hypothesis such as:

```text
High initiative
+
high complexity
+
kingside preference
+
current attacking Mode
```

Another may represent:

```text
Low risk
+
high simplification
+
strong technical conversion
```

The important requirement is:

> Shadows must predict meaningfully different behaviour when the hypotheses differ.

Multiple identical engine instances do not form a useful hypothesis ensemble.

---

## 10. Four quantities that must remain separate

### 10.1 Hypothesis identity

What kind of opponent does this Shadow represent?

### 10.2 Belief weight

How plausible is this hypothesis given real observations?

### 10.3 Predictive confidence

Assuming this Shadow is correct, how concentrated is its next-move prediction?

### 10.4 Ensemble disagreement

How differently do currently plausible Shadows predict?

These quantities should not be collapsed.

---

## 11. High confidence can coexist with high disagreement

Suppose:

```text
Shadow A:
90% probability of move X

Shadow B:
90% probability of move Y
```

Both Shadows are individually confident.

But the ensemble strongly disagrees.

This means:

> several coherent opponent hypotheses remain unresolved.

It does not automatically mean that every Shadow is low confidence.

---

## 12. Recognizer–Shadow cross-correction `[B+C]`

The intended division of labour is:

### Recognizer

> Which hypotheses explain the opponent's real past behaviour?

### Shadow Ensemble

> Which hypotheses successfully predict the opponent's real future behaviour?

The two systems may inform each other across time.

But a critical rule applies:

> **A prediction is not evidence until reality arrives.**

---

## 13. Correct evidence loop

The intended loop is:

```text
Real game history
↓
Recognizer prior / belief
↓
Candidate Shadow hypotheses
↓
Shadow predictions
↓
REAL opponent move
↓
Prediction scoring
↓
Belief update
↓
Next-round Shadow allocation
```

The real opponent move is the external anchor.

---

## 14. Forbidden self-confirming loop

The architecture must avoid:

```text
Recognizer says "aggressive"
↓
Aggressive Shadow predicts aggressive move
↓
System treats its own prediction as evidence
↓
Recognizer becomes more confident
```

This creates synthetic confirmation.

Only real observations may close the inference loop.

---

## 15. One move is still one sample

If ten Shadows all analyse the same real opponent move, the system has not obtained ten independent real observations.

One move can provide many model comparisons.

But:

\[
1\ real\ move
\neq
10\ independent\ samples
\]

This matters for confidence calibration.

---

## 16. Bayesian updating `[A+B]`

Bayesian or likelihood-based updating is a natural candidate.

Conceptually:

\[
P(H_i|o_{1:t})
\propto
P(o_t|H_i,o_{1:t-1})
P(H_i|o_{1:t-1})
\]

However, Shadows may be highly correlated because they share:

- chess knowledge;
- representations;
- training data.

Therefore naive likelihood multiplication may become overconfident.

Possible future safeguards include:

- tempered likelihoods;
- probability floors;
- forgetting factors;
- calibration;
- model-diversity constraints.

No final update rule is fixed.

---

## 17. Focus / Challenger / Sentinel `[C]`

Shadow resources may be organized into three roles.

### Focus

Refines high-probability hypotheses.

### Challenger

Represents plausible alternatives that make meaningfully different predictions.

Its purpose is to challenge the dominant model.

### Sentinel

Maintains small coverage outside the currently dominant region.

Its purpose is to detect:

- major model-family miss;
- unexpected strategy;
- premature pruning.

These are **resource-allocation roles**, not necessarily separate neural architectures.

---

## 18. Belief is not compute allocation

Let:

\[
p_i
\]

represent belief in hypothesis \(i\).

Let:

\[
q_i
\]

represent compute allocated to testing Shadow \(i\).

Then:

\[
\boxed{
p_i\neq q_i
}
\]

A low-probability Challenger may receive relatively high compute if it is highly discriminative.

That compute must not itself raise its belief.

---

## 19. Why fixed inverse allocation is insufficient

A naive rule such as:

```text
Recognizer says A = 70%
↓
Give A only 30% Shadow compute
```

may overcorrect.

If the opponent is already reliably identified, such a rule wastes resources.

Likewise:

```text
Recognizer says A = 70%
↓
Give A exactly 70% compute
```

may cause early confirmation bias.

The allocation should therefore depend on the current evidence state.

---

## 20. Dynamic Focus–Challenge balance `[C]`

A conceptual resource policy may interpolate between:

- following current belief;
- testing informative alternatives.

For example:

\[
q_i
=
\lambda_t p_i
+
(1-\lambda_t)c_i
\]

where:

- \(p_i\) = current belief;
- \(c_i\) = Challenger / test value;
- \(\lambda_t\) = current tendency to exploit the established model rather than challenge it.

This is only a conceptual control form.

It is not a validated allocation equation.

---

## 21. When Focus should increase

Focus allocation may increase when:

- high-information observations repeatedly support the same hypothesis;
- predictions are well calibrated;
- Shadow disagreement falls;
- Mode Shift risk is low;
- prediction performance remains stable.

Then the system may invest more resources in:

- local refinement;
- weakness modelling;
- opponent-specific exploitation.

---

## 22. When Challenger allocation should increase

Challenge allocation may increase when:

- the game is early and evidence is sparse;
- plausible Shadows disagree strongly;
- prediction performance deteriorates;
- the real opponent repeatedly chooses low-probability moves;
- Mode Shift risk rises;
- the current hypothesis family appears incomplete.

---

## 23. Sentinel reserve

Even at high confidence, a small exploratory reserve may remain.

Conceptually:

\[
q_{\text{sentinel}}>0
\]

The purpose is not permanent distrust.

It is protection against:

- strategy change;
- model collapse;
- deception;
- unseen behavioural regimes.

The exact minimum reserve is experimental.

---

## 24. Predictability is not ModelTrust

These are different quantities.

### Opponent Predictability

How consistently structured or forecastable does the opponent's behaviour appear?

### ModelTrust

How much evidence supports the correctness and calibration of our **current model**?

Example:

```text
Opponent behaviour is stable
+
our entire Shadow family is wrong
```

could yield:

```text
Predictability = potentially high
ModelTrust = low
```

Therefore Predictability should not become a universal confidence number.

---

## 25. OpponentModelState `[B+C]`

A lightweight state may be represented as:

\[
M_t=(P_t,T_t,D_t,R_t)
\]

where:

- \(P_t\) = Predictability;
- \(T_t\) = ModelTrust;
- \(D_t\) = Shadow disagreement;
- \(R_t\) = Mode / strategy-shift risk.

This is not intended to be a new intelligent Agent.

It is a compact state derived from existing evidence.

---

## 26. Candidate Predictability signals

Possible signals include:

### Predictive concentration

Are next-move distributions concentrated or diffuse?

### Historical predictive performance

How well have previous predictions matched real moves?

### Calibration

When the system says “70%,” is that approximately reliable over time?

### Shadow disagreement

Do plausible hypotheses agree or diverge?

### Forcedness correction

Was prediction easy because of the opponent, or because the position was nearly forced?

---

## 27. Predictability may control resources

High Predictability and high ModelTrust may justify:

- fewer active Shadows;
- more Focus compute;
- finer local refinement;
- higher confidence in targeted opponent modelling.

Low Predictability or low ModelTrust may justify:

- more Challenger compute;
- Shadow Archive revival;
- broader hypothesis coverage;
- less opponent-specific control.

Predictability changes resource allocation.

It does not define truth.

### Predictability is not TrainingValue

A difficult-to-predict opponent is not automatically worth expensive adaptation.

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
low training value
```

Conversely:

```text
moderate-strength opponent
+
reproducible exploit against our system
```

may have very high training value.

A conceptual training-value estimate might consider:

\[
TrainingValue
=
f(
Strength,
Novelty,
ReproducibleExploit,
ReusableValue,
ExpectedGain,
ComputeCost
)
\]

This is a conceptual decomposition only.

No final training-value equation is fixed.

TrainingValue should answer:

> Is this behaviour worth sending to the training ecosystem for further investigation?

It should **not** answer:

> Is the underlying hypothesis true?

Those remain separate questions.

---

## 28. Trait and Mode

The architecture distinguishes:

### Trait

A slower, more durable opponent characteristic.

### Mode

A faster, context-dependent strategic state.

Example:

A generally stable opponent may temporarily enter:

- an attacking Mode;
- a defensive Mode;
- a simplification Mode.

A short Mode change should not immediately rewrite long-term Trait.

---

## 29. Different update timescales

A possible policy is:

```text
Short anomaly
↓
possible Mode update

Repeated sustained evidence
↓
gradual Trait update
```

The exact implementation may use:

- change-point detection;
- sequential hypothesis testing;
- threshold-based logic;
- another method.

No specific detector is mandatory.

Opponent-model timescales should also remain distinct from:

- match-scale playing-policy adaptation;
- long-term League training.

---

## 30. Opening and cold start

The opponent model should not activate purely according to move number.

In highly theoretical or forced opening play:

- preference evidence may be weak;
- competence evidence may be weak;
- base-engine play should dominate;
- only minimal Shadow coverage may be necessary.

As discriminative decisions appear:

```text
coarse hypotheses
↓
stronger evidence
↓
finer hypotheses
```

may become useful.

---

## 31. Coarse-to-fine Shadows `[C]`

A future implementation may begin with broad hypotheses such as:

```text
A
B
C
```

and refine a promising region into:

```text
A1
A2
A3
```

This should be evidence-driven.

The system should not create a large fine-grained Shadow tree before sufficient information exists.

---

## 32. Bounded Shadow adaptation `[C]`

A Shadow need not remain perfectly fixed.

Conceptually:

\[
\phi_i(t)
=
\phi_i^0
+
\delta_i(t)
\]

subject to a bounded local region:

\[
\|\delta_i(t)\|
\le
r_i
\]

The purpose is to allow local refinement without destroying Shadow identity.

---

## 33. What may adapt online?

This section concerns **opponent-model adaptation**.

It should not be confused with training the system's own playing policy.

Early implementations should prefer lightweight Shadow or opponent-model adjustments such as:

- Style mixture;
- Expert mixture;
- latent conditioning;
- adapter mixture weights;
- local behavioural parameters.

The architecture does not recommend unrestricted in-game training of the shared chess backbone.

### Opponent-model adaptation vs playing-policy adaptation

These are different processes.

```text
Shadow adaptation
```

asks:

> How should our hypothesis about the opponent change?

while:

```text
playing-policy adaptation
```

asks:

> Should our own chess capabilities or specialist policies change?

The second belongs primarily to the training ecosystem.

Opponent modelling may provide evidence supporting a `TRAIN_REQUEST` or equivalent message.

It should not directly:

- retrain the League;
- overwrite a Main policy;
- replace the shared chess backbone;
- promote a candidate specialist.

---

## 34. Why adaptation must be bounded

Without bounds, a Shadow intended to represent:

```text
aggressive kingside opponent
```

might gradually drift into:

```text
technical simplifying endgame opponent
```

At that point, the hypothesis identity has changed.

Large shifts should normally be handled through:

- another Shadow;
- new Challenger creation;
- archived-hypothesis revival;
- belief transfer.

Small change:

> local adaptation.

Large change:

> hypothesis change.

---

## 35. Soft pruning and Shadow Archive `[B+C]`

Low-value Shadows need not be permanently deleted.

Possible states include:

```text
Active
Low-Compute
Archived
Reactivated
```

An archived Shadow may be revived when:

- current hypotheses fail;
- real moves suddenly fit it;
- ShiftRisk rises;
- the active ensemble becomes confidently wrong.

### Archive terminology boundary

Several forms of memory should remain distinct.

#### Shadow Archive

Stores inactive opponent hypotheses used by the online opponent model.

#### Encounter Memory

Stores selected summaries from real past encounters.

#### League / Training Archive

Stores policies, Exploiters, historical opponents, or training representatives used by the offline training ecosystem.

Therefore:

```text
Shadow Archive
≠
Encounter Memory
≠
League / Training Archive
```

They may exchange information through controlled interfaces.

They should not be treated as one undifferentiated storage system.

---

## 36. Confidently wrong ensemble

A dangerous case is:

```text
Most Shadows agree
+
predictions are individually confident
+
real opponent repeatedly does something else
```

This should not increase confidence.

It indicates possible model-family failure.

Possible reactions include:

- lower ModelTrust;
- expand Challenger coverage;
- reactivate Shadow Archive hypotheses;
- reduce opponent-specific Router influence;
- reconsider Mode;
- increase event-triggered deliberation.

---

## 37. Prediction task

Shadow quality should be evaluated as a prediction problem.

Possible metrics include:

- negative log-likelihood;
- Brier score;
- top-k move accuracy;
- calibration;
- strategic-class prediction;
- prediction stability.

Raw top-1 accuracy alone may hide poor probability calibration.

---

## 38. Opponent profiling and human behaviour

If the system is tested against human players, strong engine move prediction is not automatically an adequate behavioural baseline.

Human move prediction is a distinct research task.

Future experiments should therefore compare against appropriate behavioural models when available.

The same caution applies when historical human games are used for persistent opponent summaries.

---

## 39. Active probing `[A+B+C]`

A later-stage optional mechanism is **active probing**.

Suppose several moves already pass the Quality Gate.

If their objective quality is similar, the system may prefer a move whose likely opponent responses distinguish competing hypotheses more strongly.

Conceptually:

```text
near-optimal move
+
high discrimination value
```

may be preferred over:

```text
equally strong move
+
little information value
```

---

## 40. Active probing safety boundary

The system should not sacrifice substantial objective chess quality merely to gather information.

Therefore active probing is subordinate to:

\[
m\in\mathcal C_\epsilon
\]

or another equivalent quality constraint.

If informative probing opportunities are too rare, the mechanism should be removed.

Active probing is intended to improve inference.

It is not permission to play objectively poor moves merely to manipulate the opponent model.

---

## 41. Deception and deliberate nonstationarity

A future opponent may deliberately attempt to mislead the modelling system.

Examples include:

- behaving like one Style before switching;
- presenting a temporary false weakness;
- changing policy after ModelTrust rises;
- deliberately increasing apparent predictability;
- using several near-optimal strategy families.

The initial architecture should **not** require a separate recursive Deception Agent.

Instead, deception may be represented through:

- competing Shadow hypotheses;
- higher ShiftRisk;
- lower ModelTrust;
- increased Challenger allocation;
- Sentinel coverage;
- Mode-change hypotheses;
- out-of-family model-failure warnings.

If later experiments demonstrate that these mechanisms are insufficient, a more explicit deception model may be considered.

The architecture should avoid unbounded recursion such as:

```text
I model
your model
of my model
of your model
...
```

unless a controlled experiment demonstrates measurable value.

---

## 42. From opponent evidence to training-side investigation

Opponent modelling may sometimes discover behaviour that appears valuable beyond the current move or game.

Examples include:

- a reproducible weakness;
- an unfamiliar strategy family;
- a policy that reliably exploits the current system;
- a recurring Mode-switch pattern;
- a failure mode shared across several Shadows or policies.

Such a discovery may produce a compact candidate request:

```text
candidate opponent / policy family
evidence summary
reproducible vulnerability
novelty estimate
possible reusable value
uncertainty
```

This may be passed toward the training ecosystem as:

```text
TRAIN_REQUEST
```

or an equivalent interface.

### Critical boundary

A `TRAIN_REQUEST` means:

> this pattern may be worth further evaluation.

It does **not** mean:

> the pattern has been proved;
> training must occur;
> more compute should increase belief;
> a new specialist should be deployed immediately.

The training side must independently decide whether to:

- reject the request;
- gather more evidence;
- allocate limited compute;
- reproduce the finding;
- train candidate Exploiters or specialists;
- archive the pattern;
- deploy nothing.

This preserves:

\[
Evidence
\neq
TrainingCompute
\neq
Deployment
\]

---

## 43. Required output to the Router

The opponent-modelling layer should not send a giant unstructured model.

A useful compact interface may contain:

```text
OpponentBelief
OpponentModelState
top Shadow hypotheses
predicted move distributions
preference evidence summary
competence evidence summary
Mode-shift warning
model-failure warning
```

The exact format remains an engineering decision.

Training-side information should preferably use a separate compact interface rather than overloading the Router.

---

## 44. Opponent modelling does not equal exploitation

Improved prediction is not automatically useful chess exploitation.

The project must separately test:

\[
BetterPrediction
\stackrel{?}{\Longrightarrow}
BetterPlayingPerformance
\]

It is entirely possible that:

- prediction improves;
- but strong generic chess already performs just as well.

That would still be an important negative result.

Likewise:

\[
BetterPrediction
\not\Rightarrow
TargetedTrainingIsWorthwhile
\]

and:

\[
DetectedWeakness
\not\Rightarrow
ProfitableWeaknessAmplification
\]

These are separate hypotheses requiring separate experiments.

---

## 45. Main hypotheses

### H1 — Preference inference

Real move choices provide enough information to infer useful Style-related preferences in at least some positions.

### H2 — Predictive Shadows

A diverse Shadow Ensemble predicts opponent behaviour better than a generic baseline.

### H3 — Cross-correction

Recognizer + Shadow performs better than Recognizer-only or Shadow-only modelling.

### H4 — Dynamic allocation

Focus / Challenger / Sentinel resource control improves calibration or efficiency.

### H5 — Bounded adaptation

Small online local adaptation improves model fit without destroying hypothesis diversity.

### H6 — Predictability control

Predictability and ModelTrust improve resource allocation and reduce overconfident exploitation.

### H7 — Mode detection

Separating fast Mode from slow Trait improves robustness to strategic shifts.

### H8 — Selective persistent memory

Value-gated cross-game opponent summaries improve later modelling or playing performance without requiring permanent storage of every opponent.

### H9 — Training-value separation

Explicitly separating Predictability from TrainingValue reduces wasted adaptation compute and improves resource allocation.

### H10 — Training-request usefulness

Opponent-modelling outputs can identify a subset of discoveries that are sufficiently reproducible and reusable to justify training-side investigation.

All remain untested.

---

## 46. Failure conditions

The opponent-modelling branch should be simplified or abandoned if:

- prediction does not improve over simple baselines;
- Shadows are behaviourally redundant;
- confidence is poorly calibrated;
- model-family miss is common and undetected;
- Challenger allocation adds cost without reducing error;
- bounded adaptation causes Shadow collapse;
- Mode detection creates excessive false alarms;
- cross-game memory produces no measurable value;
- TrainingValue estimates repeatedly waste compute;
- training requests have excessive false-positive rates;
- better prediction does not improve actual play;
- compute overhead exceeds practical value.

The architecture should tolerate the possibility that:

> a strong general chess engine is already too robust for detailed opponent modelling to justify its complexity.

---

## 47. Final boundary

The opponent-modelling system should remain:

> **a probabilistic, evidence-grounded description of the opponent — not an all-purpose intelligence layer.**

Its role is to improve:

- prediction;
- uncertainty estimation;
- strategic control;
- resource allocation;
- identification of potentially reusable opponent-specific information.

The base chess system remains responsible for chess competence.

The Router remains responsible for strategic control.

The training ecosystem remains responsible for training, evaluation, archival policy, and validated capability deployment.