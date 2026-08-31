# Risks and Open Questions

> **Status: Living Research-Risk Document**
>
> This document records reasons the proposal may fail, places where the architecture may be overcomplicated, and questions that remain deliberately unresolved.

---

## 1. General warning

The architecture combines many individually plausible mechanisms.

That does not imply that the combined system is useful.

The largest global risk is:

> **architecture bloat.**

Any mechanism without:

- a distinct function;
- a measurable hypothesis;
- a baseline;
- an ablation;
- an implementable interface;

should remain outside the core system.

---

## 2. Chess may not need this architecture

Modern chess engines are already extremely strong.

It is entirely possible that:

> generic strong chess dominates most benefits of Style diversity, opponent modelling, or opponent-specific exploitation.

If so, the project may be interesting as a behavioural research system without improving competitive strength.

That would still be a valid result.

---

## 3. League necessity is uncertain

AlphaStar-style population training is powerful in complex multi-agent environments.

Chess is:

- deterministic;
- two-player;
- perfect-information;
- turn-based;
- zero-sum.

Strong chess self-play already works well.

Therefore a League may provide little benefit relative to its cost.

The League is optional until experiments justify it.

---

## 4. Architecture complexity may hide simple explanations

Suppose the full system wins more games.

Possible alternative explanations include:

- more search nodes;
- larger parameter count;
- better opening selection;
- more training data;
- implementation asymmetry.

The experimental programme must control these possibilities.

---

## 5. Style may be artificial

The proposed Style dimensions may be:

- redundant;
- subjective;
- culturally biased;
- poorly aligned with machine strategy.

A policy may be strategically different in ways not captured by human Style vocabulary.

The project must allow learned or revised representations.

---

## 6. Style dimensions may be correlated

Candidate dimensions such as:

- initiative;
- complexity;
- openness;
- risk;

may strongly correlate.

Therefore the current vector should not be treated as an orthogonal coordinate system.

Some dimensions may eventually:

- merge;
- disappear;
- become latent factors.

---

## 7. Style may become performance-destroying theatre

A Style mechanism can create visible differences simply by making bad moves.

That would be an easy but uninteresting success.

The Quality Gate is intended to reduce this risk.

However, the Quality Gate itself may be imperfect.

---

## 8. Quality Gate may be myopic

A move that appears near-optimal at one search budget may be inferior under deeper search.

Possible causes:

- tactical horizon;
- noisy neural evaluation;
- long-term strategic consequences;
- unstable search.

Therefore:

> passing the Quality Gate does not guarantee objective safety.

---

## 9. Quality Gate may suppress Style completely

If \(\epsilon\) is too small:

```text
CandidateSet ≈ one move
```

and Style has little room to act.

If \(\epsilon\) is too large:

```text
Style freedom ↑
Chess quality ↓
```

The usable region may be narrow.

---

## 10. Fake specialization

An Expert may receive a label without demonstrating superior capability.

Possible failure:

```text
"Rook Endgame Expert"
```

simply activates in rook endings but does not play them better.

Expertise must be measured by conditional performance.

---

## 11. Preference and competence may be difficult to disentangle

A policy that prefers tactical play may receive more tactical training and become tactically stronger.

Then Style and Expertise become causally connected.

This does not invalidate factorization automatically, but it may make clean statistical disentanglement impossible.

---

## 12. Factorization may add no value

A generic conditional model with similar capacity may perform as well as explicit Style × Expert factorization.

If so, the architectural distinction may remain useful conceptually but unnecessary computationally.

---

## 13. Parameter-efficient specialization may be insufficient

Adapters, embeddings, or small heads may not provide enough capacity to create genuine specialist competence.

Possible alternatives include:

- partial backbone fine-tuning;
- stronger conditional modules;
- separate specialist networks.

Those alternatives increase cost and may undermine the shared-backbone advantage.

---

## 14. Shared backbone may cause homogenization

Sharing too much representation may make different policies behaviourally similar.

Sharing too little may:

- duplicate chess knowledge;
- increase compute;
- reduce maintainability.

The correct sharing boundary is unknown.

---

## 15. Style collapse

League or outcome-based training may gradually make all Style policies converge toward similar objectively strong behaviour.

Possible mitigations exist, but they may artificially preserve difference.

The project must distinguish:

> useful strategic diversity

from:

> diversity enforced for its own sake.

---

## 16. Diversity metric risk

