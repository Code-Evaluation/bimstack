---
name: design-review
description: Review a design against the Barbados Digital Service Standards and WCAG 2.1 AA accessibility, and return a triaged verdict – Fix now, Fix later, Confirm with MDA. Use when checking a live HTML prototype or a Figma design for design system conformance and accessibility before it ships or goes to stakeholders. Triggers on "design review", "review this design", "review this prototype", "review this screen", "accessibility check", "does this meet WCAG", "check this against the standards", "is this accessible", "review this page".
---

# Design Review

This skill reviews a design – a live HTML prototype, or a Figma page or component – against the Barbados Digital Service Standards and WCAG 2.1 AA, and returns a triaged verdict. It reviews and reports; it does not change the design.

It anchors to Standard 3 (everyone can use the service): WCAG 2.1 AA is the floor, and building on the design system is how a service stays usable for every Barbadian. It also checks Standard 7 (reuse over rebuild) through component conformance.

It reviews the **visual and accessible** layer specifically. It does not re-check copy – that is `plain-language-check` – and it does not assess the whole service against all thirteen Standards – that is `service-standard-assessment`. It sits between them: the design in front of you, judged against the concrete rules.

Two references ground every judgement. `references/govbb-design-tokens.yaml` holds the agreed values, the `scope_guard`, and the recorded `accessibility_issues`. `references/govbb-design-guide.md` holds the decisions. Read them first – they are what "conformant" means here.

---

## The role this skill plays – judge, do not re-derive

This skill sits on top of the evidence skills. It does not re-read what they already read. It composes their findings and adds the judgement.

- **What the design contains** comes from `component-spec` – the tokens, states, and bindings. Ask it; do not re-read the node yourself.
- **Whether Figma and code agree** comes from `token-cross-check`. Pull its findings where they bear on the review.
- **Whether the copy is plain** is `plain-language-check`'s call, not this skill's.
- **Whether the whole service meets all thirteen Standards** is `service-standard-assessment`'s call.

This skill's own job is the design judgement: does this design use the system correctly, and can everyone use it. Everything else, it defers.

---

## Two surfaces: the live prototype, and foundational Figma

Most GOV.BB testing happens on live HTML prototypes – they are what goes in front of stakeholders and users. Figma is mainly for foundational work. A design lands in one of two places, and the review matches its rigour to the surface.

- **A live HTML prototype or screen** – the common case, and the stronger one for accessibility. Here you measure rather than infer: real contrast from computed styles, real focus order, real semantics and labels, and automated checks (for example axe) over the actual DOM. Conformance means the page uses the design system in code – the `govbb-` classes and the tokens in `tokens.css` – not bespoke CSS. This is usually the higher-value review, because it is the artefact people actually use.
- **A Figma design** – the foundational stage, before code. Here accessibility is inferred from the tokens and the layout, which is weaker than measuring a rendered page, so say so. Conformance means linked instances of real components and semantic tokens, checked against the YAML and the `scope_guard`.

Be honest about one dependency: an HTML review's "it uses the design system" verdict is only as sound as the code design system itself. If `token-cross-check` shows the code has drifted from Figma, say that the verdict inherits that drift.

---

## Preflight: run a drift check first, and report the baseline

Before reviewing anything, run `token-cross-check` and report what it finds. The reviewer needs to know what they are reviewing against: which values in the design are accurate to Figma, and where the code, the published library, and the live file have drifted apart. A clean review against a drifted baseline is false comfort.

State the baseline at the top of the review – which surface the design uses (code or Figma), and any drift that touches what you are about to check. If the design uses a token that is itself drifted or unpublished, that changes what a finding means. Say so rather than reviewing in a vacuum. For a live HTML prototype in particular, "it uses the design system correctly" only means "correct against the code", so the reviewer must know how far the code is from Figma.

---

## The rule that keeps this skill honest: reconcile against decided issues first

