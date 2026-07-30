  
GOVTECH BARBADOS  ·  ALPHA.GOV.BB

**Service Patterns**

*Definitions, Examples, and Field Standards for Barbados Digital Services*

July 2026  ·  Updated with live validation findings

**How to use this document**

This document defines the nine standard pages every alpha.gov.bb service follows, the reusable field blocks that sit within them, and the Barbados-specific field standards that apply across all forms. Use it when analysing a paper form, designing a new digital service, or reviewing an existing one.

The service pattern is not a rigid template, it is a shared language. Every service is different, but by mapping its questions to the same nine pages and the same set of reusable blocks, teams can design consistently, review quickly, and build forms that people can actually complete.

| Live validation data from alpha.gov.bb (July 2026\) found 364 errors across 17 forms in 30 days. The most common causes were missing format hints on ID and phone fields, and eligibility checks placed after rather than before personal details. Key findings are embedded throughout this document. |
| :---- |

| 1\.  Start Page |
| :---- |

**What this page does**

Sets expectations before the user commits to starting. Tells them what the service is, who it is for, what they need to have ready, how long it takes, and what it costs.

**YOU PUT HERE**

**→**  Name and plain-English description of the service

**→**  Who this service is for (citizens, residents, employers, third parties)

**→**  What the applicant needs before starting — documents, IDs, fees

**→**  A brief eligibility summary — who qualifies — but no questions yet

**→**  Processing time: how long before the applicant hears back

**→**  Cost of the service, if applicable, and how payment is made

**BARBADOS EXAMPLES**

**Get a Birth Certificate (Barbados Registration Department):** "You will need your National Registration Number (NRN), the full name and date of birth of the person named on the certificate, and your reason for ordering. Fee: BBD$10. Processing: 3–5 working days."

**Jobstart Plus Programme (MoE\&T):** "This programme is open to Barbadian citizens and permanent residents aged 18–35 who are currently unemployed. You will need your NRN, NIS number, and bank account details for payment. Applications are reviewed within 10 working days."

**Apply for a Conductor Licence (Transport Authority):** "You must hold a valid driving licence and have no disqualifications in the past 5 years. Fee: BBD$25. You will need your NRN and a recent police certificate of character from the Royal Barbados Police Force."

| The Start Page is where the user decides whether to continue. If the eligibility summary is vague or the document list is incomplete, users start the form unprepared and abandon it partway through or submit with the wrong documents. Be specific. |
| :---- |

| 2\.  Eligibility Page |
| :---- |

**What this page does**

Asks first-level filtering questions to determine whether the user can continue. If they fail any check, they are stopped immediately with a clear explanation and signposted to alternatives. No personal data is collected before this point.

**YOU PUT HERE**

**→**  Can this applicant use this service?

**→**  Are they applying for themselves or on behalf of someone else?

**→**  High-level blockers: age, citizenship, residency, employment status

**→**  One question per screen — do not bundle eligibility checks on a single page

**BARBADOS EXAMPLES**

Programme and benefit forms \- eligibility questions must come here, before Applicant Details:

*"Are you a Barbadian citizen or permanent resident?"*

*"Are you aged between 18 and 35?"*

*"Are you currently unemployed or seeking employment?"*

Certificate and licence forms \- simpler gatekeeping questions:

*"Are you applying for your own birth certificate, or on behalf of someone else?"*

*"Do you have a valid National Registration Number (NRN)?"*

*"Do you hold a valid Barbados driving licence?"*

| CRITICAL — July 2026 validation finding: age and eligibility failures on Jobstart Plus, Community Sports Programme, and all 10 Youth Opportunity forms were recorded AFTER applicant details had been entered. Users spent time on the form then discovered they did not qualify. All programme forms must run eligibility checks on this page, before the Applicant Details page. Use the eligibility screener gate. |
| :---- |

| 3\.  Applicant Details Page |
| :---- |

**What this page does**

Collects the personal information needed to identify the person applying. This page is about who they are, not about the service yet. It draws entirely from the standardised field blocks.

**YOU PUT HERE**

**→**  Full name (Name Block)

**→**  Personal identifiers: NRN, NIS Number, Date of Birth, Gender, Marital Status (Personal Details Block)

**→**  Home address including parish (Barbados Address Block)

**→**  Contact details: phone and email (Contact Block)

**→**  Confirmation that the applicant is the one submitting (if required)

**BARBADOS FIELD STANDARDS — MANDATORY ON EVERY IMPLEMENTATION**

