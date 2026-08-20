---
name: component-spec
description: Read a component or token layer from the GOV.BB Pattern Library in Figma and write an accurate, human-readable spec – anatomy, states, properties, and the tokens each part is bound to. Use when documenting what the design system actually contains, before building or reviewing against it. Triggers on "document this component", "component spec", "what tokens does the button use", "token reference", "what states does this have", "read this Figma component", "is this tokenised", "state inventory", "GovBB design system".
---

# Component Spec

This skill reads a component or a token layer from the GOV.BB Pattern Library in Figma and writes a spec you can trust. It produces a documented reference – anatomy, states, properties, and the exact token each part is bound to – not a loose description.

It anchors to Standard 3 (everyone can use the service): the design system is how we keep every service accessible and consistent, so an accurate account of what it contains is the floor everything else builds on. It also serves Standard 7 (open, common, interoperable platforms): you can only reuse what already exists if you can see it clearly, so reading the system accurately is the first step to not rebuilding it.

The design system reference lives in two files. `references/govbb-design-tokens.yaml` holds the exact values, the scope guard, and the decided-versus-defect status – read it for what should be true. `references/govbb-design-guide.md` holds the narrative and the decisions behind it. Read them before you read Figma – they tell you what should be there, so you can tell drift from fact. When the YAML disagrees with live Figma, trust Figma.

---

## When to use this skill

- Documenting a component so a developer or designer can build against it
- Writing a token reference for the colour, typography, or semantic layers
- Answering "what does this component use?" or "what states does it have?"
- Checking whether something is tokenised, present-but-untokenised, or not in Figma at all
- Producing a state inventory before a design review or a cross-check
- Onboarding someone to the Pattern Library

If the request is "does Figma match the code?", that is a different job. Use the cross-check skill, not this one. This skill reads one side – Figma – and reports what is there.

---

## Read Figma safely – these rules are not optional

The design system build that produced our references hit the same failures again and again. These rules encode what we learnt. Follow them exactly.

1. **Convert node IDs first.** Share URLs use dashes (`123-456`). The Figma tools need colons (`123:456`). Convert before every call.
2. **Discover structure, then scan.** On any large file, run a structure-discovery call first. Decide what is worth reading from the real numbers. Then read narrowly. A combined discover-and-scan call truncates on budget and reports half a picture as if it were whole.
3. **Never trust a same-call read-back.** A call that reads a value and reports it once is a single reading, not a confirmation. If a value matters, read it in a separate, later call before you write it into a spec.
4. **Keep reads small.** The last operation in a long call is the one most likely to come back empty. Prefer several small reads over one wide sweep.
5. **Check the scope guard before you write anything.** Classify what you are looking at (see below) before you describe it. This is the difference between documenting and hallucinating.
6. **Search reads the published library; a node read reads the live file.** `search_design_system` returns the published snapshot, which can lag the working file – sometimes by a whole rename. It also does not reliably report a component's type, variant count, or bindings. Use it to locate a component, then read the node itself for what is there now: `get_metadata` on a page or component node returns the live subtree, and `get_variable_defs` returns the live bindings. The no-arg page listing is unreliable – it may return only the Cover page – so reach a page by its node ID, from a share URL or by walking a node you already know.

If a value looks strange, check a concrete real example before theorising. A "compound" ID is usually a trailing comma, not a mystery. The mundane explanation is almost always the right one.

---

## The scope guard – classify before you document

`references/govbb-design-tokens.yaml` holds the scope guard under `scope_guard`. Three states matter, and they are not the same. Say which one applies before you write a single line of spec.

- **Tokenised** – present in Figma and bound to the Colour or Tokens collection. You can document the bindings.
- **Exists, not tokenised** – present in Figma but with no token bindings yet. Document the anatomy and states, but say plainly that no tokens are bound. Do not invent bindings.
- **No confirmed Figma presence** – exists in the GitHub design system code but has not been found in Figma. Do not write anatomy, states, or tokens for it. Say no Figma presence is confirmed, and offer to check directly.

The third state is where hallucination happens. A component existing in the React package is not evidence it exists in Figma. If the YAML lists it as unconfirmed – Breadcrumbs, Hint, FormGroup, SummaryList, and others – treat it as unconfirmed until you have looked.

The YAML `scope_guard` holds the current classification – read it rather than working from memory, because the tokenised list grows as components are built out. At the last read, the colour primitives, the semantic layer, and the typography scale were built; Button, Link, and the form-control atoms (Radio, Checkbox, Input, Select, Label, Date, and File upload) were tokenised; Header, Footer, Payment, and ShowHide existed but were not; and several components named in the code had no confirmed Figma presence. Re-scan before trusting any of this if time has passed – both Figma and the code drift.

---

## What the skill reads

### Tokens

Three layers, in order:

- **Primitives** – the `Colour` collection. Raw hue-stop values on the 10 / 20 / 40 / 60 / 80 / 90 scale, plus the grey ramp and the neutral endpoints. Stop 40 is the anchor.
- **Semantic** – the `Tokens` collection. Text, Surface, Border, Status, and Focus roles. Every one is an alias into a primitive, never raw hex. Report both the token and the primitive it points to.
- **Component** – also in `Tokens`, under a component group (for example `Button/`, `Link/`). Scoped to one component's parts and states.

For typography, read the `Typography` collection: 8 roles across Desktop, Tablet, and Mobile modes. For spacing, read the `Space` collection: it is now named (`xxs` to `xl`) and partly wired to components, but still provisional – a naming inconsistency is unresolved and some stops are missing.