Before flagging anything, check it against the recorded `accessibility_issues` and the deferred decisions in the guide. A failing value that is already recorded – the teal focus ring (A1), the danger ring (A2), the text-button contrast (A3, issue #149), the forced-colours marks (A4) – is a **decided, deferred issue, not a new bug**. Report it under Fix later with its tracking reference, never under Fix now as if it were fresh.

The same holds for the five user-set tokens and any value the guide marks as deliberate. A review that re-flags settled decisions as urgent failures trains the team to ignore it. Cry wolf once and the whole review is worth less.

---

## When to use this skill

- Reviewing a Figma page or component before handoff
- Reviewing a rendered screen or prototype before user testing or release
- Running an accessibility pass against WCAG 2.1 AA
- Checking a design uses the system – tokens, components, spacing – rather than bespoke styling

---

## What the review covers

Go through each, and for every finding note the Standard or the WCAG criterion by number, and the evidence.

### Design system conformance (Standard 7)

- **Colour** – every colour resolves to a semantic or component token, not a raw hex. Check against the YAML `colour` layer.
- **Typography** – roles come from the `Typography` scale as real text styles, not loose sizes, and never a legacy or "do not use" style. Check against the YAML `typography`. A legacy text style in a component is a finding.
- **Spacing and grid** – spacing uses the `Space` scale where it applies. Treat it as provisional per the guide.
- **Component reuse** – on Figma, linked instances of real, present components (check the `scope_guard`), not detached copies; on HTML, the `govbb-` classes and DS components, not bespoke CSS or inlined styles. A detached instance or a bespoke reimplementation is a finding.
- **States** – interactive elements show their real states (a form field has its error state, not just default).

### Accessibility (Standard 3, WCAG 2.1 AA)

On a rendered page, run an automated accessibility checker (for example axe) over the DOM as the first pass, then check by hand what automation cannot see. Automation catches roughly a third of WCAG; the manual pass carries the rest – focus order, meaning, alternatives, and whether a control is actually reachable. On a Figma design there is no DOM to run against, so these are inferred from the tokens and layout and are weaker – say which you did.

- **Contrast** – text meets 4.5:1 (WCAG 1.4.3); non-text and focus indicators meet 3:1 (1.4.11). Measure; do not eyeball. Reconcile against A1 to A3 first.
- **Focus visibility** – every interactive element has a visible focus state (2.4.7).
- **Target size** – interactive targets are large enough (2.5.8).
- **Text alternatives** – icons and images that carry meaning have them (1.1.1). The Select chevron and similar icon fills should not be the only signal.
- **Forced colours** – check the A4 pattern (checkbox and radio marks, back-button arrow, select chevron) survives Windows High Contrast. Recorded as deferred – report under Fix later.
- **Meaning not by colour alone** – errors and states are not signalled by colour only (1.4.1).

---

## The output: a triaged verdict

Write the verdict in the simplest words you can, for a reader who did not build the design – if it only makes sense to the person who named the layers, it is not finished.

Every finding lands in exactly one bucket. The bucket is the point – it tells the team what to do next.

- **Fix now** – blocks someone from using the service, and is fixable in the design system now. A new contrast failure on body text, a detached instance, a missing error state, colour as the only signal. Accessibility blockers default here unless already recorded as deferred.
- **Fix later** – real but not blocking, or already tracked and deferred. The recorded issues (A1 to A4, #149) live here with their references. Provisional-spacing nits live here.
- **Confirm with MDA** – not a design call. A content, policy, or domain decision only the Ministry, Department, or Agency can make – a required legal phrase, a mandated field, a term of art. Bring the recommendation and the evidence; do not decide it in the review.

```markdown
# Design review – [page, component, or screen]

**Reviewed:** [what, and the Figma node or screen source]
**Against:** Barbados Digital Service Standards, WCAG 2.1 AA, GOV.BB design system
**Read on:** YYYY-MM-DD
**Evidence from:** component-spec [nodes], token-cross-check [if used]

## Summary

[Two or three sentences. The overall state, and the count in each bucket.]

## What you are reviewing against (drift preflight)

[The surface reviewed (code or Figma), and what token-cross-check found: any drift between the published library, the live file, and the code that touches this design. So the reader knows the baseline before the findings.]

## Fix now

| Finding | Standard / WCAG | Evidence | Recommendation |
|---|---|---|---|

## Fix later

| Finding | Standard / WCAG | Status / reference | Note |
|---|---|---|---|
| Teal focus ring 2.21:1 | WCAG 1.4.11 | Recorded A1, deferred | Not a new bug – tracked |

## Confirm with MDA

| Question | Why it is theirs | Recommendation to bring |
|---|---|---|

## What was not reviewed

[Copy – deferred to plain-language-check. Whole-service conformance – service-standard-assessment. Anything not read this pass.]

## What is working

[Always include this. A review that is all critique reads as if the design were all wrong, which it rarely is.]
```

---

## What not to do

- **Don't re-flag a decided issue as new.** Reconcile against the recorded `accessibility_issues` and deliberate values first. Recorded issues go under Fix later with their reference.
- **Don't duplicate the other skills.** Copy is `plain-language-check`. Whole-service Standards are `service-standard-assessment`. Figma-versus-code drift is `token-cross-check`. Component detail is `component-spec`.
- **Don't claim a WCAG failure you did not measure.** Give the ratio and the criterion. A guessed failure is as damaging as a missed one.
- **Don't fix the design.** This skill reviews. Hand the findings to the person or the composition skill; do not edit.
- **Don't leave a finding un-triaged.** Every finding is Fix now, Fix later, or Confirm with MDA. An untriaged list is not a review.

---

## When this skill isn't enough

- **The copy needs review.** Use `plain-language-check`. This skill does not judge words.
- **The whole service needs assessing.** Use `service-standard-assessment` for all thirteen Standards. This skill reviews a design, not a service.
- **A finding needs the component read in detail.** Use `component-spec` for the exact tokens and states behind a finding.
- **A fix needs building.** Rebuilding from the system is `page-composition`'s job; generating code is `build-for-production`'s. This skill says what is wrong, not fixes it.
- **There is nothing to review yet.** The HTML prototype this skill reviews comes from the prototyping workflow (`brief-to-prototypes`) or `build-for-production`. Review comes after there is something built.