| Field | Format | Required hint text | Example to show |
| :---- | :---- | :---- | :---- |
| National Registration Number (NRN) | YYMMDD-XXXX | Yes — mandatory | e.g. 970315-1234 |
| Telephone / Mobile Number | 246-XXX-XXXX | Yes — mandatory | e.g. 246-430-1234 |
| Postal Code | BB \+ 5 digits | Yes — mandatory | e.g. BB11000 |
| Date of Birth | DD/MM/YYYY | Yes — label the format | e.g. 15 03 1997 |
| Parish | Dropdown — 11 options | Placeholder: "Select a parish…" | Christ Church, St. Michael, etc. |
| NIS Number | 6-digit numeric | Yes — with source hint | "Find this on your NIS card or payslip" |

**PARISH DROPDOWN OPTIONS (BARBADOS — ALL 11\)**

Christ Church · St. Andrew · St. George · St. James · St. John · St. Joseph · St. Lucy · St. Michael · St. Peter · St. Philip · St. Thomas

| CRITICAL — July 2026 validation finding: The NRN field (applicant.idNumber) is the single most-failed field across the entire platform — 76 errors across 5 forms. The format hint is not optional. Every form that collects the NRN must show "e.g. 970315-1234" beneath the field. Telephone generated 45 errors; postcode generated 42\. The same rule applies to both. |
| :---- |

| 4\.  Criteria and Entitlement Page |
| :---- |

**What this page does**

Goes deeper than the Eligibility Page. Asks behaviour- or status-based questions that determine whether the applicant is entitled to the specific service they are requesting. These questions may reveal disqualifications, conditions, or legal requirements that affect what happens next.

**YOU PUT HERE**

**→**  Past offences, disqualifications, endorsements

**→**  Status-based conditions that may affect entitlement

**→**  High-level yes/no triggers that open further question branches

**→**  These determine entitlement before gathering supporting evidence

**BARBADOS EXAMPLES**

*"Have you ever been disqualified from holding a conductor licence in Barbados or any other country?"*

*"Have you received any NIS benefit payments in the past 12 months?"*

*"Is the person whose death certificate you are requesting a Barbadian citizen?"*

*"Are you currently receiving an employer pension in addition to NIS benefits?"*

*"Has your NIS benefit claim been previously rejected?"*

| This page always precedes Evidence-Based Questions. A "Yes" here is what triggers a follow-up evidence page. A "No" may mean the evidence page is skipped entirely. Design the flow so that only relevant follow-up questions are shown. |
| :---- |

| 5\.  Evidence-Based Questions Page |
| :---- |

**What this page does**

Collects the detailed information needed to verify a claim, rule, or condition flagged on the Criteria and Entitlement page. This page only appears when there is something to follow up on from the previous page.

**YOU PUT HERE**

**→**  Structured details triggered by a "Yes" answer in Criteria and Entitlement

**→**  Court name, date, period (if they declared a disqualification)

**→**  Description of the relationship to the deceased (death certificate)

**→**  Employer and employment period details (Jobstart, NIS forms)

**→**  Specific structured evidence questions required for back-end checks

**BARBADOS EXAMPLES**

*"Which court issued the disqualification, and on what date? \[Conductor Licence\]"*

*"What is your relationship to the deceased? \[Death Certificate\]"*

*"Name your most recent employer and the dates of your employment. \[Jobstart Plus\]"*

*"Which educational institution are you currently attending, and what programme are you enrolled in? \[Youth Opportunity BTU\]"*

| This page always follows from something asked in Criteria and Entitlement. If nothing was flagged there, this page does not appear. Never use this page to ask general background questions — it is specifically for follow-up evidence. |
| :---- |

| 6\.  External Evidence Upload Page |
| :---- |

**What this page does**

Allows the user to upload documents the government system cannot verify digitally. Keep this page as short as possible — only ask for documents that are genuinely required and cannot be obtained through system integration.

**YOU PUT HERE**

**→**  National Registration Number card (both sides) or valid passport

**→**  Police Certificate of Character — from the Royal Barbados Police Force (RBPF)

**→**  Birth certificate of the person named (for third-party certificate requests)

**→**  Proof of death — where required for survivor's benefit or estate applications

**→**  NIS contribution statement (for employment history verification)

**→**  Educational certificates (for Youth Opportunity BTU and similar programmes)

**→**  Bank statement or passbook showing account name and number (for direct deposit)

**→**  Passport photographs (for licence and ID applications)

**→**  Employer letter confirming employment status or termination

**ACCEPTED FILE FORMATS — STANDARD ACROSS ALL ALPHA.GOV.BB SERVICES**

PDF, JPG, PNG  ·  Maximum file size: 5MB per document  ·  Scanned documents must be legible and unobstructed

