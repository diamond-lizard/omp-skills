# Examples

Use these examples as behavioral targets, not scripts to copy verbatim. Preserve the workflow while adapting terminology, depth, and language to the user's field.

## Example 1 — When not to run the ladder

**User**
> Quick question: in this field, what does "MRO" stand for?

**Good response**

Answer it. Do not calibrate, do not open the intake, do not climb.
> MRO is *mean-field reorientation* in this context (some papers write it out as
> molecular reorientation). It is the term used for the population-averaged
> alignment change, as opposed to the single-molecule picture.
>
> If you want the fuller picture of how it fits the rest of the framework, I can
> walk you up from the motivation.

**Why this is good**

- It passes the one-turn test, so the ladder never starts.
- The offer is one line at the end, made once. If the user does not take it, it
  is dropped.
- No prerequisite checklist in front of a two-word answer.

**Also do not run the ladder when**

- the user is a specialist asking a specific technical question inside their own
  field;
- the user asked for it short ("quickly", "TL;DR");
- the task is translation, proofreading, formatting, debugging, or a search with
  a known target;
- the user is blocked mid-experiment and needs the fix;
- the user already declined the ladder earlier in the session.

## Example 2 — Field-level onboarding from an adjacent specialty

**User**

> I work on nonlinear optics but know almost nothing about topological photonics. Can you guide me into the field step by step?

**Good first response**

Start with Step 0 rather than immediately teaching topology. Name a small set of load-bearing prerequisites and exploit the user's optics background as an anchor.

> Before we start, I want to locate the shortest bridge from nonlinear optics to topological photonics. Please mark each item as **used it / learned it / new**:
>
> | Prerequisite | Why it matters |
> | --- | --- |
> | Bloch modes and band structures | Topological photonics usually describes photonic modes as bands over momentum space. |
> | Berry phase / Berry curvature | These encode geometric properties of bands and lead to topological invariants. |
> | Coupled-mode or tight-binding models | They provide compact models for lattices of coupled resonators or waveguides. |
> | Symmetry and symmetry breaking | Time-reversal, inversion, and crystalline symmetries constrain possible topological phases. |
>
> Also tell me your target: **read papers**, **understand experiments**, **design a system**, or **enter the field broadly**.
>
> I will use your nonlinear-optics intuition wherever it genuinely transfers, but I will flag where the analogy stops working.

**Why this is good**

- It calibrates specific prerequisites instead of asking a vague background question.
- It does not reteach optics the user already knows.
- It establishes the user's target before choosing depth.
- It does not dump the five-rung ladder in the first turn.

## Example 3 — Decode a dense abstract

**User**

> I cannot understand this abstract about moire excitons. Please explain it to me.

**Good response pattern**

Do not force the full Step 0 intake. Enter Decode mode.

> **Gist:** The authors are studying excitons whose motion and optical properties are modified by a long-period moire potential created when two atomically thin crystals are stacked with a small twist or lattice mismatch.
>
> **Terms in order**
> - **Exciton:** a bound electron-hole pair that behaves as a composite quasiparticle.
> - **Moiré superlattice:** a long-wavelength periodic pattern produced by two slightly mismatched lattices.
> - **Localization:** confinement of an exciton's center-of-mass motion to particular regions of that moire potential.
>
> **Reconstructed passage:** [Rewrite the supplied abstract faithfully in simpler language, preserving its claims.]
>
> **What you need to evaluate the claim:** (1) how excitons form in the relevant material system and (2) how a moire potential modifies their energy landscape.
>
> **Source fidelity:** I will label statements that are background or my inference separately from claims explicitly made in the abstract.

When the actual abstract is available, preserve its terminology and scientific scope. Do not insert mechanisms, causality, or conclusions that the authors did not state.

## Example 4 — Hands-on request about chiral phonons

**User**

> I do experimental optics and want to get into chiral phonons. Please walk me through it step by step; I mainly want to know how they are verified experimentally.

**Good first response**

Calibrate only the prerequisites that matter for an experimental target.

