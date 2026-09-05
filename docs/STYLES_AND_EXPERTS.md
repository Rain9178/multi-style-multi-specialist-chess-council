# Styles and Experts

> **Status: Concept / Unimplemented / Untested**
>
> This document defines the proposed distinction between Style and Expertise.
> The distinction is a research hypothesis and must be tested rather than assumed.

---

## 1. Core distinction

The project separates two questions.

### Style

> Among objectively competitive ways of handling a position, what kind of solution does the policy tend to prefer?

Style is primarily a **preference concept**.

### Expertise

> In which classes of positions or strategic tasks does the policy demonstrate superior capability?

Expertise is primarily a **competence concept**.

The distinction can be summarized as:

\[
\boxed{
Preference\neq Competence
}
\]

This distinction should remain visible even when Style and Expertise interact during training or online control.

---

## 2. Style `[B+C]`

Candidate Style dimensions include:

\[
\mathbf{s}=
[
s_{\text{initiative}},
s_{\text{risk}},
s_{\text{complexity}},
s_{\text{openness}},
s_{\text{closure}},
s_{\text{restriction}},
s_{\text{prophylaxis}},
s_{\text{counterattack}},
s_{\text{simplification}},
s_{\text{asymmetry}}
]
\]

These dimensions are provisional project representations.

They are **not** claimed to be:

- a standard scientific taxonomy of chess style;
- psychologically independent variables;
- mathematically orthogonal dimensions;
- the only useful description of strategic preference.

Some candidate dimensions may eventually prove redundant or strongly correlated.

That should be resolved experimentally rather than by terminology alone.

---

## 3. Candidate Style dimensions

### Initiative

Preference for:

- creating threats;
- maintaining the initiative;
- forcing the opponent to respond.

### Risk tolerance

Tolerance for:

- material imbalance;
- king-safety risk;
- structural concessions;
- uncertain compensation.

### Complexity

Preference for:

- high branching;
- difficult tactical decisions;
- strategically ambiguous positions.

### Openness

Preference for:

- opening files;
- opening diagonals;
- central breaks;
- increased piece activity.

### Closure

Preference for:

- closed pawn structures;
- locked centres;
- manoeuvring positions.

### Restriction

Preference for:

- limiting opposing piece activity;
- controlling key squares;
- reducing counterplay.

### Prophylaxis

Preference for:

- identifying an opponent's plan;
- preventing or weakening it before direct action.

### Counterattack

Preference for:

- accepting temporary pressure;
- retaining resources for counterplay;
- striking after the opponent commits.

### Simplification

Preference for:

- exchanging pieces when advantageous;
- reducing uncertainty;
- entering technically favourable endings.

### Asymmetry

Preference for:

- structurally unbalanced positions;
- unequal pawn structures;
- unusual material distributions;
- positions with strategically different plans for each side.

---

## 4. Style is not a cartoon personality

The architecture should avoid treating policies as rigid labels such as:

```text
100% Aggressive
100% Defensive
100% Positional
```

A policy may instead express a mixture.

For example:

\[
\mathbf{s}
=
0.5\,\text{restriction}
+
0.3\,\text{prophylaxis}
+
0.2\,\text{counterattack}
\]

The final representation might be:

- continuous;
- discrete;
- latent;
- learned;
- partly hand-defined.

The project does not require the final Style representation to preserve the current candidate dimensions.

---

## 5. Stable does not mean unwilling to sacrifice

Style should not be reduced to binary stereotypes.

A relatively stable policy might:

- avoid speculative complications;
- demand clear positional or tactical compensation;
- still sacrifice material decisively once the compensation is sufficiently strong.

Therefore:

```text
Low random risk
+
High compensation threshold
+
Decisive action once justified
```

is internally coherent.

The goal is strategic preference, not caricature.

---

## 6. Expertise `[B+C]`

Candidate Expertise dimensions include:

\[
\mathbf{e}=
[
e_{\text{kingside}},
e_{\text{defence}},
e_{\text{tactics}},
e_{\text{closed-centre}},
e_{\text{open-centre}},
e_{\text{rook-endgame}},
e_{\text{minor-endgame}},
e_{\text{imbalanced-material}},
e_{\text{pawn-structure}},
e_{\text{conversion}}
]
\]

These are also provisional.

Unlike Style, an Expertise coordinate should ultimately be supported by **measured capability**, not merely by intended behaviour or routing labels.

---

## 7. Candidate Expertise domains

Possible specialist domains include:

1. kingside attack;
2. defence and king safety;
3. complex tactics;
4. closed-centre play;
5. open-centre play;
6. rook and pawn endings;
7. minor-piece endings;
8. imbalanced material;
9. pawn structures;
10. conversion of an advantage.

A future implementation may:

- combine categories;
- split categories;
- discover latent categories;
- discard categories that cannot be measured reliably.

---

## 8. Preference is not competence

This distinction is an architectural invariant.

\[
\boxed{
Style\neq Expertise
}
\]

Examples:

- A policy may frequently enter tactical positions but calculate them poorly.
- A policy may prefer quiet positions but still be exceptionally strong tactically when tactics are forced.
- A strong endgame specialist does not necessarily seek early simplification.
- A policy may prefer kingside attacks without being especially good at executing them.

Therefore:

> entering a position frequently is not sufficient evidence of being good at that position.

---

## 9. Preference evidence

Style should ideally be evaluated in positions where:

1. several moves are objectively competitive;
2. those moves differ meaningfully in strategic character;
3. the policy repeatedly expresses a stable preference.

For example, suppose several near-optimal alternatives lead toward:

- open tactical play;
- restrictive manoeuvring;
- simplification.

Repeated selection among those alternatives may provide Style evidence.

A forced move provides little Style evidence.

---

## 10. Competence evidence

Expertise requires performance evidence.

Useful signals may include:

- lower evaluation loss in target positions;
- stronger match score from controlled starting positions;
- better conversion rate;
- lower tactical failure rate;
- stronger defensive survival;
- superiority over parameter-matched non-specialists.

An Expert is therefore not merely:

> a module with an Expert label.

It must demonstrate measurable target-domain ability.

Likewise:

> targeted training does not itself establish Expertise.

A policy trained specifically for a domain or opponent weakness must still demonstrate held-out capability before being treated as a validated Expert.

---

## 11. Style and Expertise are distinct, not orthogonal

Early versions of the project described Style and Expertise as two orthogonal axes.

That wording is too strong.

The current position is:

> Style and Expertise are conceptually distinct factors whose interactions are allowed and may be important.

A possible interaction is:

\[
Style
\rightarrow
State\ Distribution
\rightarrow
Training\ Experience
\rightarrow
Expertise
\]

For example:

```text
High initiative / high complexity preference
↓
More dynamic attacking positions
↓
More training experience in those positions
↓
Possible kingside / tactical specialization
```

This is a **project hypothesis**, not an established result.

The reverse direction may also occur:

```text
Strong Expertise in a domain
↓
Higher confidence in related positions
↓
Different strategic preferences
```

Whether these interactions are significant must be tested.

---

## 12. Factorized strategic state `[B+C]`

A conceptual strategic context has historically been represented in this project as:

\[
P=(\mathbf{s},\mathbf{e})
\]

This generalizes the project's early two-axis intuition:

\[
P=(x_{\text{style}},y_{\text{expert}})
\]

However, a useful distinction is required.

### Style state

The active Style mixture may change during a game.

### Expertise capability

Measured specialist competence is normally a slower property of the policy.

### Expertise activation

The Router may change how strongly an existing Expertise capability is emphasized in the current position.

Therefore, when the repository informally writes:

\[
P_t=(\mathbf{s}_t,\mathbf{e}_t)
\]

the time-varying \(\mathbf{e}_t\) should be interpreted primarily as:

> **active Expertise emphasis or routing context**

rather than:

> the underlying specialist competence being relearned every move.

A more explicit future implementation may separate:

