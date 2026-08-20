# Question protocol

A structured interrogation applied to every question on every page of a prototype during Stage 2 Mode A (persona-lens critique). The protocol catches questions that exist because someone in the back office wanted the data, not because the citizen needed to answer them.

---

## The four questions about every question

For each field, radio group, checkbox set, dropdown, or free-text input on every page of the prototype, answer:

### 1. Who uses this answer?

Name the specific team, system, or process that consumes this data. "The back office" is not specific enough – which person, in which workflow, for what decision?

If nobody can name a consumer, flag it:

> **QP-FAIL: No identified consumer.** This question collects data nobody has claimed. Remove it or identify the consumer before testing with users.

### 2. What do they use it for?

Describe the decision, action, or downstream process the answer feeds. Be concrete: "The renewals officer uses this to match the application to the register entry" – not "for processing".

If the use is vague or speculative, flag it:

> **QP-WARN: Vague purpose.** The stated use ("for our records") doesn't name a specific decision or action. Challenge the MDA: if the answer doesn't change a decision, the question shouldn't be asked.

### 3. What happens if the citizen leaves it blank?

Three possible answers:

- **The service breaks** – a downstream system or decision genuinely cannot proceed. The question is correctly required.
- **The service degrades** – someone has to chase the citizen or make an assumption. The question may be required, but the error handling and the "why we need this" hint text must be clear.
- **Nothing happens** – the field is nice-to-have. It should be optional, and the form should say so. If it's currently marked required, flag it:

> **QP-FAIL: False mandatory.** Nothing downstream breaks if this is blank, but the form marks it as required. Either make it optional or justify the requirement.

### 4. What happens if the citizen fills it in with any old thing?

Three possible answers:

- **A downstream check catches it** – fine, but the citizen will get chased. Is the chase worth the hassle? Would a format hint or validation prevent it?
- **Nobody checks** – the data sits unchecked. If the question is important enough to ask, it's important enough to validate. Flag:

> **QP-WARN: Unvalidated input.** No downstream check exists. Either add validation, accept any input gracefully, or remove the question.

- **Wrong data causes harm** – to the citizen (wrong benefit amount, rejected application) or to the service (bad data in the register). Flag:

> **QP-FAIL: Harm from bad data.** Wrong input here causes [specific harm]. The form needs inline validation, a confirmation step, or a lookup to prevent it.

---

## Additional checks per question

Beyond the four core questions, also check:

### Format assumptions

Does the question assume a format the citizen might not match?

- Date fields that assume DD/MM/YYYY when Barbadians commonly write month names
- Phone fields that reject valid Barbadian mobile formats (e.g. +1-246 with or without dashes)
- Name fields that reject hyphens, apostrophes, or names shorter than two characters
- Address fields that assume a street number when many Barbadian addresses use lot numbers, tenantry names, or landmarks
- NRN / Trident ID fields without clear format guidance

> **QP-WARN: Format assumption.** This field assumes [format], but [population segment] commonly uses [alternative]. Add format guidance or accept multiple formats.

### Answer options that exclude

For radio buttons, dropdowns, and checkbox sets:

- Is there an "Other" or equivalent option for people whose situation doesn't fit?
- Do the options use language the citizen would use, or internal categories?
- Are the options mutually exclusive when they should be, and non-exclusive when they should be?
- Is there a "Prefer not to say" option where the question touches sensitive data?

> **QP-FAIL: Excluding options.** A [persona type] cannot answer this question truthfully because [reason]. Add [specific option] or restructure.

### Hint text and "why we ask"

- Does the question have hint text explaining what a good answer looks like?
- Does it explain *why* the service needs this information?
- For questions that citizens commonly get anxious about (income, health, identity documents), is there reassurance about how the data will be used?

> **QP-WARN: Missing context.** This question asks for [sensitive/unusual data] without explaining why or how it will be used. Citizens who are anxious about [specific concern] may abandon here.

---

## Aggregating protocol failures

After running the protocol across every question on every page, collect and rank:

1. **QP-FAIL items** – these are blockers. Each one is a question that shouldn't be asked (no consumer), shouldn't be required (false mandatory), or causes harm (bad data, excluding options).

2. **QP-WARN items** – these are friction points. Each one makes the form harder to complete correctly or makes the citizen less confident. They're not blockers, but they compound – a form with ten QP-WARNs and no QP-FAILs is still a hard form.

3. **QP-OK items** – questions that pass all four checks. Don't list these individually in the report, but give the count so the team knows the ratio.

The aggregated list feeds into the Stage 3 report under "Question-protocol failures".

---

## When to skip the protocol

Never. Every question on every page gets the protocol. The whole point is that it's mechanical and exhaustive – it catches the questions nobody thought to challenge because they've always been on the form.

The only variation is depth: in Mode A (fast), the protocol runs as a desk review against the HTML. In Mode B (agentic walkthrough), it runs as part of the simulated completion, so format assumptions and validation behaviour are tested live.