> Understood. Your target is **experimental verification of chiral phonons**, so I will route the ladder toward physical picture → measurable signature → experimental scheme, rather than starting from the full group-theory formalism.
>
> First, please mark each item below as **used it / learned it / new**:
>
> | Prerequisite | Why it matters |
> | --- | --- |
> | Lattice vibrations and phonon eigenmodes | The "rotation" of a chiral phonon appears first in the phase relation between atomic displacements. |
> | Brillouin zone and K/K' valleys | Phonon chirality in many two-dimensional materials is tied to inequivalent valleys. |
> | Circularly polarized light and angular momentum | Experimental selection rules usually involve angular-momentum exchange between light and phonons. |
> | Raman / IR selection rules | They determine which phonon modes a given spectroscopy can see. |
>
> I read your experimental target as "**judge whether a material has a measurable chiral-phonon signature, and design an optical verification scheme**". If you actually lean toward time-resolved pump-probe or Raman, tell me and I will adjust the route.

**Why this is good**

- It uses experimental optics as an anchor.
- It adapts the ladder to the stated hands-on target by preparing to expand Rung 4.
- It does not assume that valley physics automatically proves phonon chirality.

## Example 5 — Checkpoint and branching

After teaching a rung about Berry curvature, do not end with only "Does that make sense?"

**Better checkpoint**

> Quick check: suppose the Berry curvature is strongly concentrated near K and has the opposite sign near K'. If both valleys are equally populated, what would you expect for a valley-summed Hall-like response?
>
> A. The two valley contributions tend to cancel.  
> B. They necessarily add.  
> C. Berry curvature becomes zero at each valley.

**Branching behavior**

- If the user chooses **A** and explains the cancellation correctly, advance.
- If the user chooses **C**, explain the difference between *local nonzero Berry curvature* and *cancellation after summing contributions* using a momentum-space picture.
- If the user says this is trivial, skip ahead rather than adding more elementary questions.

## Example 6 — Respect an explicit request for a compact overview

**User**

> I know the basics already. Give me the whole roadmap in one response so I can decide what to study.

**Good behavior**

Honor the request instead of enforcing one rung per turn. Give a compact five-rung roadmap, identify likely prerequisites, and let the user choose where to zoom in. Keep the overview diagnostic rather than turning every rung into a full lecture.

## Example 7 — Distinguish source claim from inference

**User**

> The paper says the mode has circular atomic motion at K. Does that prove it is a chiral phonon and that circularly polarized Raman must detect it?

**Good response pattern**

> **Source claim:** The paper reports circular atomic motion for the mode at K.
>
> **Background:** Circular or elliptical atomic motion can be associated with phonon angular momentum, but terminology and definitions of phonon chirality depend on the symmetry and mode context.
>
> **Inference:** The reported eigenvector may support a chiral-phonon interpretation, but that conclusion requires checking the mode symmetry, degeneracy, valley relation, and the definition used by the authors.
>
> **Not established by that statement alone:** It does not automatically imply that conventional circularly polarized Raman can detect the mode. Raman visibility depends on momentum conservation, Raman tensors or higher-order processes, resonance conditions, and the experimental geometry.

The exact technical content must be adjusted to the supplied source and verified when freshness or source-specific claims matter.

## Example 8 — Labelling a reading path

At Rung 5 the skill names specific works, which is where fabrication happens.
Every named item is either verified in this session or explicitly marked
unverified.

**Good behavior, with search available**
> **Reading path**
>
> 1. *Orientation:* [review title], [journal, year] — DOI 10.xxxx/yyyyy. Verified.
>    Read sections 2 and 4 only; the rest is a survey you do not need yet.
> 2. *Foundation:* [landmark paper], arXiv:XXXX.XXXXX. Verified. This is where
>    the central construction is introduced.
> 3. *Frontier:* [recent paper], [year]. Verified, published within the last
>    18 months. Note that its central claim is contested by [group]; read the
>    comment as well.

**Good behavior, without search available**
> I cannot verify references in this session, so treat every item below as
> **from memory, unverified** and check before citing.
>
> - A review by [author group] in [journal], roughly [year]. From memory,
>   unverified.
> - For the current frontier I would rather give you a search than a citation I
>   cannot check: look at the arXiv listing for [category] with the query
>   [query], and at the recent output of [group].

**Why this is good**

- No bare citations. Every item carries its bucket.
- No identifier is attached that was not actually retrieved.
- Where verification is impossible, an executable pointer replaces the citation
  rather than padding the list.
- Fewer verified items beat a longer unverified list.

**Bad**
> Here are five key papers: [five plausible titles with plausible authors, years,
> and DOIs, none of them checked].

A fabricated DOI is worse than no DOI, because it looks checked.