### Components

For a component, read and report:

- **Anatomy** – the parts, and the property set (for example Button's Type, State, Disabled, Negative, Alternate).
- **States** – every state, with the token or value each part takes in it.
- **Bindings** – for each part and state, the semantic or component token it is bound to, and the primitive underneath.
- **Effects** – focus rings, shadows, and opacity rules that are baked into the component rather than bound to a variable. Say when a value is baked in, not bound.

---

## Output templates

Write the spec in the simplest words you can, for a reader who did not build the Figma file – if it only makes sense to the person who named the layers, it is not finished.

### Component spec

```markdown
# Component spec – [component name]

**Source:** GOV.BB Pattern Library (Figma), file Rexszlh17fXo0XAxO75Mq5
**Read on:** YYYY-MM-DD
**Scope-guard state:** Tokenised / Exists, not tokenised / No confirmed Figma presence

## Anatomy

[The parts and the property set. One line each.]

## States and bindings

| Kind / variant | State | Part | Token | Primitive | Hex |
|---|---|---|---|---|---|
| Primary | Default | fill | Button/Primary/fill | Teal/teal-80 | #0E5F64 |
| Primary | Hover | fill | Button/Primary/fill-hover | Teal/teal-60 | #1A777D |

## Effects baked into the component

[Focus rings, shadows, opacity rules not bound to a variable. Say so clearly.]

## Known deferred issues – do not flag as bugs

[Anything the YAML records as tracked, with its issue number. These are decided, not defects.]

## What is not confirmed

[Anything you could not read, or that needs a separate verification call. Be honest about the gaps.]
```

### Token reference

```markdown
# Token reference – [layer name]

**Source:** GOV.BB Pattern Library (Figma), file Rexszlh17fXo0XAxO75Mq5
**Read on:** YYYY-MM-DD

## [Group, for example Text]

| Token | Primitive | Hex | Notes |
|---|---|---|---|
| Text/text-primary | Neutral/black-00 | #000000 | |
| Text/text-link | Teal/teal-80 | #0E5F64 | |

## Values that need a note

[Tokens the YAML records as failing a check, with the requirement and the tracked status. Report them; do not silently fix them.]
```

---

## A worked example – Button

**Scope-guard state:** Tokenised. The YAML confirms Button as 39 tokens in the `Tokens` collection.

**Anatomy.** Six kinds – Primary, Secondary, Tertiary, Text, Text-Negative, Negative. Four states per kind – Default, Hover, Pressed, Focus. An Alternate property gives an inverse-context treatment for dark backgrounds. Disabled is 25% opacity on the base colour, not a separate token.

**A slice of the states and bindings:**

| Kind | State | Part | Token | Primitive | Hex |
|---|---|---|---|---|---|
| Primary | Default | fill | Button/Primary/fill | Teal/teal-80 | #0E5F64 |
| Primary | Hover | fill | Button/Primary/fill-hover | Teal/teal-60 | #1A777D |
| Primary | Pressed | fill | Button/Primary/fill-pressed | Teal/teal-90 | #0A4549 |
| Negative | Default | fill | Button/Negative/fill | Red/red-80 | #A42C2C |

**Effects baked in.** The focus state uses a drop-shadow ring in `Teal/teal-40` (#30C0C8), spread 4, blur 0. It is baked into the component, not bound to a variable, so it needs no separate token.

**Known deferred issues – do not flag as bugs.** `button-text-text-hover` (yellow-40) is 1.56:1 on white; `button-text-text-pressed` (yellow-80) is 2.08:1; `button-text-negative-text-hover` (red-40) is 2.78:1. All three fail WCAG AA and all three are tracked in design system issue #149. They are decided, not defects.

---

## What not to do

- **Don't invent bindings for a component with no confirmed Figma presence.** Say it is unconfirmed and offer to check. A React component is not proof of a Figma one.
- **Don't report a same-call read-back as confirmed.** Read it again, separately, before it goes in the spec.
- **Don't fix anything – this skill only reads.** The focus-ring contrast (A1, A2), the text-button contrast (issue #149), and the disabled-opacity question are recorded decisions; report them with their status, do not quietly correct them. More generally, a value that does not match the YAML is a signal to ask, not a defect to fix – it may be a deliberate edit the YAML has not caught up with yet.
- **Don't collapse the three scope-guard states.** "Not tokenised" and "not in Figma" are different answers to different questions. Keep them apart.
- **Don't cite a value you did not read this session.** If you are unsure, read it or say you are unsure. A confident wrong hex is worse than an honest gap.
- **Don't reach past Figma.** This skill reads Figma. If someone asks whether the code agrees, that is the cross-check skill's job.

---

## When this skill isn't enough

- **Figma against code.** If the question is whether Figma and the `govtech-bb/govbb-design-system` CSS agree, use the cross-check skill (Standard 7). This skill reads one side only.
- **Building a page.** If the request is to assemble a page or flow from components, that is the composition skill's job. This skill documents the parts; it does not place them.
- **A value that looks wrong.** If a token reads as an error rather than a decision – something the YAML has not already recorded as tracked – do not spec around it. Flag it to a person. The YAML records at least one live example as finding CC-01: `Focus/focus-ring` points to `Yellow/yellow-40` in Figma, while the code's general focus ring is teal. That is a suspected Figma-side error, not settled design.
