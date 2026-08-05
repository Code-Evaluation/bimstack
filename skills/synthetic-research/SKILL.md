---
name: synthetic-research
description: >-
  Generate synthetic personas grounded in real population data and run them
  against a bimstack prototype to surface comprehension, logic, and edge-case
  failures before real user testing – raising the floor, never replacing it.
  Use when a prototype is ready for a pre-flight check, when the team wants to
  stress-test a form before recruiting participants, or when a phase gate is
  approaching and the team needs confidence that the obvious failures are already
  caught. Complements real research – it never substitutes for it. Triggers on
  "/bimstack:synthetic", "synthetic personas", "synthetic research", "test this
  form automatically", "pre-flight check", "robot user testing", "who might
  struggle with this form", "run the personas against it".
---

# Synthetic research

Generate synthetic personas for a government service and run automated user research against a bimstack prototype, producing a friction report the team can act on before real testing begins.

This skill raises the floor. It catches the comprehension failures, logic gaps, and edge-case exclusions that real participants shouldn't have to discover. It cannot tell you what real Barbadians feel, fear, or misunderstand in ways nobody predicted – only real research does that.

It supports Barbados Digital Service Standards 1 (meet user needs – by stress-testing assumptions before they reach users), 3 (everyone can use it – by deliberately over-representing awkward cases), and 10 (continuously improved – by making pre-flight checks cheap enough to run every round).

For the larger workflow this sits inside, read `PLAYBOOK.md` – the *Rapid prototyping loop* section. The natural flow is:

```
/bimstack:build  →  /bimstack:synthetic  →  fix blockers  →  real user testing  →  /bimstack:iterate
```

The `synthetic-findings.md` this skill produces can be fed directly to `bimstack:prototype-iteration` as structured feedback (Path B), so the loop closes without extra glue.

---

## When to use

- After `/bimstack:build` produces prototypes and before the first round of real user testing
- Before a show-and-tell or MDA review, to catch embarrassing failures early
- Before a phase gate (`/bimstack:assess`), as a deeper pre-flight
- When the team has iterated a prototype and wants a quick check before the next testing round
- When recruiting real participants will take time and the team wants to use the gap productively

**Do not use this skill to avoid real user testing.** Synthetic personas find what a careful desk review would find, faster and more systematically. They do not find what only a real citizen in a real situation can show you. Standard 1 requires real research. This skill makes real research more productive by clearing the obvious failures first.

---

## The standing disclaimer

Every artefact this skill produces – the persona pack, the friction report, any intermediate output – must open with this disclaimer or a close paraphrase:

> **Synthetic research disclaimer.** These findings are from synthetic personas, not real users. They catch comprehension, logic, and edge-case failures cheaply and early. They cannot tell you what real Barbadians feel, fear, or misunderstand in ways nobody predicted. This report raises the floor before real research – it never replaces it.

This is non-negotiable. The skill must never present synthetic quotes, observations, or findings as if they came from real participants. Label everything synthetic. Use "the synthetic persona would…" or "Marcia (synthetic) would likely…" – never "the user said" or "participants reported".

---

## Three stages

Each stage produces a reviewable artefact before the next runs. The team can stop after any stage.

---

### Stage 1 – Persona pack

**Input:** the prototype's brief, its assumptions panel, and any service context the team provides – who the service is for, what triggers use, what's at stake, known population data (census, digital-inclusion stats, contact-centre themes, prior research).

**Output:** `prototype-N-name/synthetic-round-K/personas.md`

Generate 5–8 synthetic personas following the schema in `references/persona-schema.md`. Read that file before generating any persona.

**Rules:**

1. **Ground in data.** Every persona field should cite real data where available. Where no data exists, mark the attribute `[ASSUMPTION – VERIFY WITH USERS]` (or the appropriate tag) using the same convention as the prototype assumptions panel.

2. **Over-represent awkward cases.** The mandatory edge-case personas from the schema are non-negotiable – every pack includes at minimum:
   - A mobile-only user with low digital confidence
   - A screen-reader or other assistive-technology user
   - Someone missing the ID document or reference number the form assumes
   - Someone whose circumstances don't fit the happy path (no fixed address, recent name change, irregular income – pick what's relevant to the service)