A diversity metric may reward superficial differences.

Examples:

- different opening choices;
- random move variation;
- arbitrary Style features.

Behavioural distance alone does not prove strategic value.

---

## 17. Human Style classification may be unreliable

Human observers may:

- disagree;
- project historical-player stereotypes;
- overinterpret short games.

Blind human evaluation should therefore be only one measurement channel.

---

## 18. Historical-master analogies can mislead

Using names such as Tal, Petrosian, Karpov, or Capablanca may help explanation.

But the project is not intended to reproduce their actual policies.

Human-player analogies should not become hidden training definitions unless explicitly tested.

---

## 19. Router may become a “god module”

If every subsystem sends unstructured information into one opaque Router, the Router becomes impossible to:

- understand;
- test;
- ablate.

The Router should remain a bounded meta-controller with explicit inputs and outputs.

---

## 20. Router may duplicate base search

A sufficiently complicated Router might begin performing its own chess evaluation.

That would duplicate the base engine.

The architecture should preserve the boundary:

```text
Router:
strategic control

Base engine:
concrete chess verification
```

---

## 21. Persistent routing may create strategic inertia

Minimum tenure and Plan continuity reduce oscillation.

But excessive persistence may prevent:

- tactical response;
- superior alternatives;
- necessary Style changes.

Break conditions must override ordinary persistence.

---

## 22. Plan semantics may be underspecified

A chess search engine naturally produces:

- moves;
- evaluations;
- principal variations.

It does not automatically produce human concepts such as:

> “restrict the queenside.”

A first implementation should use simple structured PlanState.

Natural-language or semantic planning is not required.

---

## 23. Strategic exceptions may be hard to define

Possible search signals such as:

- PV instability;
- evaluation swings;
- MultiPV gaps;

may not map cleanly to strategic exceptions.

The correct compression from search evidence to Router-relevant information remains open.

---

## 24. Opponent sample scarcity

One game contains few informative opponent decisions.

A detailed profile may be statistically impossible.

The system must tolerate:

- broad uncertainty;
- unresolved hypotheses;
- weak personalization.

---

## 25. Opening theory reduces identity evidence

An opponent may follow known theory for many moves.

Such moves can be highly predictable while revealing little about Style.

The system needs forcedness and information-value correction.

---

## 26. Opponent Style may be position-dependent

The same player may appear:

- aggressive in one opening;
- defensive in another;
- simplifying in a favourable ending.

A single permanent Style label may therefore be misleading.

The Trait / Mode distinction is intended to help but remains untested.

---

## 27. Model-family misspecification

All active Shadows may be wrong.

A dangerous failure occurs when:

```text
Shadows agree
+
Shadows are confident
+
Reality disagrees
```

Ensemble agreement cannot be treated as proof of correctness.

---

## 28. Correlated Shadows

Shadows may share:

- backbone;
- training data;
- search;
- inductive biases.

Therefore they are not independent samples.

Naive Bayesian combination may become badly overconfident.

---

## 29. Challenger overuse

Too much Challenger compute may waste resources continuously testing unlikely alternatives.

This can reduce:

- prediction depth;
- exploitation depth;
- base search resources.

The correct Focus/Challenge balance is unknown.

---

## 30. Challenger underuse

Too little Challenger compute may create confirmation bias:

```text
early hypothesis
↓
most resources follow it
↓
alternative models receive little testing
↓
initial mistake persists
```

The allocation must remain dynamic.

---

## 31. Sentinel reserve may be wasted compute

Maintaining non-zero exploratory coverage provides protection against model miss.

But if the opponent is extremely stable and well understood, Sentinel compute may provide almost no value.

The minimum reserve should be tested rather than treated as sacred.

---

## 32. Predictability may be mismeasured

A position may be predictable because it is forced.

That does not imply that the opponent is generally predictable.

Predictability measures need contextual correction.

---

## 33. ModelTrust may lag reality

A model can remain historically well calibrated while the opponent has just changed Mode.

Recent evidence may need more influence than old evidence.

The right temporal weighting is unknown.

---

## 34. Trait vs Mode may not be identifiable

What appears to be:

> temporary Mode

may actually be:

> previously hidden Trait.

Or the reverse.

The architecture should maintain uncertainty instead of forcing a clean answer.

---

## 35. Bounded adaptation may still collapse Shadows

Even small local adaptation may cause several Shadows to converge toward the same behavioural model.

