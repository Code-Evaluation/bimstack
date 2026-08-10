# GOV.BB design guide

The prose companion to `govbb-design-tokens.yaml`. This file explains how the GOV.BB design system is organised, the decisions behind it, and the rules for working in it. The exact values – every hex, token, size, and status – live in the YAML. This file never repeats them, so the two cannot drift against each other.

Anchored to Standard 3 (everyone can use the service), Standard 5 (works the first time), and Standard 7 (open, common, interoperable platforms).

**Precedence.** Figma is the source of truth. If anything here or in the YAML drifts from the live Pattern Library, trust the live Figma file.

---

## How these two files work together

We keep the design system reference in two files on purpose.

- **`govbb-design-guide.md`** (this file) holds the narrative: how each layer is built, why the decisions were made, what is still open, and the rules for reading Figma safely. It carries no exact values.
- **`govbb-design-tokens.yaml`** holds the structured values: the tokens, the typography scale, the component inventories, the scope guard, the accessibility issues, and the confirmed cross-check findings. Skills read it directly.

If you need to know *how* colour works, read this file. If you need to know *what* `Text/text-link` resolves to, read the YAML. Keeping values out of the prose means an update to a hex changes one file, not two, and the two can never disagree.

---

## Keeping these files true

Both files are a curated snapshot. Figma is the origin. That gap is where drift hides, so we manage it deliberately.

- **Facts get re-read, never remembered.** Token values and component inventories drift the moment someone edits Figma. Refresh them by reading Figma again – the `component-spec` skill is how we do that – not from memory or assumption.
- **Decisions get changed deliberately.** The judgements Figma cannot express – what is deferred, what is a known issue, how the scope guard classifies a component – are human-owned. Change them with a reason recorded, not in passing.
- **A value that looks wrong may be a deliberate edit, not corruption.** Several tokens in the YAML (`text-tertiary`, `text-error`, `text-success`, `border-default`, `border-strong`) carry a note that they were set intentionally by the design lead and should not be "corrected" back to an assumed value without asking first. This applies more broadly than those five: if a value in Figma does not match what this guide or the YAML says, that is a signal to ask, not to fix unilaterally. This applies doubly after any bulk operation on the primitive collection, which can silently break a downstream alias without changing its name.
- **Drift gets caught by the skills.** The cross-check skill compares Figma against the code and tells us when a value has gone stale. The audit skill runs that check on a schedule.
- **Updates go through a pull request.** These are bimstack references. They change in the open, anchored to a Standard, in house style. Every refresh bumps `last_verified` in the YAML, so a reader can always see how old the snapshot is.

---

## Figma files

Two files, both listed with their keys in the YAML under `figma_files`.

- The **Pattern Library** is the source of truth for all tokens and components.
- The **Alpha** file is the live product. It consumes the Pattern Library as a linked library, so a change in the Pattern Library reaches Alpha, and a variable that looks unused in the Pattern Library may have thousands of uses in Alpha.

---

## Tech stack

- **Framework.** Next.js 15, React 19, TypeScript, with Astro for the site and docs.
- **Styling.** A CSS-first pipeline. The CSS package is the single source of truth for styling.
- **Packages.** `@govtech-bb/frontend` holds the canonical CSS. `@govtech-bb/react` holds thin React wrappers built with `cva` and ships no CSS of its own.
- **Class prefix.** `govbb-`, for example `govbb-btn`, `govbb-form-group`, `govbb-radio`.
- **Repo.** `github.com/govtech-bb/govbb-design-system` is the current source of truth, confirmed directly with Aaron. A published `llms.txt` sits at the repo's old name (`design-system`) and predates the rename. Treat it as historical, and check whether `govbb-design-system` publishes its own before relying on it.

Do not introduce Tailwind for new components. Do not reach for one-off colours. Use CSS variables, not hex values directly.

---

## How colour works

Three layers, all detailed in the YAML under `colour`.