```text
measured Expertise capability
```

from:

```text
current Expertise activation
```

The exact notation remains an engineering choice.

---

## 13. Shared chess backbone `[A+B]`

General chess competence should normally be shared.

A generic conditional policy may be written:

\[
\pi(m|x,\mathbf{s},\mathbf{e})
=
F_{\theta_B,\theta_A}
(x;\mathbf{s},\mathbf{e})
\]

where:

- \(x\) = current chess state;
- \(\theta_B\) = shared chess parameters;
- \(\theta_A\) = optional specialization parameters.

This equation describes dependency.

It is not a fixed neural-network specification.

---

## 14. Why not train every combination?

Suppose the project eventually considers approximately:

- 10 Style archetypes or dimensions;
- 10 Expertise domains.

A naive discrete Cartesian implementation might suggest:

\[
10\times10=100
\]

complete independent models.

That is not required.

The proposal instead asks whether:

> general chess competence can be shared while behavioural preference and specialist capability are represented more economically.

---

## 15. Parameter-efficient specialization `[A+B]`

Candidate implementation families include:

- embeddings;
- adapters;
- small conditional heads;
- learned mixture weights;
- other parameter-efficient modules.

No particular method is mandatory.

For example, a future experiment might compare:

```text
Shared Backbone
+
Style Embedding
+
Expert Embedding
+
Small Conditional Head
```

against:

```text
Shared Backbone
+
Parameter-Matched Generic Head
```

This comparison is important because an apparent improvement may come merely from additional parameters rather than meaningful Style/Expert factorization.

---

## 16. Quality-constrained Style expression `[B]`

Style should normally be expressed only after base chess quality is constrained.

Let:

\[
\mathcal C_\epsilon
\]

be the current near-optimal candidate set.

Then a conceptual Style rule is:

\[
m_{\text{style}}
=
\arg\max_{m\in\mathcal C_\epsilon}
U_{\text{style}}(m)
\]

The intended ordering is:

```text
Chess quality constraint
↓
Style preference
```

rather than:

```text
Chess quality
+
unrestricted personality reward
```

This is intended to reduce behaviours such as sacrificing material merely to appear aggressive.

---

## 17. Style scoring in an early prototype

A black-box prototype may use interpretable features such as:

- material imbalance;
- king-safety change;
- number of legal replies;
- centre opening;
- file opening;
- exchange tendency;
- spatial restriction;
- asymmetry;
- concentration of attacking pieces.

These are prototype instruments.

They are not claimed to be a final scientific definition of Style.

A learned representation may later replace them.

---

## 18. Expert relevance vs Expert competence

These are two different questions.

### Expert relevance

Does this position belong to a domain where a specialist may matter?

### Expert competence

Is this particular specialist actually stronger than the alternatives here?

Therefore:

```text
This is a rook ending
```

does not automatically imply:

```text
The rook-ending Expert must control the position.
```

A future Router should be able to use both domain relevance and measured specialist capability.

---

## 19. Expertise competence vs Expertise activation

A further distinction is required.

### Expertise competence

Represents measured capability.

Examples:

- strong rook-endgame performance;
- strong tactical calculation;
- strong defensive survival.

This should normally change only through:

- training;
- evaluation;
- validated capability updates.

### Expertise activation

Represents how strongly the current control system uses or emphasizes that capability.

This may change from move to move.

Therefore:

\[
\boxed{
ExpertiseCompetence
\neq
ExpertiseActivation
}
\]

For example:

```text
Policy has strong tactical Expertise
+
current position is quiet and closed
```

may correctly produce:

```text
low tactical-Expert activation
```

without implying that tactical competence disappeared.

---

## 20. Expert activation

Possible signals include:

- material configuration;
- game phase;
- pawn structure;
- king safety;
- tactical complexity;
- learned position embeddings;
- historical specialist performance.

The project should avoid a purely hard-coded rule such as:

> “endgame detected → endgame Expert always wins routing.”

Activation should depend on both:

```text
domain relevance
```

and:

