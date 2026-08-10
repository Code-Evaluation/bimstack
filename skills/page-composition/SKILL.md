---
name: page-composition
description: Assemble a page or a flow in Figma from GOV.BB design system components – linked instances only, never detached – placed in the right information-architecture order. Use when building a page, a service flow, or a service-page draft from the Pattern Library. Triggers on "compose a page", "build this flow in Figma", "assemble a page", "lay out a service page", "start page", "check your answers page", "put this together from the design system".
---

# Page Composition

This skill assembles a page or a flow in Figma from GOV.BB design system components. It places linked instances of Pattern Library components in the right order, never detached copies, and hands back a composition you can trust to stay in sync with the system.

It anchors to Standard 5 (the service works the first time): a page built from real, current components, in the order the information architecture calls for, works the first time rather than looking right and behaving wrong. It also serves Standard 7 (open, common, interoperable platforms): every element is a linked instance of a shared component, so the page inherits fixes instead of drifting. And it serves Standard 12 (easy to find): placement follows the agreed IA, not a guess.

This skill writes to Figma. That makes it different from `component-spec` and `token-cross-check`, which only read. The safe-writing rules below are not optional.

Two references ground it. `references/govbb-design-tokens.yaml` `scope_guard` tells you which components actually exist and are tokenised – compose only from those. `references/govbb-design-guide.md` holds the decisions. Read them before you place anything.

---

## This skill leans on two others – call them, do not duplicate them

- **What a component contains comes from `component-spec`.** If you are unsure of a component's states or anatomy before you place it, read it first. Do not compose from a half-remembered shape.
- **Missing components are built by `component-build`.** The scoping below can surface a component the flow needs but the library lacks. Building it is `component-build`'s job, behind its plan-first gate. This skill hands off and waits; it never builds a component inline.

Structure – the screens, and the order of a flow – this skill scopes itself with the user, in the workflow below. If the project has an agreed information architecture, follow it; there is no separate IA skill to defer to.

This skill's own job is the assembly: the right instances, in the right order, wired correctly, verified.

---

## When to use this skill

- Building a content page or a landing page from Pattern Library components
- Assembling a service flow: Start, question pages, Check Your Answers, Confirmation
- Drafting a service page as a starting point for content and research
- Turning an agreed structure and a set of components into a real Figma layout

If the request is "what does this component contain?", use `component-spec`. If it is "does Figma match the code?", use `token-cross-check`. This skill puts existing components together; it does not document them or audit them.

---

## Before you place anything: scope, ask, scrutinise, get the components signed off

A page or a flow is not something to start placing on a hunch. Work through this order first, and get a real go-ahead at each gate. It is slower to start and far faster to finish, because it catches the wrong structure and the missing component before they are built into fifteen frames.

1. **Scope it.** Set out the flow as you believe it should be: the screens, and for each the grid and the sections in order. Say what each screen is for. This is a proposal, not a decision.
2. **Ask for inspiration.** Before you settle the scope, ask whether the user has references, a prototype, or a design to build from. Take what they give as input, not instruction.
3. **Scrutinise the direction.** Interrogate the scope against the Standards, WCAG, and – above all – whether the service flow is correct. Do not just trace the inspiration. This is where a wrong status gets dropped, a personal-data decision gets made, a step gets removed. Pull in `design-review` if the design is far enough along to review.
4. **Propose the component inventory, as a picture.** List every component the full flow needs, and mark each one: exists and is tokenised, needs building, or needs a new variant. Show it as a **labelled wireframe** – a low-fidelity skeleton that names each block – not just a list, so the user sees the shape and the parts at once. Default to a self-contained HTML artifact, unless the user asks for it in Figma.
5. **Get the inventory signed off.** Wait for a clear yes on the structure and the components before anything is built.
6. **Build and verify the missing components.** Hand off to `component-build` for anything marked "needs building" or "needs a variant". Each is built behind its plan-first gate and confirmed with a screenshot before it is used.
7. **Compose, and screenshot each breakpoint.** Only now place the confirmed components. Screenshot every breakpoint after each batch – silent bugs (a reversed order, a missing footer, wrong-size type) show up in the picture, not the return value.

Skip a gate and you pay for it later: an unreviewed flow gets built wrong, a missing component gets improvised into a detached one-off, a breakpoint ships at the wrong type size.

---

## The one rule that defines this skill: linked instances only

Every element you place is a linked instance of a published Pattern Library component. Never detach. Never paste a local copy. Never rebuild a component from shapes because it is quicker.

A detached instance stops receiving library updates. It looks identical the day you place it and drifts silently from then on – the opposite of Standard 7. If a component you need does not exist, do not build a one-off in the page. Stop, say so, and flag it for the component work.

Compose only from components the `scope_guard` confirms as present. Prefer ones that are tokenised, so the page inherits token fixes too. And compose from the **published** library – if `token-cross-check` shows the published library is behind the working file, say so, because a page built now will inherit the published state, not the latest edits.

---

## Write to Figma safely – these rules are not optional

These come from hard experience on this file. Follow them exactly.

