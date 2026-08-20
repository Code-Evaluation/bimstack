---
name: component-build
description: Create a new component in the GOV.BB Pattern Library in Figma from the design system's tokens – bound to real semantic and component tokens and the current text styles, never raw values or legacy styles. Always proposes a build plan and waits for sign-off before it writes. Use when a component is confirmed missing and needs building. Triggers on "build a component", "create a new component", "add this to the design system", "make the missing component", "build the H1 component", "build FormGroup".
---

# Component Build

This skill creates a new component in the GOV.BB Pattern Library from the design system's own tokens. It builds anatomy, states, and variants, binds every part to the right token, and hands the result to `component-spec` to document and `design-review` to check.

It anchors to Standard 3 (everyone can use the service): new components built correctly on the system keep every service accessible and consistent. It also serves Standard 7 (reuse over rebuild): a real, shared component is the thing other files and the code should reuse, instead of each team building its own.

This skill writes to the Pattern Library – the source of truth that the Alpha file and the code consume. That is why it never writes first. It always proposes a plan and waits.

Two references ground every build. `references/govbb-design-tokens.yaml` gives the `scope_guard` (only build what is confirmed missing), the token names to bind, and the anatomy patterns of sibling components. `references/govbb-design-guide.md` gives the decisions and the conventions.

---

## The plan-first gate – propose, then wait, before any write

This is the rule that defines the skill. **Never create or change anything in Figma before proposing a plan and getting an explicit go-ahead.** Building a component mutates the shared source of truth; an unwanted component is worse than a missing one, because it looks official and gets reused.

The proposal, before any write, states:

- **What** the component is, and **where** it will sit (page and grouping).
- **Anatomy** – the parts, and the property set (the variants: states, types, sizes).
- **Tokens** – for every part and state, the exact semantic or component token it will bind to, the spacing token, and the text style. Name them; do not say "the right colour".
- **Structure** – the auto-layout it will use, and how it nests any existing atoms.
- **What it will not do** – what is out of scope, and anything it cannot bind because the token does not exist.

Then stop. Only on a clear go-ahead do you build. If the plan needs changing, propose the change and wait again.

---

## When to use this skill

- A component the code or a design needs is confirmed missing from Figma (for example the ones the composition dry run flagged: a page H1, a fieldset grouping)
- The team has agreed a new pattern the system should adopt
- An atom exists but its composed wrapper does not, and the wrapper is needed
- `page-composition` scoped a flow, and its component inventory surfaced something missing or needing a new variant, and the user has approved building it

Do not use it to rebuild something that already exists, or to fix an existing component's bindings – that is remediation, not building. And do not use it on a component whose `scope_guard` state is "exists" or "unconfirmed"; check first.

---

## Build from tokens – never raw values, never legacy styles

Every part binds to the system, or it does not ship.

- **Colour** – bind to a semantic or component token (`Text/…`, `Surface/…`, `Border/…`, or a `Button/…`-style component token). Never a raw hex.
- **Spacing and radius** – bind to the `Space` scale where a value matches. Leave a genuinely non-scale value (a fully-rounded radius) unbound on purpose, and say so.
- **Typography** – apply the real current text style (`Body-regular`, `H1`, and so on), which bundles font, size, and line-height. Never bind a loose `fontSize`. **Never apply a legacy or "do not use" text style** – if a sibling component still carries one, that is a bug to flag, not a pattern to copy.
- **States** – check for opacity before trusting any state as tokenised. Opacity is how a disabled state gets faked: a layer dimmed to look disabled has no token behind it. Strip the opacity and let the nested component's own disabled variant do the work.

If a part needs a token that does not exist, do not invent a raw value in its place, and do not reach past the token layer to bind a primitive directly. Where a component needs colours the semantic layer does not cover – a status set, a new component's own parts – **create a component-token group for it first**: a `<Component>/*` group in the Tokens collection, each token aliasing a primitive, exactly as `Button/*` and `Link/*` do. Then bind the component to that group, not to the primitives. This keeps the component one layer above the ramp, so a palette change flows through and the component reads in its own vocabulary (`Status pill/received-border`, not `Teal/teal-80`). Reach for this whenever a new component introduces colours of its own. If even the primitive you would alias is missing, stop and say so.

---

## Extending the system: new tokens and new variants

Sometimes the missing piece is not a whole component but a value or a variant the system does not carry yet. The plan-first gate still applies – propose, then wait – because these change the shared library.