Diversity must be monitored.

---

## 36. Adaptation radius is unknown

If the radius is too small:

> Shadows cannot fit the opponent.

If too large:

> Shadow identity disappears.

No universal radius is assumed.

---

## 37. Online gradient training may overfit

A single game provides very little data.

Unrestricted online SGD could cause:

- catastrophic drift;
- recent-move overfitting;
- instability;
- difficult rollback.

Therefore early implementations should prefer lightweight latent or mixture adaptation.

---

## 38. Prediction may not improve play

This is one of the most important risks.

Even if:

\[
PredictionQuality\uparrow
\]

it may be that:

\[
PlayingStrength
\approx unchanged
\]

because a strong generic engine already chooses robust moves.

Opponent prediction and opponent exploitation must be evaluated separately.

---

## 39. Weakness may be unreachable

An opponent may have a real weakness in a certain position class.

But reaching that class may require objectively inferior chess.

Therefore:

\[
Weakness\neq UsefulTarget
\]

Reachability and objective cost are essential.

---

## 40. Vulnerability may be transient

A weakness observed once may be caused by:

- one tactical oversight;
- time trouble;
- unusual opening;
- temporary Mode.

It should not automatically become a durable opponent trait.

---

## 41. Vulnerability graph may become a narrative generator

A dangerous failure mode is:

```text
W1 implies W2
W2 implies W3
W3 implies W4
```

without enough real evidence.

The graph could then become a plausible-sounding story rather than a predictive model.

Graph expansion must require evidence.

---

## 42. Dynamic graph may add no value

A static weakness profile may perform just as well.

If so:

> remove the graph.

The dynamic graph has a higher burden of proof because it adds substantial complexity.

---

## 43. Repair behaviour can be misinterpreted

If an opponent moves defenders toward a region, the system may infer:

> that region is a weakness.

But the move may simply be objectively best play.

Opponent repair behaviour must be interpreted together with base-engine evaluation.

---

## 44. Active probing may be too rare

Near-optimal moves that are also highly informative may occur infrequently.

If so, active probing may never justify implementation complexity.

It remains optional.

---

## 45. Active probing may accidentally reduce strength

A move can be only slightly worse under current evaluation but strategically inferior long-term.

Information-seeking should remain subordinate to robust chess quality.

---

## 46. Asymmetric hysteresis may become excessive conservatism

“Trust slowly, retreat quickly” protects against model errors.

But if retreat is too aggressive, the system may never exploit a real weakness strongly enough to gain value.

The asymmetry requires calibration.

---

## 47. Belief vs control separation adds complexity

Separating:

- belief;
- compute;
- intervention;

is conceptually cleaner.

But it creates more control variables.

A future implementation may discover that a simpler approximation works equally well.

---

## 48. Population explosion

Style × Expertise × Main × Exploiter × history can produce a huge policy population.

Without strict admission criteria, compute may become unmanageable.

Population expansion must be value-driven.

---

## 49. Archive explosion

Even if archived policies are cheap relative to training, indefinite history accumulation creates:

- storage;
- evaluation;
- bookkeeping;
- regression-test cost.

Archive retention policy remains an open engineering problem.

---

## 50. Non-transitivity complicates ranking

A population may contain:

```text
A > B
B > C
C > A
```

A single Elo score may obscure these relationships.

Evaluation must preserve pairwise structure where useful.

---

## 51. Exploiters may overfit

An Exploiter may discover an extremely narrow trick against one Main.

Training too heavily against it may:

- improve one matchup;
- reduce generality.

The correct balance between targeted repair and broad robustness is unknown.

---

## 52. League may simply burn compute

This is an explicit failure condition.

If:

```text
League compute >> self-play compute
```

while:

```text
robustness gain ≈ zero
```

the League should be removed.

---

## 53. Experimental leakage

Specialist test positions may accidentally appear in training data.

Style classifiers may reuse training labels.

Opening suites may be tuned after seeing results.

These forms of leakage can invalidate conclusions.

---

## 54. Evaluation cherry-picking

The project should avoid selecting only:

- visually impressive games;
- successful exploitations;
- favourable Style examples.

Full match and experiment distributions should be reported.

---

## 55. Baseline weakness

A complex architecture can appear strong if compared only against a weak baseline.

Strong modern chess engines must remain central controls.

---

## 56. Prior-art overlap

Several architectural components already have substantial research precedent.

Examples include:

- population training;
- opponent modelling;
- dynamic expert routing;
- hierarchical control;
- temporal abstraction;
- weakness exploitation;
- human move prediction.

The project should not turn a new combination into an unsupported claim of first discovery.

---

## 57. Academic novelty is unresolved

The architecture may eventually contain:

- novel combinations;
- novel mechanisms;
- no academically novel mechanism at all.

This cannot be established through internal discussion.

Formal novelty claims require literature review.

---

## 58. Third-party software licensing

A permissive architecture-document license does not override the licenses of:

- Lc0;
- Stockfish;
- other engines;
- datasets;
- third-party libraries.

Independent implementers remain responsible for dependency compliance.

---

## 59. Implementation portability

The architecture should ideally not depend permanently on one engine.

However, changing the base engine may affect:

- evaluation semantics;
- MultiPV behaviour;
- search interface;
- neural representations.

Portability must be tested rather than assumed.

---

## 60. Human-opponent modelling raises separate issues

If future experiments use human data, additional concerns include:

- privacy;
- data availability;
- player identity;
- behavioural drift;
- rating differences;
- platform rules.

These are outside the core architecture but matter for real deployments.

---

## 61. Online competitive-use boundary

The project is intended for:

- offline research;
- engine-vs-engine testing;
- local play;
- controlled experiments.

Unauthorized real-time assistance to humans in competitive online chess is outside project scope.

---

## 62. Open Style questions

Unresolved questions include:

- How many Style dimensions are useful?
- Should Style be human-readable or latent?
- Which dimensions are redundant?
- How stable should Style be?
- Can Style exist without strength sacrifice?
- Should historical games seed Style?
- How should Style collapse be measured?

---

## 63. Open Expertise questions

Unresolved questions include:

- Which specialist domains are meaningful?
- How should specialist datasets be built?
- How should competence be normalized for search budget?
- How much parameter separation is necessary?
- Can Expertise emerge naturally from Style-driven state distributions?

---

## 64. Open Quality Gate questions

Unresolved questions include:

- fixed or adaptive \(\epsilon\)?
- which evaluation scale?
- tactical vs strategic thresholds?
- how to handle evaluation uncertainty?
- should different higher-level mechanisms use different gates?

---

## 65. Open Router questions

Unresolved questions include:

- rule-based or learned?
- hard routing or soft mixture?
- how much state should it observe?
- how is intervention strength calibrated?
- when should Plan continuity break?
- what search exceptions should be exposed?

---

## 66. Open League questions

Unresolved questions include:

- Is a League needed at all?
- How many policies?
- Which population roles?
- Which matchmaking scheme?
- How large should the Archive become?
- What triggers new population members?
- How should style collapse be prevented?

---

## 67. Open opponent-model questions

Unresolved questions include:

- how should initial priors be formed?
- how many Shadows?
- how should Shadow diversity be measured?
- how should correlated likelihoods be calibrated?
- how much Challenger budget is useful?
- how much Sentinel budget is useful?
- how should Mode Shift be detected?
- how should ModelTrust be estimated?

---

## 68. Open adaptation questions

Unresolved questions include:

- which parameters may adapt online?
- what is the adaptation radius?
- when should a Shadow be modified vs replaced?
- how should adaptation be rolled back?
- how should Shadow identity be preserved?

---

## 69. Open vulnerability questions

Unresolved questions include:

- what constitutes one vulnerability node?
- when is evidence sufficient?
- how should context similarity be defined?
- how should vulnerability confidence decay?
- how many graph transitions should be retained?
- how should repair-induced weaknesses be validated?

---

## 70. Open experimental questions

Unresolved questions include:

- which opening suites?
- which fixed search budgets?
- which Style test positions?
- which Expertise test sets?
- which human evaluators?
- which effect sizes count as practically meaningful?
- how much additional compute is acceptable?

---

## 71. Removal principle

Every mechanism should have a clear removal condition.

Examples:

```text
League adds no value
→ remove League
```

```text
Dynamic graph ≈ static profile
→ remove dynamic graph
```

```text
Active probing adds no measurable value
→ remove active probing
```

```text
Style × Expert ≈ generic conditional model
→ simplify factorization
```

---

## 72. Final risk principle

> **The architecture should remain easier to falsify than to defend rhetorically.**

If a mechanism survives only because every negative result can be reinterpreted as another hidden success, the mechanism is not scientifically useful.