3. **Make them specific to this form.** The "how their situation stresses this form" field is the most important one. A persona that could apply to any government service is a persona that will find nothing specific.

4. **Don't generate comfortable professionals.** If the service population includes comfortable professionals, one persona is enough. The rest should represent the people most likely to struggle.

**Checkpoint:** present the persona pack to the team. Ask: "Do these personas cover the population you're worried about? Is anyone missing? Should any grounding data be updated?" Wait for confirmation before Stage 2.

---

### Stage 2 – Testing

Two modes. Mode A always runs. Mode B runs when asked or when the prototype is approaching a phase gate.

#### Mode A – Persona-lens critique (fast, always run)

Each persona "reads" every page of the prototype and flags:

- **Questions they wouldn't understand** – jargon, civil-service register, reading-age failures. Cross-reference the `bimstack:plain-language-check` word-swap list where relevant.
- **Answer options that don't fit them** – radio buttons with no matching option, dropdowns that assume a standard situation, checkboxes that force a false choice.
- **Assumptions that exclude them** – the form assumes a document they don't have, a device they don't use, a living situation that doesn't match theirs.
- **Points where they'd abandon** – where anxiety, confusion, or frustration would cause them to close the tab. Be specific about why.

Then run the **question protocol** (read `references/question-protocol.md`) against every question on every page:

1. Who uses this answer?
2. What do they use it for?
3. What happens if it's blank?
4. What happens if it's filled in with any old thing?

Flag any question with no clear consumer (`QP-FAIL`), a vague purpose (`QP-WARN`), a false mandatory (`QP-FAIL`), unvalidated input (`QP-WARN`), format assumptions (`QP-WARN`), or excluding options (`QP-FAIL`).

**Output:** working notes per persona, saved as `prototype-N-name/synthetic-round-K/mode-a-notes.md`. These feed Stage 3.

#### Mode B – Agentic walkthrough (deeper, run on request or before a phase gate)

For each persona, attempt the prototype end-to-end as that persona.

**How to walk the prototype:**

1. Read the prototype HTML. Identify the page structure – the `goTo()` navigation, the form fields, the validation logic, the branching.
2. For each persona, simulate completing the form the way that persona would: choosing the answers their circumstances dictate, using the device and input patterns their profile describes, hitting the edges their situation creates.
3. For each persona, also run at least one **deliberately sloppy attempt**: wrong date formats, skipped required fields, back-button navigation mid-flow, pasting into fields that expect typed input, submitting with no changes on a page. This tests validation messages, error recovery, and whether the form handles the back button gracefully.

**Record for each walkthrough:**

