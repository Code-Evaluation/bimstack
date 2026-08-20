# Service Design Review — Checklist

Work through this systematically. Don't skip sections — even if a service looks good at first glance, edge cases hide in the details. Say it once if an issue repeats on every page; note that it applies throughout.

---

## 0 · Before you start

- [ ] Have you seen the whole journey? Every page, every branch? If unsure, ask.
- [ ] Is there any evidence of user research or testing? If none, note it in the summary — it caps the confidence of the review.
- [ ] What kind of thing is this — a form journey, a tool (calculator/estimator/checker), an entry/information page, or a mix? Apply the relevant sections below.

---

## 1 · Content — plain language (every citizen-facing word)

- [ ] Reading age: target grade 5, flag over grade 8. Flag sentences over 20 words with a rewrite.
- [ ] Swap list — flag and rewrite:

| Don't write | Write instead |
|---|---|
| submit | send |
| provide | tell us, give us |
| verify, validate | check |
| select | choose |
| proceed | continue |
| commence | start |
| reside | live |
| prior to | before |
| obtain | get |
| purchase | buy |
| terminate | end, stop |
| utilise | use |
| ensure | make sure |
| mandatory | required, you must |
| in the event that | if |
| in the absence of | without |
| not later than | by |
| in receipt of | getting |
| with regard to / in respect of | about |
| pursuant to / in accordance with | under, following |
| approximately | about |
| sufficient | enough |
| endeavour | try |
| kindly / be advised that | (delete) |

- [ ] Voice: "you" not "the applicant"/"the user"; "we" not "the Department". On tools, never "applicant" — nobody is applying.
- [ ] One canonical organisation name, said in full on first use. Multiple org acronyms on one page (PRCD/PAD/NISSS) = must-fix. One MDA contact story — citizens contact one body, not four.
- [ ] The system does the work: no "Do not include commas" — the system strips commas, accepts spaces, trims whitespace. Flag instructions that exist because validation is lazy.
- [ ] Verb-based H1s: "Calculate your Government pension", not "Government Pension Calculator".
- [ ] Specificity: no "soon", "shortly", "further information", "required documentation" — dates, links, named documents.
- [ ] Error messages: what went wrong AND what to do — both.
- [ ] Confirmation content: reference number + what happens next + roughly when — all three.
- [ ] Honesty: estimates labelled as estimates; unconfirmed figures never presented as facts; no overclaiming eligibility or giving advice the MDA hasn't signed off.

---

## 2 · Flow — the one-job test (information page decision)

Ask, for any page mixing explanation with action:

- [ ] Is the page doing more than one job? (e.g. give an estimate AND teach how it works)
- [ ] Does the explanation sit between the user and the thing they came to do?
- [ ] Would that information be more useful after the result?

**Mostly yes → split**: information gets its own URL, hyperlinked from the entry page and the results page. Primary action stays clear and direct.
**Mostly no → keep together**: a short "before you start" section on the entry page is fine. Don't create a page nobody needs to maintain.

Either way: record as a hypothesis for the next usability round (e.g. "can people find the formula page when they want it?").

---

## 3 · Flow — forms (the nine pattern pages)

Map every question to its correct page (see `service-patterns.md`):
Start → Eligibility → Applicant Details → Criteria and Entitlement → Evidence-Based Questions → External Evidence Upload → Check Your Answers → Payment and Submit → Confirmation.

- [ ] **Eligibility before Applicant Details — always.** The most disruptive live error on the platform. Must-fix, every time.
- [ ] One eligibility question per screen; ineligible users stopped immediately with explanation + alternatives.
- [ ] No personal data collected before the eligibility gate.
- [ ] Start page: plain description, who it's for, what to have ready, processing time, cost. Specific — vague document lists cause abandonment.
- [ ] Start page promises match the form: same documents, same units (months vs years), same fees.
- [ ] Criteria and Entitlement precedes Evidence-Based Questions; evidence pages only appear when a criteria answer triggered them.
- [ ] Upload page as short as possible; only documents the agency can't verify internally.
- [ ] Check Your Answers: section-by-section summary, "Change" links per section (not per field), fee stated, declaration + checkbox directly above submit.
- [ ] No payment page on free services; every service still has a submit step.
- [ ] Confirmation: reference number bold and copyable, email confirmation line, processing time, MDA contact, any follow-up steps. Multi-party legal documents state distribution and deadline explicitly.

---

## 4 · Flow — tools (calculators, estimators, checkers)

- [ ] Results come **after** the calculation, per the pattern — never a results block before input.
- [ ] Worked calculation with the user's own numbers on the results page ("300 months ÷ 600 × $60,000"), not just the abstract formula.
- [ ] Next steps come after the result — the moment they're most useful.
- [ ] "Estimate only" caveat attached to the result, not buried on an intro page.
- [ ] Edge cases the tool can't calculate correctly are routed to the MDA — never shown a confidently wrong number.
- [ ] Input precision honest: if year-only entry can drop months of real service, flag it.
- [ ] "How it's calculated" content linked from entry and results pages (if split — see section 2).

---

## 5 · Standards — Barbados field standards (fix-now items)

Backed by live validation data (364 errors across 17 forms, July 2026):

- [ ] NRN: YYMMDD-XXXX, hint "e.g. 970315-1234" — mandatory (76 live errors without it)
- [ ] Telephone: 246-XXX-XXXX, hint "e.g. 246-430-1234" — mandatory (45 errors)
- [ ] Postal code: BB + 5 digits, hint "e.g. BB11000" — mandatory (42 errors)
- [ ] Date of birth: DD/MM/YYYY, format labelled
- [ ] Parish: dropdown, all 11, placeholder "Select a parish…"
- [ ] NIS number: 6 digits, source hint ("Find this on your NIS card or payslip")
- [ ] Declaration: standard Barbados text; penalty warning above checkbox on penalty forms; checkbox visually separate, ≥44×44px tap target; directly above submit
- [ ] Uploads: PDF/JPG/PNG, ≤5MB, legible-scan note
- [ ] Submit button labelled for the action: "Submit Application" / "Pay and Submit" / "Submit Certificate Request"
- [ ] Standardised blocks used where they apply (name, address, personal details, contact, declaration…); deviations noted
- [ ] Data minimisation: every field has a clear purpose; nothing asked that government systems can verify (don't ask for an NIS card upload if NIS can verify the number)

---

## 6 · Standards — accessibility and evidence

- [ ] Labels on all fields; results announced (aria-live) on dynamic tools
- [ ] Sensible heading structure; works at mobile widths (rendering varies by device — a page that looks fine on one phone can be cramped on another)
- [ ] Error summary at top + inline errors on validation
- [ ] Be honest about what you can't verify from screenshots — say so rather than passing it
- [ ] If no user testing has happened: recommend an unaided round with 5+ participants. Per the patterns doc, five people attempting the form out loud catches format-hint and eligibility-placement errors in an afternoon. Not optional.

---

## 7 · Do NOT flag

- Back buttons, back-link position, "Previous" button labels — platform chrome; only flag navigation that is actually broken (wrong order, dead end, data loss)
- Progress indicators / "step X of Y" absence
- The form-builder's own preview/review modes
- Fields that aren't on the form (unless the form's own start page claims them)
- The same platform-level issue repeated per page — once, noted as throughout
- Bajan vernacular by default — plain and relatable, not folksy