- **A missing scale value.** If a page needs a spacing or radius the scale does not hold, add it as a token, not a raw value. Name it to fit the scale. If the name does not fit cleanly, say so: a strained name (a t-shirt scale forced to hold 40 and 48) is a sign the scale needs a decision, not a quiet insertion.
- **A new variant on an existing set.** To add a size or a state a set lacks – a Tablet size on the chrome, say – mirror the nearest existing variant, rename its variant property to the new value, and change only what the new variant needs. The thing that changes often lives in a different node per component: a frame's padding on one, a child's padding on another. Read each one before you edit it, and keep related values on a scale (16 / 48 / 128, not 40 / 48 / 128 by accident).
- **Renaming a variant value.** Rename the value, the internal default label, and any nested component that shows the same word (a pill inside a banner). Then it must be republished before any consumer can use the new name.

---

## Editing a shared library, and the republish that follows

Components here are published to a library that other files consume. Two things follow, and both are easy to miss.

- **Local versus remote.** A component imported by key reports as remote, but its editable source is local to the library file. Find it by name with a page search and edit that source; you cannot edit a remote reference.
- **The republish gap.** After you change a shared component, the change is not live for consumers until the library is republished, and even then a consuming file's existing instances keep the old version until the file accepts the update. To move them onto the new version, swap each instance onto the freshly imported variant and re-assert its layout sizing, or ask the user to run "Update all" in the Assets panel. Say clearly when the republish is the user's step, because you cannot do it.

---

## Write to Figma safely – these rules are not optional

1. **Plan first, write only on sign-off.** See the gate above. This is the first rule for a reason.
2. **Verify every write with a separate call.** A write that reports success is not confirmed. Read it back in a later call.
3. **Keep writes small.** Four to six operations, then verify. The last operation in a long call is the one most likely to fail silently.
4. **Auto-layout, correct child order.** Build in auto-layout. Insert children at the right index – appending puts them last, which silently reverses order. Check the child order after, not just that the call succeeded.
5. **Watch for override resets.** Binding a variable on a component can reset per-instance overrides on that property elsewhere in the file. After binding, re-check known instances and reapply if needed.
6. **Check before you create.** Inspect the current state first. Confirm the component really is missing, and that you are not colliding with an existing name.
7. **Convert node IDs.** Dashes in share URLs (`123-456`), colons for the tools (`123:456`).
8. **Screenshot after a batch.** A read confirms the tree; a screenshot confirms the render. A hidden text node, a wrong-coloured marker, an orphaned part – these show in the picture, not the property read.
9. **Place new work clear of existing work, and re-check afterwards.** Adding a variant grows the set, so a neighbour placed close by gets overlapped or nudged. Read back both edges and confirm the clearance rather than assume it.
10. **Expose nested instance properties.** Set `isExposedInstance` on every nested instance, so an input's label and hint surface on the parent instance instead of forcing the next person into the layer tree.

For the mechanics of writing into Figma, follow the `figma-use` guidance rather than improvising tool calls.

---

## How to build

1. **Confirm it is missing.** Check the `scope_guard`. If it exists or is unconfirmed, stop and use `component-spec` instead.
2. **Borrow the anatomy from a sibling.** Match how the system already builds this kind of thing – an atom plus a composed wrapper, the same state set, the same token roles. Read the sibling with `component-spec` if unsure.
3. **Propose the plan** (the gate). Wait.
4. **On sign-off, build in small verified batches** – frame and structure first, then parts, then variants – reading back each batch before the next.
5. **Bind every part** to its token and text style as planned. No raw values, no legacy styles.
6. **Verify the whole component** with a final read: the variants exist, the bindings resolve, nothing is a raw value.
7. **Hand off** – `component-spec` to document it, `design-review` to check it, and update the `scope_guard` entry so the system knows it now exists and is tokenised.

---

## Output

Write every summary in the simplest words you can, for a reader who did not build the Figma file – if it only makes sense to the person who named the layers, it is not finished.

- **First, the plan** – and nothing written until it is signed off.
- After the build: the component in Figma, plus a summary of each part, its variant set, and the token and text style each part is bound to.
- The final verification read, so the reader knows it was checked, not just written.
- A note of anything left unbound on purpose, and any token that was missing.

---

## What not to do

- **Don't write before sign-off.** The plan-first gate is the whole point.
- **Don't use a raw value or a legacy style.** Bind to tokens and the current text styles, or stop and flag the missing token.
- **Don't rebuild what exists.** Check the `scope_guard` first. Rebuilding creates a duplicate source of truth.
- **Don't detach or improvise.** Nest real atoms; do not paste shapes.
- **Don't skip the verification read.** A write is not done until a separate call confirms it.

---

## When this skill isn't enough

- **It already exists.** Use `component-spec` to read and document it, not this skill to rebuild it.
- **An existing component needs fixing** – a wrong binding, a legacy style. That is remediation. Plan it the same way (propose, then wait), but it is a change, not a build.
- **The build needs checking.** After building, use `design-review` for conformance and accessibility, and `component-spec` for the record.
- **A page needs assembling from the new component.** That is `page-composition`.