- The path taken (page sequence, which branches hit)
- Every error or validation message encountered
- Where the persona got stuck or stranded (couldn't proceed, couldn't go back, didn't know what to do)
- Where the persona would have abandoned (with the reason)
- Whether the sloppy run produced helpful error messages or confusing ones

**Output:** walkthrough logs per persona, saved as `prototype-N-name/synthetic-round-K/mode-b-walkthroughs.md`. These feed Stage 3.

---

### Stage 3 – The report

**Output:** `prototype-N-name/synthetic-round-K/synthetic-findings.md`

Structured as follows:

#### 1. Disclaimer

The standing disclaimer (see above). Always first. Always present.

#### 2. Friction log per persona

For each persona, a table or list of every friction point found in Stages 2A and 2B:

| Page | What stopped them | What they misread or misunderstood | Severity |
|---|---|---|---|
| Page name | Specific friction | Specific comprehension failure | blocker / major / minor |

Severity definitions:

- **Blocker** – the persona cannot complete the form. They are stuck, stranded, or excluded. The form is broken for this person.
- **Major** – the persona can probably complete the form but would likely get it wrong, abandon out of frustration, or need help. The form is hard for this person.
- **Minor** – the persona notices something odd, hesitates, or is mildly confused, but can proceed. The form is rough for this person.

#### 3. Cross-cutting themes

Themes that appear across multiple personas, ranked by severity. For each theme:

- **What the theme is** – one sentence.
- **Which personas it affects** – names and why.
- **Severity** – blocker / major / minor, based on the most severe instance.
- **Suggested fix** – a specific rewrite, design change, or structural change. For copy rewrites, reference `bimstack:plain-language-check`. For interaction changes, reference the GovBB design system patterns.
- **Standard** – the Barbados Digital Service Standard this theme pushes against (1, 3, 4, 5, etc.).

#### 4. Question-protocol failures

Listed separately from the persona friction, because these are structural issues with the form itself – not persona-specific:

| Page | Question | Protocol failure | Severity | Recommendation |
|---|---|---|---|---|
| Page name | Field or question | QP-FAIL or QP-WARN with detail | blocker / major / minor | Specific action |

#### 5. "Test this with real people"

The most important section. For each persona that surfaced significant friction (major or blocker), recommend:

- **Why this persona matters** – the real population segment they represent.
- **What to test** – the specific friction points that synthetic research flagged but can't resolve. (Does a real citizen in this situation actually abandon? Or do they muddle through?)
- **Recruitment suggestion** – how to find real participants matching this persona's key characteristics. Be specific to Barbados: which parish, which community group, which MDA contact list, which assisted-digital channel.

This section exists to make the handoff to real research concrete. Don't end with "test with real users" – end with "recruit two mobile-only citizens from [specific channel] and test pages 3–5 specifically".

---

## What this skill does not do

- **It does not replace user testing.** Say it again: synthetic personas find desk-review failures systematically. They do not find what real people find. The disclaimer exists because this distinction matters.
- **It does not generate quotes.** Synthetic personas do not "say" things. They "would likely struggle with" or "cannot answer" things. The language matters – the team must never mistake a synthetic observation for a real participant's words.
- **It does not assess the service design.** It tests the form as built. If the underlying service model is wrong – the wrong process, the wrong eligibility criteria, the wrong channel – synthetic personas won't catch that. Real discovery does.
- **It does not test emotional responses.** A synthetic persona can flag that a question about income is anxiety-inducing for people in financial difficulty. It cannot tell you *how* anxious, or what that anxiety makes someone do. Only a real participant can.
- **It does not sign off the prototype.** A clean synthetic-research report means the obvious failures are cleared. It does not mean the prototype is ready for beta. Standard 1 still requires real research.

---

## File structure

```
prototype-N-name/
├── synthetic-round-K/
│   ├── personas.md              ← Stage 1 output
│   ├── mode-a-notes.md          ← Stage 2A working notes
│   ├── mode-b-walkthroughs.md   ← Stage 2B walkthrough logs (when run)
│   └── synthetic-findings.md    ← Stage 3 report
```

Round numbering (`K`) increments with each run of the skill against the same prototype – if the team fixes blockers and runs again, the second run is `synthetic-round-2`. Prior rounds are kept so the team can see what improved.

---

## Bundled files

| File | When to read |
|---|---|
| `references/persona-schema.md` | Before generating any persona (Stage 1) – the template, grounding-data guidance, mandatory edge cases, and a worked example |
| `references/question-protocol.md` | Before running Mode A (Stage 2) – the four-question protocol, additional checks, and aggregation rules |

---

## Integration with bimstack

| Skill | Relationship |
|---|---|
| `bimstack:brief-to-prototypes` | Upstream – produces the prototypes this skill tests |
| `bimstack:prototype-iteration` | Downstream – `synthetic-findings.md` feeds in as Path B structured feedback |
| `bimstack:plain-language-check` | Referenced in Mode A and in suggested rewrites |
| `bimstack:service-standard-assessment` | The friction report cites standards; the assessment can reference synthetic rounds as evidence of pre-flight diligence |
| `bimstack:research-planning` | Downstream – the "Test this with real people" section feeds recruitment and session planning for real research |

---

## Citing standards

Every synthetic-findings report cites the standards that each theme pushes against. Examples:

> Standard 3 (everyone can use it) – Marcia (synthetic) cannot complete the form without a smartphone; no assisted-digital path exists.
> Standard 4 (simple, relatable language) – three personas flagged "pursuant to" on page 2; the word is on the swap list.
> Standard 5 (works first time) – the back button on page 4 loses entered data; four of six personas would have to re-enter their CPD details.
> Standard 1 (meet user needs) – QP-FAIL on the "practice type" question: no identified consumer for this data.