1. **Verify every write with a separate call.** A write that reports success is not confirmed. Read the result back in a later, separate call before you build on top of it.
2. **Keep writes small.** Four to six operations per call, then verify. The last operation in a long call is the one most likely to fail silently.
3. **Default to auto-layout.** Create frames and place instances in auto-layout unless there is a clear structural reason not to. When you move or swap a child, insert it at the correct index – appending puts it last, which silently reverses order in a horizontal or vertical layout. Check the actual child order after the operation, not just that it succeeded.
4. **Watch for override resets.** Binding a variable on a main component can reset per-instance overrides on that property elsewhere in the file. If your composition relies on an instance override – a corner radius set to sit flush, say – re-check it after any base-level change and reapply if needed.
5. **Check before you create.** Inspect the current state of the page or frame before adding to it. Do not assume it is empty or in the shape you left it.
6. **Convert node IDs.** Share URLs use dashes (`123-456`); the tools need colons (`123:456`). Convert before every call.
7. **Append every part you create.** An instance you create but never append floats on the page, orphaned – the footer is the usual victim. Add it to its frame in the same call.
8. **`clone()` lands on the current page.** Set the current page before cloning, or the clone lands where you did not expect. Selecting and zooming to nodes also needs their page to be current.
9. **Screenshot each breakpoint after a batch.** A read confirms the tree; the screenshot confirms the render. Wrong-size type, a reversed order, an orphaned part – these show in the picture, not the return value.
10. **Locate published components by key, and confirm with a node read.** A name search happily returns the wrong thing: the library can hold two published components with the same name, or a stale test copy published beside the real one. The name locates; only the node read confirms.

For the mechanics of writing into Figma, follow the `figma-use` guidance rather than improvising tool calls.

---

## How to compose

1. **Get the structure from the IA skill.** What page, what section, what order in the flow. Do not proceed on a guessed IA.
2. **List the components you need**, and check each against the `scope_guard`. Confirm each exists and is published. Read any you are unsure of with `component-spec`.
3. **Build the frame in auto-layout**, then place linked instances in IA order, one small batch at a time, verifying each batch with a separate read.
4. **Set each instance to the right variant** – the state, size, or type the page calls for. Do not leave a control on its Default variant if the page needs its Error state.
5. **Do not restyle.** No local colours, no local type. The components carry the tokens; let them.
6. **Verify the whole composition** with a final read: the right instances, in the right order, on the right variants, none detached.

---

## Build responsively and consistently

These are the things that look right in one breakpoint and wrong in the others, or right on the surface and wrong underneath.

- **Set the Typography mode on every breakpoint frame.** Type is responsive through variable modes, not separate styles: the Typography collection has Desktop, Tablet, and Mobile modes, and each text style resolves to whichever is set on the frame. A frame with no mode inherits the page default (Desktop), so a Tablet or Mobile frame renders **desktop type** until you set its mode. A cloned frame keeps the source's mode, so always set it after cloning. Match the mode to the frame.
- **Match the content margins to the chrome.** Full-width chrome (header, footer, banners, back link) carries its own side inset per size variant – for example 128 on Large, 48 on Tablet, 16 on Small. The content column must sit on the same inset, or the page and its chrome will not line up. If a size the flow needs has no matching chrome variant, that is a component job, not a reason to fudge the content margin.
- **Follow the service-name pattern.** The service name is the H1 on the Start page, with no caption above it. On every page after Start, the service name becomes the caption above that page's own H1. One name, set once, carried through.
- **Across files, instance from the published library by key.** A flow lives in the Alpha file; the components live in the Pattern Library. Import the published components, variables, and text styles by key, build one breakpoint, then clone within the file for the other states and breakpoints. Do not rebuild components in the Alpha file.

---

## Service-page mode

Service-page drafting is a mode of this skill, not a separate one. The pattern, one thing per page:

- **Start page** – what the service is, what the user needs, how long it takes, what happens after.
- **Question pages** – one question each (unless two are tightly related, like first and last name), with a label, an optional hint, and a Continue button. Use `Form/Component/Input`, the form controls, and `Atom/Label` for errors – never a bespoke error text.
- **Check Your Answers** – every answer with a Change link, before the user commits.
- **Confirmation** – a reference number, what happens next, and roughly when.

The copy for these pages is the content-designer's work and the `plain-language-check` skill's. This skill lays out the structure; it does not write the words. Draft with real labels where you have them and clear placeholders where you do not, and say which is which.

---

## Output

Write every summary in the simplest words you can, for a reader who did not build the Figma file – if it only makes sense to the person who named the layers, it is not finished.

- The composed page or flow in Figma, built from linked instances in auto-layout.
- A short written summary: each element placed, which component and variant it is an instance of, and where it sits in the flow. Note anything you could not place because the component does not exist, and anything left on a placeholder.
- The result of the final verification read, so the reader knows the composition was checked, not just written.

---

## What not to do

- **Don't detach anything.** This is the whole point. A detached instance is drift waiting to happen.
- **Don't build a missing component in the page.** If it does not exist, stop and flag it. Do not improvise a one-off.
- **Don't restyle instances.** No local colours, no local fonts, no overridden token values. Flag it if the component looks wrong; do not paint over it.
- **Don't put placeholder text in form fields.** The label and hint carry the meaning; a placeholder disappears the moment someone types and reads as an already-filled answer. Leave the input empty – clearing a text node keeps its line height, so the field does not collapse.
- **Don't place on a hunch.** Scope the structure and get it signed off first (the workflow above). Skipping the scope is how a flow gets built wrong.
- **Don't trust a write you have not read back.** Verify in a separate call, every time.
- **Don't compose silently from a stale library.** If the published components are behind the working file, say so – the page will inherit the published state.

---

## When this skill isn't enough

- **The structure is genuinely unresolved.** If there is no clear answer on what the flow is or what order it runs in, settle that with the user in the scoping step before composing – do not resolve it by placing frames.
- **A component you need does not exist.** That is component work, not composition. Flag it; do not build it inline.
- **The copy needs writing.** Words are the content-designer's job, checked with `plain-language-check`. This skill places the structure.
- **The page needs to become code.** Turning a composed Figma page into a working prototype or production markup is a build job, for `build-for-production`, not this skill.