- **Primitives** (the `Colour` collection). Each hue runs on an ordered scale, lightest to darkest, with stop 40 as the anchor (the brand or pure colour) and the higher stops as hover and pressed intensities. Not every hue carries every stop – the gaps are intentional. The grey ramp is a full eight-stop scale; black and white are absolute endpoints outside it. All 52 primitives are grouped into slash-prefixed folders (`Teal/`, `Grey/`, `Neutral/`, and so on) and arranged in ascending order within each group, so the Variables panel reads cleanly rather than needing the layers panel to confirm what exists.
- **Semantic** (the `Tokens` collection). Roles for text, surface, border, status, and focus. Every semantic token is an alias into a primitive, never a raw hex. When you document one, report both the token and the primitive underneath. Light mode only for now. A newer token, `Surface/icon-default`, exists specifically for icon-on-surface fills (for example the Select chevron) so that icon backgrounds are not silently coupled to `surface-secondary`'s value.
- **Component** (also in `Tokens`, grouped by component). Scoped to one component's parts and states, for example everything under `Button/`, `Link/`, or the form-control atoms.

The full primitive hex table is not yet captured in the YAML – only the stops referenced by a token or an accessibility note. Read the rest from Figma before relying on them, and record what you read.

---

## How typography works

The `Typography` collection holds the type scale: eight roles across three modes (Desktop, Tablet, Mobile). Full sizes and weights are in the YAML under `typography`.

The font is Figtree. Set the face explicitly so it does not fall back to Calibri or a system font. Real Figma text styles exist for each role (`Body-regular`, `H1`, and so on, plus two styles marked "do not use" left over from an earlier pass) and bundle font, size, and line-height together. Apply the actual text style, not a loose `fontSize` variable binding – binding only the size leaves line-height unmanaged, and it may still carry a stale percentage-based value from a legacy style. `Body-regular` is now applied this way on Button, Atom/Label, and Link. Heading and Text components are not yet wired to the scale. For web pages, the design system handles typography – do not override it.

---

## How spacing works

The `Space` collection holds seven values, detailed in the YAML under `spacing`. Corner radius and the common vertical gaps are now bound where they cleanly matched an existing value – see the YAML's `wiring_status` note. Not everything should be forced into the scale: Radio's fully-rounded corner (roughly half its own width, to read as a circle) is a different convention from a linear spacing value and is left unbound on purpose. A naming inconsistency between `xm` and the rest of the scale is still unresolved.

---

## Components

Before you answer any question about a component, decide which of three states it is in. They are not the same, and the YAML `scope_guard` records the state for each.

- **Tokenised** – present in Figma and bound to the Colour or Tokens collection.
- **Exists, not tokenised** – present in Figma but with no bindings yet. Document the anatomy and states, but say plainly that no tokens are bound.
- **No confirmed Figma presence** – exists in the GitHub code but not found in Figma. Do not write anatomy, states, or tokens for it. A React component is not proof of a Figma one. Say no presence is confirmed, and offer to check.

That third state is where hallucination happens. The YAML lists which components fall in each state; trust it over any assumption, and re-scan if time has passed.

### Button

Six kinds – Primary, Secondary, Tertiary, Text, Text-Negative, Negative – each with four states: Default, Hover, Pressed, Focus. Disabled is 25% opacity on the base colour, not a separate token. The Alternate property gives an inverse-context treatment for dark backgrounds, with its own tokens under `Button/Alternate/`. The focus state uses a drop-shadow ring baked into the component, not bound to a variable, so it needs no separate token. Fully tokenised. All values are in the YAML.

### Link

Two contexts – Light and Dark – each with five states: Default, Hover, Active, Focus, Visited. Active and Focus look identical in both contexts. Focus is the background treatment only, with no separate drop-shadow ring, unlike Button. Fully tokenised. All values are in the YAML.

### The form-control atoms – Radio, Checkbox, Input, Select

Radio and Checkbox share one architecture: an `Atom/` component holding the full interaction matrix (Selected/Unselected across Default, Hover, Focus, and Error), and a `Form/` wrapper that nests an instance of the atom rather than duplicating its shapes. This is a sound two-layer pattern, but it was found half-wired: the `Form/` wrappers had only two variants each and their nested atom instance was hardcoded to Default, meaning a radio or checkbox inside a form could never visually show focus or an error. Both were rebuilt to the atom's full state range, with the nested instance correctly swapped per state.

Input follows the same shape: `Atom/Input` carries six states (Default, Hover, Focus, Typing, Done, Error) across two types (Single line, Multi-line), and `Form/Component/Input` nests it alongside a label. By design, the input shows no placeholder text in any state except Typing and Done – there is deliberately nothing to align in the other states, and this is not a gap to fill.