| Only request documents the agency cannot obtain internally. If NIS can verify NIS numbers from their own system, do not ask for an NIS card upload. If the Barbados Registration Department can verify birth registration, do not ask for a birth certificate to be uploaded. Work with the MDA to establish what can be verified automatically. |
| :---- |

| 7\.  Check Your Answers Page |
| :---- |

**What this page does**

Shows the applicant a complete, readable summary of everything they have entered. Gives them the opportunity to review and correct before submitting. No new questions are asked here.

**YOU PUT HERE**

**→**  A section-by-section read-only summary of all answers

**→**  "Change" links next to each section heading (not each individual field)

**→**  Uploaded file names with a link to replace each one

**→**  Total fee due (if applicable), clearly stated before submission

**→**  The legal declaration and consent checkbox — directly above the submit button

**DECLARATION TEXT — STANDARD FOR BARBADOS GOVERNMENT SERVICES**

| Standard declaration: "I declare that the information I have provided on this form is true and correct to the best of my knowledge and belief. I understand that providing false information may result in prosecution under the laws of Barbados."For NIS forms carrying a penalty: add "WARNING: Any person who makes a false statement is liable to a fine or term of imprisonment or both." Display this ABOVE the submit button. |
| :---- |

| CRITICAL — July 2026 validation finding: The declaration checkbox (declaration.confirmed) failed 7 times across 4 forms. The checkbox is being missed. Requirements: minimum 44×44px tap target on mobile; visually separate from the declaration text body; do not place it at the bottom of a long paragraph — use a summary \+ expandable detail approach for long declarations. |
| :---- |

| 8\.  Payment and Submit Page |
| :---- |

**What this page does**

Allows the user to pay for the service (when applicable) and make their final submission. Even when there is no fee, there is always a submit step on this page.

**YOU PUT HERE**

**→**  Fee amount, clearly stated in BBD$ before the user enters payment details

**→**  Accepted payment methods — currently EZ Pay for government services

**→**  Payment confirmation before submission is triggered

**→**  Final declaration (if not already on the Check Your Answers page)

**→**  Submit button — labelled specifically, e.g. "Submit Application", "Pay and Submit", "Submit Certificate Request"

**BARBADOS PAYMENT NOTES**

Government services on alpha.gov.bb currently accept payment via EZ Pay. Do not show a payment page for services that are free of charge — go directly to submission. Where a service has tiered fees (e.g. different certificate types), state the correct fee on the Check Your Answers page before the user reaches this step.

| Not every service requires payment, but every service has a submit step. The submit button label should reflect what the action does — "Submit Application" for a programme form, "Pay and Submit" for a fee-bearing service, "Submit Certificate Request" for a records request. |
| :---- |

| 9\.  Confirmation and Next Steps Page |
| :---- |

**What this page does**

Confirms the submission and tells the user clearly what will happen next, in what timeframe, and who to contact if they need help. No form fields appear on this page — it is entirely output.

**YOU PUT HERE**

**→**  "Your application has been submitted." — clear, direct, prominent

**→**  Application reference number — bold, easy to copy or screenshot

**→**  Email confirmation: "A confirmation has been sent to \[email address\]."

**→**  Expected processing time — be specific where possible

**→**  What the applicant should do if their circumstances change

**→**  MDA contact details for follow-up enquiries

**→**  Any follow-up steps the applicant needs to take (e.g. attend in person, await a call)

**BARBADOS EXAMPLES**

**Get Birth Certificate:** "Your certificate request has been submitted. Reference: BRD-2026-XXXXXX. Processing takes 3–5 working days. Your certificate will be available for collection at the Barbados Registration Department, Coleridge Street, Bridgetown. You will receive an email when it is ready."

**Jobstart Plus Programme:** "Your application has been submitted. Reference: JSP-2026-XXXXXX. The Ministry of Education and Technological and Vocational Training will review your application within 10 working days. You will be contacted at the phone number or email you provided."

| If a service generates a legal document with multiple parties — for example, the Termination of Service form which must reach both the employee and NIS within 7 days — the confirmation page must make the distribution and deadline explicit. "A copy has been sent to \[recipient\]. NIS must receive their copy within 7 days of the termination date." |
| :---- |

**Standardised field blocks**

Reference each block for field-level specifications, validation rules, and design notes.

