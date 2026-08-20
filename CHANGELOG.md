# Changelog

All notable changes to bimstack will be recorded here. Format inspired by [Keep a Changelog](https://keepachangelog.com).

## [Unreleased]

### Added

- **A review skill for any citizen-facing service** – `service-design-review` reviews a form, a calculator or estimator, an entry or information page, or a whole journey against the alpha.gov.bb service patterns and the Barbados field standards. Three lenses (content, flow, standards), output triaged into must fix now / fix later / confirm with the MDA / test with users, and it closes by offering the fixes as a comparison prototype rather than a list of notes. Adds `/bimstack:review`.

- **Five design-system skills for the GOV.BB Pattern Library** – `component-spec` (reads a component or token layer from Figma and writes an accurate spec: anatomy, states, properties, and the token each part is bound to), `token-cross-check` (cross-checks the design system across the published Figma library, the live working file, and the front-end code, reporting what is missing, mismatched, or unpublished; reads only, never edits), `page-composition` (assembles a page or flow in Figma from linked design-system instances – never detached – behind a scope-and-sign-off workflow: scope the flow, gather inspiration as input, scrutinise the service logic, agree the component inventory as a labelled wireframe, fill library gaps first, then compose breakpoint by breakpoint), `design-review` (reviews a design against the Barbados Digital Service Standards and WCAG 2.1 AA and returns a triaged verdict – Fix now / Fix later / Confirm with MDA – with rigour matched to the surface: measured on a live HTML prototype, inferred on Figma), and `component-build` (creates a new Pattern Library component bound to real semantic and component tokens and current text styles, never raw values or legacy styles, behind a plan-first gate: it proposes anatomy, variants, and the token each part binds to, and waits for sign-off before it writes). Anchored to Standards 3, 5, and 7
- **Two canonical design references, split so values cannot drift** – `references/govbb-design-tokens.yaml` (the structured, machine-checkable values: the colour, semantic, and component token layers, the type scale, the component inventory, and a scope guard separating what is tokenised from what merely exists in Figma) and `references/govbb-design-guide.md` (the narrative: how each layer is built, the decisions behind it, and the rules for reading and writing Figma safely). Values live only in the YAML; the skills read the YAML for values, the guide for reasoning, then Figma for current state
- Cross-references wired in: the content-designer agent's deliverables table now points to `bimstack:component-spec`, `bimstack:token-cross-check`, `bimstack:page-composition`, `bimstack:design-review`, and `bimstack:component-build`, and its design-system note defers to `component-spec` for reading what the system contains
- **Three field-tested skills from the GovTech Barbados research practice** – `synthesize-research` (raw study data → layered findings package: exec summary, briefing docx, GovTech-style findings deck, and appendix, with a themes sign-off checkpoint before anything is written), `govtech-research-plans` (a complete three-part research plan as one .docx – research plan, discussion guide, note-taking framework – for usability testing, discovery interviews, or concept testing, grounded in the actual prototype being tested), and `service-journey-mapping` (service blueprints and journey maps as self-contained GovBB-style HTML pages with inline commenting, built from a transcript, a Miro board, or an interview with the user). These complement the existing native skills: `research-planning` coaches the thinking behind a discussion guide where `govtech-research-plans` produces the full session document; `journey-map`/`service-blueprint` output canonical Markdown where `service-journey-mapping` produces publishable, commentable HTML

- **Four user research skills** – `research-coach` (router and mentor across the whole research cycle, session craft including the false close, direct feedback on the researcher's craft), `research-planning` (decision-linked objectives before any discussion guide, digital literacy warm-up block, behavioural questions over opinion questions, the "if this didn't exist" test), `transcript-analysis` (behaviour over stated preference, workarounds and friction that changed behaviour, cross-referencing prior transcripts as confirms/contradicts/new/extends, coached interpretation), and `research-presenting` (finding → "so what", shared sense-making with the delivery team, action-first readout structure, memorable plain language)
- **`synthetic-research`** – generate synthetic personas grounded in real population data and run them against a bimstack prototype (persona-lens critique + optional agentic walkthrough), producing a friction report with cross-cutting themes, question-protocol failures, and specific recruitment suggestions for real testing. Sits between `/bimstack:build` and real user testing; `synthetic-findings.md` feeds directly to `prototype-iteration` as Path B structured feedback. Bundled: `references/persona-schema.md` (persona template + grounding-data guidance) and `references/question-protocol.md` (the four-question interrogation applied to every form field). Wired into: service-designer agent, AGENTS.md, WORKFLOW.md (Test), PLAYBOOK.md (rapid loop), MANUAL.md, GETTING-STARTED.md, and the README skill tree
- Cross-references wired in: `discovery-kit` and `prototype-iteration` now defer to the research skills; service-designer agent, AGENTS.md, WORKFLOW.md (Listen), PLAYBOOK.md, MANUAL.md, and the README skill tree updated to match
- **Five native service-design skills** – `journey-map` (steps, channels, feelings, pain points, all evidenced or marked `[ASSUMPTION]`), `service-blueprint` (frontstage/backstage/systems swimlanes separated by the lines of interaction, visibility, and internal interaction; time and failure-point rows), `ecosystem-map` (actors, channels, systems, and flows around a citizen need, informal actors included, Standard 7 note per system), `experience-map` (the citizen's whole goal across services, seams as first-class findings, scope recommendation), and `workshop-facilitation` (decision-linked working sessions with agenda, capture plan, and facilitation craft). Mapping skills output canonical Markdown plus an optional single-file GovBB-style HTML visual, matching the prototype pattern

### Fixed

- **Dead skill references.** The service-designer agent, delivery-manager agent, `discovery-kit`, `/bimstack:discover`, WORKFLOW.md, and AGENTS.md deferred to `service-design:*` and `design:*` skills that ship in separate Anthropic plugins and weren't declared as dependencies – a user installing only bimstack hit dead ends at the core service-design moments. All such references now point to the native `bimstack:*` skills, with the Anthropic plugins repositioned as optional extensions in MANUAL.md

## [0.1.1] – 2026-05-16

### Fixed

- Plugin manifest: removed the `agents`, `commands`, and `skills` enumeration arrays from `.claude-plugin/plugin.json`. Claude Code auto-discovers these from the `agents/`, `commands/`, and `skills/` directories – the explicit listings were redundant and may have interfered with registration on some installs.
- Documentation: every `/command` reference now uses the `/bimstack:command` namespaced form that Claude Code actually exposes. The bare `/command` form does not appear in `/` autocomplete (plugin commands surface under the `/bimstack:` prefix).
- Documentation: agent invocation guidance corrected. Claude Code agents are auto-routed by task description, not `@-mentioned`. Use `/agents` to list available agents.

## [0.1.0] – 2026-05-16

First public cut. Built end-to-end in a single working session.

### Added

- **Five agents** – service-designer, content-designer, delivery-manager, developer, cyber-engineer
- **Five skills** – service-standard-assessment, plain-language-check, weeknote, discovery-kit, show-the-thing
- **Six slash commands** – `/bimstack:weeknote`, `/bimstack:assess`, `/bimstack:discover`, `/bimstack:show`, `/bimstack:plain-language`, `/bimstack:threat-model`
- **Four canonical references** – the 13 Barbados Digital Service Standards, the 10 GOV.UK Design Principles, the GDS Way phases (Discovery → Alpha → Beta → Live), and the GovTech Barbados house style
- **Top-level documentation** – README, ETHOS, AGENTS, WORKFLOW, CONTRIBUTING
- **Plugin manifest** at `.claude-plugin/plugin.json`
- **MIT licence**

### Anchored to

- [Barbados Digital Service Standards](https://github.com/govtech-bb/Barbados-Digital-Service-Standards) (13 standards)
- [GOV.UK Design Principles](https://www.gov.uk/guidance/government-design-principles) (10 principles)
- [GDS Way](https://gds-way.digital.cabinet-office.gov.uk/) and [Service Manual agile delivery](https://www.gov.uk/service-manual/agile-delivery) (Discovery / Alpha / Beta / Live)
- Existing GovTech Barbados skills: `govtech-barbados-services`, `govtech-barbados-forms`, `govtech-barbados-presentations`, `govtech-barbados-qr-codes`
- Anthropic skill plugins: `design`, `service-design`, `frontend-design`

### Known gaps

These are deliberate omissions for v0.1 – flagged here so the next version knows where to look.

- No example deliverables in the repo yet. v0.2 should ship at least one worked example per skill.
- No MCP server registration. Agents call existing skills directly; later versions may bundle bespoke MCPs (e.g. a Trident ID dev sandbox, a design-system component fetcher).
- No CI yet. v0.2 should lint plain-language usage and check that every agent cites the references it claims to.
- No hooks. Phase-gate hooks (e.g. "block a `Met` rating without evidence") are a candidate for v0.3.

### Inspirations

Format inspired by [Garry Tan's gstack](https://github.com/garrytan/gstack). Worldview inspired by the UK Government Digital Service, particularly Russell Davies and Giles Turnbull on working in the open, and the Cabinet Office Service Manual on agile delivery.