Select did not originally follow this pattern at all: `Atom/Select` was a single static component with its own independent box, overlapping an arrow icon on top, rather than nesting `Atom/Input`. It has been rebuilt to nest a correctly state-matched `Atom/Input` instance beside a fixed 62×62 arrow frame, using auto-layout rather than an overlap, with the input's right-edge corner radius set to 0 so the two elements read as one control. `Input/Select` is the composed version with a label, mirroring `Form/Component/Input`.

All four atoms are fully tokenised. Anatomy, state counts, and token names are in the YAML.

### Atom/Label

The shared label pattern used by every form control: a Label line, a Description (hint) line, and – on the Error variant only – a separate Error message line, styled and positioned as its own element rather than repurposing the Description slot. Any component's error state should use this component's Error variant directly, not a bespoke error text of its own.

### Date and File upload

Date composes three `Form/Component/Input` instances (day, month, year) under one label. Its error state shows one combined message at the wrapper level – not one per field, which would be repetitive and does not match how a date's validity is actually judged.

File upload originally had a Default and an Uploaded state with no way to show an error, and its label was two raw text layers rather than a real component. Both are fixed: an Error state now exists across both sizes, and the label uses `Atom/Label` with real copy ("Upload a file" / "Attach a .pdf, .docx, or .png file"), so it participates in the same token wiring as every other label rather than sitting outside it.

### What still has no confirmed Figma presence

Several components named in the GitHub code library have not been found anywhere in Figma: Breadcrumbs, ErrorMessage, FooterLink, FormGroup, Hint, LinkButton, SkipLink, SummaryList, and Heading/Text as their own components. Character count and Password input do not exist in either place. Textarea is not missing – `Atom/Input`'s Multi-line type covers it. Do not invent anatomy or token bindings for anything in this list; say plainly that no Figma presence is confirmed.

---

## Accessibility

The audit found issues, all recorded in the YAML under `accessibility_issues`. Every one is tracked and none is a bug to fix now. This matters for review work: a failing contrast value that appears in that list is a decided, deferred issue, not a defect to flag. Report it with its status; do not silently correct it. The clearest example is the text-button hover and pressed colours, tracked in GitHub DS issue #149.

One issue – forced colours in Windows High Contrast, affecting the checkbox and radio marks, the back-button arrow, and the select chevron – sits outside the token system and needs a CSS fix.

---

## Scope guard

The scope guard is how we answer "is this tokenised?", "does this exist in Figma?", and "is this in the GitHub code?" without guessing. Read it before generating any token reference, component spec, or cross-check output. Its three states, above, must never be collapsed into each other – "not tokenised" and "not in Figma" are different answers to different questions. The full classification lives in the YAML under `scope_guard`.

---

## Cross-check findings

We ran a real comparison of the Figma tokens against `packages/frontend/src/tokens.css`, read directly from the repo. The full findings are in the YAML under `cross_check_findings`. The headlines:

- **The CSS is still on the old naming scale.** The rename to the ordered scale has not been carried into code. The YAML records the value mapping from each old CSS name to its Figma name.
- **One likely Figma-side error (CC-01).** The general keyboard focus ring is teal in the code but points to yellow in Figma's `Focus/focus-ring`. These should be the same thing. The gold link-focus surface is correctly mapped; `Focus/focus-ring` is the one that looks wrong. Not yet corrected – flag before building on it.
- **One never-applied fix (CC-02).** The broken on-dark Link hover colour that was meant to be corrected during the Link merge is still live in the code, now formalised as its own primitive. Flag for correction; it is not an intentional distinct colour.
- **One real conflict (CC-04).** Disabled opacity is 40% in the CSS and 25% in the recorded Button decision. Confirm which is correct before either side changes.

The rest are naming-only matches, scoping gaps, and one intentional divergence in how state overlays are built. Confirmed matches that need no action are listed too, so a future check does not re-open them.

---

## External ecosystem

Sources beyond Figma and the CSS, found while cross-referencing bimstack. Relevant to any content, IA, or handoff work these skills touch.