| Block | Optimal page | Notes |
| :---- | :---- | :---- |
| Name Block | Applicant Details | Title, first name, middle name(s), last name. Use DD/MM/YYYY field ordering. Pre-fill after login where possible. |
| Barbados Address Block | Applicant Details | Street address, district (village/area), parish (dropdown — 11 options), postal code (BB \+ 5 digits). Hint text mandatory on postcode. |
| Personal Details Block | Applicant Details | NRN (YYMMDD-XXXX — example hint mandatory), NIS Number, Date of Birth, Gender, Marital Status. High candidate for pre-fill. |
| Contact Block | Applicant Details | Telephone (246-XXX-XXXX — example hint mandatory), mobile, email. At least one of telephone/mobile required. |
| Eligibility Screener Gate | Eligibility Page | Age range, citizenship, residency, programme-specific questions. Must come BEFORE Applicant Details on all programme forms. Added July 2026\. |
| Eligibility Block | Eligibility Page | ID type gate, termination type gate. Simple yes/no. Stop ineligible users immediately with a clear explanation and alternatives. |
| Employer Identity Block | Applicant / Criteria | Employer name and NIS registration number. Can pre-populate from employer login. Format TBC with NIS (Q-01). |
| Employment History Block | Evidence-Based | Occupation, employment dates, termination and last paid dates. Sequential date validation mandatory. |
| Business Details Block | Applicant / Criteria | Business name, CAIPO registration number, nature of business, estimated monthly income. |
| Evidence Upload Block | External Evidence Upload | NRN card, passport, police certificate (RBPF), NIS statement, bank passbook, educational certificates. PDF/JPG/PNG, max 5MB. |
| Declaration Block | Check Your Answers | Legal statement \+ consent checkbox \+ date. Penalty-carrying forms: add legal warning above checkbox. 44px minimum tap target. |
| Payment Block | Payment and Submit | EZ Pay integration. Display fee clearly before user enters payment. State the exact BBD$ amount. |
| Official Use Block | Admin view only | Internal officer fields. Never visible to citizens. Requires separate MDA officer UI spec. |
| Banking Details Block | Applicant Details | Bank, branch, account type, account number. Required when claimant elects direct deposit. Upload bank statement as proof. |
| Alternate Payee / Nominee Block | Applicant Details | Mirrors full applicant details for a nominated third party. Requires empowerment instrument upload. |

**AI prompt — form analysis**

Use this prompt when analysing a paper form or an existing digital form to categorise its questions into the service pattern structure.

| Prompt to use: Using the attached alpha.gov.bb Service Patterns document, analyse the attached form and do the following:1. Categorise each question or field into the correct service pattern page (Start Page, Eligibility Page, Applicant Details, Criteria and Entitlement, Evidence-Based Questions, External Evidence Upload, Check Your Answers, Payment and Submit, Confirmation).2. Flag any questions that are in the wrong position — for example, eligibility checks that appear after personal details, or evidence questions that appear before criteria questions.3. Note any fields that are missing Barbados-specific format guidance: NRN (YYMMDD-XXXX), telephone (246-XXX-XXXX), postcode (BB \+ 5 digits), date of birth (DD/MM/YYYY).4. Identify which standardised blocks apply, and note any fields that deviate from the block specification.5. Produce a summary table showing: Field / Current Page / Correct Page / Issue (if any).\[Attach this document and the form to analyse\] |
| :---- |

**Common design errors to avoid**

Based on live validation data from alpha.gov.bb forms (July 2026, 364 errors across 17 forms).

**1\. Eligibility checks placed after personal details**

The most disruptive error. Users fill in their name, address, and ID, then discover they don't qualify. Move all age, citizenship, and programme eligibility checks to the Eligibility Page (page 2), before the Applicant Details page (page 3). Use the eligibility screener gate.

**2\. Missing format hints on ID, phone, and postcode fields**

The NRN, telephone, and postcode fields all have specific Barbadian formats. Without an example, users guess and get rejected. "e.g. 970315-1234", "e.g. 246-430-1234", and "e.g. BB11000" are not optional — they are required on every implementation of these fields.

**3\. Shared templates deployed without field-level testing**

The 10 Youth Opportunity forms share a common template. When the template had field guidance gaps, those gaps appeared on all 10 forms. Test any template on its own before deploying it as the basis for multiple forms.

**4\. Declaration checkboxes that are easy to miss**

The declaration checkbox fails when it is too small to tap on mobile, or sits at the bottom of a long text block that users skip. Visually separate it from the declaration text and ensure a minimum 44×44px tap target.

**5\. Launching without user testing**

Five people attempting a form out loud before it goes live would catch format hint problems and eligibility placement errors in a single afternoon. Informal testing is not optional for government services — it is the cheapest quality check available.

*GovTech Barbados  ·  alpha.gov.bb  ·  July 2026*  
