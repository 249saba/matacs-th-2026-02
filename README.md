# MATACS Technical Assessment — AI Proposal Triage Design

**Candidate:** Saba Areej
**Reference:** MATACS-TH-2026-02  
**Time spent:** 1:30mins 

## Deliverables

- [Process-flow diagram](diagram/proposal-triage-flow.png)
- [Design write-up](WRITEUP.md)
- [AI prompts](PROMPTS.md)
- [Expected ranking](ranking.json)

## AI-use disclosure

I used ChatGPT to help structure the design, draft the workflow and prompt, and check the proposed scoring approach. I reviewed the suggestions against the assessment requirements and the stated criteria. One important correction was to reject an instruction embedded in Proposal C that tried to dictate its rank; proposal text is untrusted input and must never override the system's rules. I also treated Proposal D's missing schedule as unknown rather than inventing a completion time.

## Assumptions to review

The brief defines criterion weights but does not fully specify numeric scoring rubrics for every weighted criterion. The write-up therefore proposes explicit, configurable scoring rules as an assumption. These should be approved by the administrator before use. The sample ranking is illustrative, not a procurement recommendation.