- **Barbados Digital Toolkit** (`barbados-digital-toolkit.alpha.gov.bb`) – the official hub for designing and building government digital services. Confirmed live.
- **"Writing content for Barbadians"** (`barbados-digital-toolkit.alpha.gov.bb/design-and-build/writing-content-for-barbadians.html`) – a live plain-language content guide with its own word-swap list and sentence-length rules. It overlaps but is not identical to bimstack's `house-style.md`: each carries swaps the other does not. If these skills are ever asked to review or generate citizen-facing copy, check both. Neither has been confirmed as authoritative over the other.
- **Design System Storybook** (`govtech-bb.github.io/design-system`) – live but stale, sitting at the old pre-rename repo name. `govbb-design-system` is the real source of truth.
- **A Google Doc** was shared as a possible source on content rules. Access returned a 401, so its contents are unconfirmed. If revisited, ask for comment access or a direct export.

**Two structural gaps, unresolved.** `house-style.md` claims to be maintained across four `govtech-barbados-*` skills (`-services`, `-forms`, `-presentations`, `-qr-codes`). None of them exist in the bimstack package as of v0.2.0. Whether they live elsewhere is unconfirmed. This is the same open question as the content-rules source: two references to the same rules will drift the moment one is updated and the other is not. Ask Abisola or Amoge before building content-rule logic anywhere.

---

## What is still open

- The `Focus/focus-ring` mapping (CC-01) needs a person to confirm and correct in Figma.
- The disabled-opacity conflict (CC-04) needs a decision on 25% versus 40%.
- The content-rules source and the four missing `govtech-barbados-*` skills need Abisola or Amoge to answer.
- Spacing's `xm` naming inconsistency is unresolved. Dark mode and inverse-context tokens are parked.
- The full 52-variable primitive hex table is not yet captured in the YAML.
- Header, Footer, Payment, and ShowHide exist in Figma but are not yet tokenised. Table exists only as a plain frame, not a real component.

---

## Operating rules

These encode hard-won lessons from the design system build. Follow them exactly.

1. **Never trust a same-call read-back.** A script that writes and then reads back in the same call is not verification. Always use a separate, later tool call to confirm any write.
2. **Use `for...of` with `await`, never an async callback in `.find()` or `.filter()`.** `array.find(async ...)` always returns the first element, because the async callback returns a truthy Promise whatever the match.
3. **Keep writes small.** The last operation in a long script is the one most likely to fail silently. Aim for four to six operations per call, then verify before continuing.
4. **Discover structure before scanning.** On a large file, run a structure-discovery call first, decide what to scan from the real numbers, then scan narrowly. A combined discover-and-scan call truncates on budget.
5. **Check before creating or renaming.** Inspect the actual current state in the file before any create or rename, even when the plan seems obviously correct.
6. **Check usage before deleting.** A variable that looks unused in the Pattern Library may have thousands of uses in the linked Alpha file. Check cross-file usage before deleting anything, and prefer a clear "legacy, do not use" rename over deletion.
7. **Convert node IDs.** Share URLs use dashes (`123-456`). The Figma tools require colons (`123:456`). Convert before every tool call.
8. **A value that looks wrong may be a deliberate edit, not corruption.** If a token, colour, or property does not match what this guide or the YAML documents, that is a signal to ask, not to fix unilaterally – the value may have been changed on purpose in Figma since the last read, and this file may simply be stale. This applies doubly after any bulk operation on a primitive collection, which can silently break a downstream alias without changing its name. Re-verify what actually changed before assuming why.
9. **Default to auto-layout when creating any component or frame**, unless told otherwise or there is a clear structural reason not to, such as an intentionally overlapping icon treatment. When replacing or moving a child within an auto-layout frame, insert it at the correct index rather than appending it – appending puts it last, which silently reverses visual order in a horizontal or vertical layout. Check the actual child order after the operation, not just that it succeeded.
10. **Binding a variable to a property on a main component can silently reset per-instance overrides on that same property**, even on unrelated instances elsewhere in the file, such as a corner-radius override used to make two components sit flush against each other. After binding any property at the base-component level, re-check every known instance override of that property and reapply it if needed.
11. **For typography, apply the real Figma text style, not a loose `fontSize` binding.** A text style bundles font, size, and line-height together as one unit. Binding only the size leaves line-height unmanaged, and it may still carry a stale percentage-based value from an old style. Check for an existing text style before doing anything more manual.
