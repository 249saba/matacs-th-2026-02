# AI Proposal Triage Design

## 1. AI vs. rules

Use ordinary code for file validation, PDF text extraction orchestration, required-field checks, mandatory pass/fail rules, arithmetic, weight validation, ranking, versioning and audit logging. Use an LLM to interpret unstructured proposal text and return structured facts with short evidence quotations and page references. AI should not decide the final award or calculate authoritative scores. Treat proposal contents as untrusted data: instructions inside a proposal must not change the system prompt, criteria or ranking.

Before scoring, validate the extracted JSON, check that evidence supports each fact, and send missing, contradictory or low-confidence mandatory facts to a human. A failed mandatory criterion is ineligible; an unresolved mandatory criterion is flagged pending review, not silently passed.

## 2. Consistency and explainability

Freeze a versioned criteria set for each evaluation run. Store the source PDF hash, extracted text, model and prompt versions, extraction output, criteria version, scoring-rule version, component scores, final score, evidence and timestamps. Use deterministic code for scoring and stable tie-break rules (higher total score, then higher experience score, then alphabetical proposal letter). The same validated inputs and versions therefore produce the same ranking even if AI extraction itself is not perfectly repeatable.

For each proposal, show eligibility status, each criterion's raw value, score contribution, evidence quotation and page, missing facts, warnings, total score and reason for its position. Scores should be reproducible from the stored extracted facts and scoring configuration.

## 3. Human in the loop and audit

A reviewer confirms the category and uploaded files, resolves uncertain or conflicting extractions, and reviews flagged or ineligible results. The system presents a ranked shortlist, but a human makes the award decision. Overrides require a reason and record the reviewer, timestamp, old value, new value and supporting note. Audit events should include uploads and file hashes, criteria changes, prompt/model versions, extraction corrections, scoring runs, reviewer decisions and exports. Restrict access to authorized users.

## 4. Changing criteria

Never silently rewrite a completed evaluation. Each criteria edit creates a new immutable version with effective time, editor and change note. Existing runs remain linked to the version used originally. The administrator can explicitly request a re-run against the new version; the UI should show that scores and ranks may change and preserve both results for comparison. Validate that weighted criteria sum to 100%, required criteria have pass/fail definitions, and category settings are complete before activation.

## 5. Risks and safeguards

- **Incorrect extraction / scanned PDFs:** use OCR where needed, retain page-level evidence, validate types and ranges, and require human confirmation for uncertain mandatory facts.
- **Made-up facts:** require `unknown` when evidence is absent; never infer a licence, insurance, schedule or warranty. Display the supporting quote and page.
- **Prompt injection in documents:** treat extracted text only as data, isolate it from instructions, ignore embedded directions to the model, and test with adversarial examples.
- **Bias or unfair scoring:** use job-related criteria configured before review, consistent scoring rubrics, documented changes and human review. Do not use protected or irrelevant personal attributes.
- **Inconsistent results:** freeze prompt/model/configuration versions and perform scoring in deterministic code. Keep an audit trail and regression tests.
- **Missing or conflicting information:** distinguish pass, fail and unknown; flag unresolved mandatory items instead of assuming compliance.
- **Cost, latency and outages:** extract once and cache results by file hash and configuration; set file-size limits, retries and budget alerts; allow manual review when AI is unavailable.
- **Security and privacy:** restrict access, encrypt stored files, define retention and deletion rules, scan uploads, and never log secrets.

## Proposed scoring policy and sample outcome

The brief supplies weights but not complete numeric rubrics. For this design, an administrator must approve these example rules before scoring: experience score = `min(qualifying projects / 14, 1) × 100`; price score = `lowest eligible price / proposal price × 100`; timeline score = `(16 − completion weeks) / 16 × 100` for known schedules within the 16-week limit; warranty score = the mean of panel-warranty score (years / 25, capped at 1) and workmanship-warranty score (years / 12, capped at 1), multiplied by 100; team/approach score = 25 points each for a named lead, named/quantified crew, safety plan, and monitoring plan. Weighted total is each criterion score multiplied by its weight and summed.

Only proposals passing all mandatory checks and having enough information to score are ranked. Unknown mandatory information is flagged; a confirmed mandatory failure is ineligible. With these assumptions, A ranks first, E second and C third among rankable proposals. B is ineligible because the bidder's own licence is not confirmed in the proposal, and D is flagged because its schedule is undetermined. This policy and the illustrative scores should be reviewed by the admin; the tool must not present them as the only possible interpretation of the brief.