```text
validated specialist capability.
```

---

## 21. Style × Expert compatibility `[C]`

Not every combination is equally natural in every position.

A future system may learn or estimate:

\[
C(x,\mathbf{s},\mathbf{e})
\]

where \(C\) represents compatibility in position \(x\).

Examples might include:

- restriction × closed centre: often compatible;
- simplification × direct kingside attack: sometimes in tension;
- low-risk Style × strategic sacrifice: potentially compatible when compensation is strong.

Low compatibility should not automatically prohibit an unusual combination.

Unexpected combinations may generate useful new behaviour.

---

## 22. Style may shape the training distribution `[C]`

One project hypothesis is:

\[
Style
\rightarrow
State\ Distribution
\rightarrow
Expertise
\]

For example:

```text
High-complexity preference
↓
More open / kingside-complex positions
↓
More experience in those states
↓
Potential tactical / kingside competence growth
```

Another example:

```text
Restriction preference
↓
More closed structures
↓
More experience in manoeuvring states
↓
Potential closed-centre specialization
```

This mechanism should not be assumed.

It requires experimental verification.

---

## 23. Style is not the same as human imitation

The project does not require a Style to resemble a historical human master.

A successful Style may be:

- recognizably distinct;
- strategically coherent;
- stable across relevant positions;

while still being:

> distinctly machine-like.

Human-likeness is therefore not a primary success criterion.

The central question is behavioural differentiation under strong chess constraints.

---

## 24. Optional human-game Style seeds

Historical human games may optionally be used as:

- initialization data;
- behavioural seeds;
- interpretability references.

For example, historical players may help illustrate concepts such as:

- dynamic attack;
- prophylaxis;
- restriction;
- simplification.

However, the project is not intended to reproduce historical personalities exactly.

A trained system may develop Style combinations that have no direct human analogue.

---

## 25. League roles are not Style or Expertise

Training roles and strategic dimensions must remain separate.

### League roles

Examples include:

```text
Main
Exploiter
Historical
```

These describe:

> training purpose or policy lineage.

### Style

Describes:

> strategic preference.

### Expertise

Describes:

> demonstrated capability.

Therefore:

\[
Main\neq Style
\]

and:

\[
Exploiter\neq Expert
\]

A Main policy may support many Style × Expertise configurations.

An Exploiter may use whichever Style and Expertise combination best exposes a target weakness.

Likewise:

> a policy does not become an Expert merely because it is an Exploiter.

---

## 26. Strategic trajectory

A game can be viewed conceptually as:

\[
P_1
\rightarrow
P_2
\rightarrow
P_3
\rightarrow
\dots
\]

where:

\[
P_t=(\mathbf{s}_t,\mathbf{e}_t)
\]

represents the currently emphasized strategic mixture.

As noted earlier, the time-varying Expertise term should primarily be understood as **active Expertise emphasis**, not rapid rewriting of underlying capability.

For example:

```text
Opening:
stable development

↓ position changes

Middlegame:
initiative × kingside attack

↓ attack is neutralized

Closed structure:
restriction × closed-centre expertise

↓ centre opens

Tactical phase:
high-complexity Style × tactical expertise

↓ simplification

Ending:
technical / low-risk Style × rook-endgame expertise
```

The objective is not frequent switching.

The objective is **strategically meaningful switching**.

---

## 27. Persistent routing

A Style/Expert combination should not normally be reselected independently every move.

Possible stabilizers include:

- minimum tenure;
- switching margin;
- cooldown;
- Plan continuity;
- asymmetric hysteresis.

These are Router-level mechanisms.

They do not change the definitions of Style or Expertise.

---

## 28. Targeted specialist training does not establish Expertise

Later adaptive versions of the architecture may create a candidate specialist after:

```text
opponent vulnerability
↓
TrainingRequest
↓
targeted training
```

This process creates:

> a **candidate specialist**

not:

> a validated Expert.

A candidate specialist must still be evaluated against appropriate controls.

Possible questions include:

- Does it outperform the shared base model in the target domain?
- Does it outperform a parameter-matched generic adaptation?
- Does it transfer beyond one opponent?
- Does its gain survive held-out starting positions?
- Does it create unacceptable regression elsewhere?

Only demonstrated capability should justify an Expertise claim.

Therefore:

\[
TargetedTraining
\not\Rightarrow
ValidatedExpertise
\]

---

## 29. Style collapse `[B+C]`

Training may cause differentiated policies to converge toward similar behaviour.

Possible monitoring signals include:

- policy-distribution distance;
- controlled-position preference differences;
- held-out Style classification;
- Style-vector drift;
- behavioural feature distance.

The project must avoid circular validation.

For example:

> A classifier trained on the exact same handcrafted Style labels used to train the policies cannot by itself prove that meaningful Style diversity exists.

Independent evaluation is required.

---

## 30. Candidate Style-preservation methods

Possible future methods include:

- near-optimal Style preference;
- Style anchors;
- behavioural diversity regularization;
- preference-consistency objectives;
- optional Style-seeded training;
- conditional parameter separation.

All remain experimental.

A Style-preservation mechanism should be considered unsuccessful if the resulting diversity is achieved mainly by large strength loss.

---

## 31. Expertise evaluation

Experts should be tested on held-out target distributions.

Possible suites include:

- kingside attacks;
- defensive positions;
- tactical positions;
- closed centres;
- open centres;
- rook endings;
- minor-piece endings;
- imbalanced-material positions;
- pawn-structure problems;
- conversion positions.

Controls should include:

- base model;
- wrong specialist;
- parameter-matched generic model;
- equal search budget.

For adaptive specialists, useful additional controls may include:

- pre-adaptation policy;
- observation-only adaptation;
- opponent-specific training without domain transfer;
- held-out related opponents.

---

## 32. Style evaluation

Style should preferably be measured through several independent methods:

- controlled preference positions;
- policy-distribution differences;
- blind human review;
- held-out behavioural classifiers;
- cross-opening consistency;
- cross-opponent consistency.

A Style is more convincing if multiple independent measurements agree.

---

## 33. Initial prototype scale

The early prototype does not need the full candidate taxonomy.

A practical first test may use:

```text
3 Style profiles
×
3 Expertise profiles
```

The purpose is not to establish a permanent 3×3 system.

It is to test whether:

- preference differences are visible;
- specialist relevance is meaningful;
- joint routing behaves coherently;
- the Quality Gate prevents obvious strength collapse.

Only useful dimensions should survive later expansion.

---

## 34. Main hypotheses

The central hypotheses are:

### H1 — Style

Distinct Style conditioning can produce measurable preference differences without unacceptable chess-strength loss.

### H2 — Expertise

Expert conditioning can produce genuine target-domain competence differences.

### H3 — Non-redundancy

Style and Expertise contain useful information that is not fully redundant.

### H4 — Joint control

Style × Expert control produces more useful strategic differentiation than Style-only or Expert-only alternatives.

### H5 — Factorization

Factorized Style/Expert conditioning provides value beyond merely increasing parameter count.

### H6 — Distributional interaction

Style-induced changes in training-state distribution may contribute to later specialization.

### H7 — Activation separation

Separating measured Expertise competence from online Expertise activation improves interpretability and prevents routing state from being mistaken for learned capability.

This final group of hypotheses remains especially uncertain and requires explicit testing.

---

## 35. Failure conditions

This branch should be considered unsuccessful if:

- Style labels produce no meaningful behavioural difference;
- Style diversity requires unacceptable objective-strength loss;
- Experts do not outperform appropriate controls;
- Style and Expertise cannot be meaningfully distinguished;
- a parameter-matched generic model performs equally well;
- routing labels change while observable chess behaviour does not;
- Style dimensions collapse into redundant or arbitrary coordinates;
- targeted training produces specialist labels without held-out competence;
- Expertise activation cannot be meaningfully separated from measured competence.

If these failures occur, the architecture should simplify rather than preserve the terminology artificially.