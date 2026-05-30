# Engineering Documents
 
A guide to the four core documents and how they connect — from problem discovery to technical decision-making.
 
---
 
## Overview
 
```
PRD  ──▶  RFC  ──▶  TDD  ──▶  ADR
What        Which     How       Why
& why?      path?    exactly?   this?
```
 
Each document hands off to the next. No step can be done well without the one before it.
 
---
 
## The Four Documents
 
### 1. PRD — Product Requirements Document
> *"What are we building, and why does it matter?"*
 
**Owner:** Product Manager  
**Written:** Before any technical work begins  
**Answers:**
- What problem are we solving?
- Who are the users?
- What does success look like?
- What is out of scope?
The PRD deliberately avoids saying *how* to build anything. That's engineering's job.
 
---
 
### 2. RFC — Request for Comments
> *"Should we do X or Y? Let's discuss before committing."*
 
**Owner:** Tech Lead or Engineer  
**Triggered by:** PRD (when the solution space is large or unclear)  
**Answers:**
- What are the viable approaches?
- What are the trade-offs of each?
- Which direction should we go?
The RFC is an open discussion document. It invites broad input — across the team or across teams — before anyone invests in a detailed design. Think of it as a checkpoint that prevents the team from going deep on the wrong solution.
 
> **When to skip RFC:** For small, well-scoped features where the approach is obvious, go directly from PRD to TDD. The TDD's *Alternative Approaches* section can serve the same purpose at a smaller scale.
 
---
 
### 3. TDD — Technical Design Document
> *"We chose X. Here's exactly how we'll build it."*
 
**Owner:** Engineer  
**Triggered by:** PRD (or RFC if one was written)  
**Answers:**
- What is the system architecture?
- How does the data model change?
- What are the API contracts?
- What are the key flows and edge cases?
The TDD is the detailed blueprint. By this point, the *what* and *which direction* are settled — the TDD focuses entirely on the *how*.
 
---
 
### 4. ADR — Architecture Decision Record
> *"Here's a specific decision we made during design, and why."*
 
**Owner:** Engineer  
**Triggered by:** Decision points that arise while writing the TDD  
**Answers:**
- What decision was made?
- What context led to it?
- What alternatives were considered?
- What are the consequences?
ADRs are not a separate phase — they emerge *during* the TDD process. One TDD can produce multiple ADRs. Unlike other documents, ADRs are **evergreen**: they accumulate over the lifetime of the project and become invaluable when onboarding new engineers.
 
---
 
## End-to-End Example
 
**Scenario:** Building a notification system (email + push alerts for order status)
 
| Document | Content |
|----------|---------|
| **PRD** | "Users need to know when their order ships. Support to reduce tickets by 30%. Must support email and push." |
| **RFC** | "Option A: build in-house queue. Option B: use a managed service (SNS, Pub/Sub). Recommendation: Option B — faster to ship, ops burden is lower at our scale." |
| **TDD** | "We'll use SNS + SQS. Exposes `POST /notifications`. Two consumers: SendGrid worker for email, FCM worker for push. Schema: `notifications(id, user_id, type, payload, status, sent_at)`." |
| **ADR-001** | "Chose SNS over building a custom queue. Context: team is small, message volume is low. Custom queue adds ops burden with no benefit at current scale." |
| **ADR-002** | "Chose SendGrid over SES. Better deliverability dashboard, faster SDK integration. Cost delta is acceptable at current volume." |
 
---
 
## When to Use Each
 
| Scenario | Flow |
|----------|------|
| Small feature, clear approach | PRD → TDD → ADR |
| Large feature or cross-team work | PRD → RFC → TDD → ADR |
| Pure architecture decision | RFC → ADR |
| Incident response / ops change | Postmortem → ADR |
 
---
 
## Key Principle
 
> **Each document answers exactly one question.**  
> PRD answers *what*. RFC answers *which path*. TDD answers *how*. ADR answers *why this specific choice*.  
> Mixing concerns across documents is where teams lose clarity.
 

## References

### RFC
+ [Rust RFC Book](https://rust-lang.github.io/rfcs/introduction.html)
+ [Kubernetes Enhancement Proposals](https://github.com/kubernetes/enhancements/tree/master/keps/sig-apps/2255-pod-cost)

### ADR
+ [ADR Template by Michael Nygard](https://github.com/architecture-decision-record/architecture-decision-record/tree/main/locales/en/templates/decision-record-template-by-michael-nygard)
+ [ADR Complete Guide](https://docsio.co/blog/architecture-decision-record)