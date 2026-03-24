🏥 Member Portal Modernization
HIPAA-Compliant Self-Service Platform for Medicare, Medicaid & Commercial Members

Overview
This project documents the end-to-end product ownership of a member portal migration and modernization at a nonprofit health plan in Minnesota — delivered under a hard 8-month deadline driven by vendor end-of-support. The effort was not a greenfield build. It was a high-stakes platform migration with accumulated technical debt, a non-negotiable go-live date, and a significant architectural innovation at its center: a Universal Person ID that unified fragmented member identities across plans for the first time.
Role: Senior Business Analyst / Product Owner (end-to-end ownership)
Timeline: 8 months (hard deadline — legacy vendor support cutoff)
Population Served: Medicare Advantage, Medicaid, Commercial members
Compliance Scope: HIPAA/PHI, CMS member access requirements

Problem Statement
The health plan operated a member portal through an external vendor that had accumulated significant technical debt. Persistent disagreements over architectural direction, unresolved integration issues, and a deteriorating vendor relationship made continued operation untenable. A vendor transition was initiated — with a firm 8-month window before the legacy vendor withdrew support entirely.
This created three simultaneous challenges:
1. Migration under a non-negotiable deadline
The new portal had to be fully built, integrated, tested, and deployed within 8 months. There was no option to extend — the old system would go dark regardless.
2. Technical debt inherited from the legacy platform
Years of workarounds and unresolved architectural decisions had to be untangled and re-solved cleanly in the new environment, while still meeting the delivery timeline.
3. A broken member identity model
The most significant structural problem: members who held multiple plans (e.g., Medicare Advantage + Medicaid) each had a separate Member ID per plan, with no linkage between them in the underlying data. This meant a member with two plans effectively existed as two unrelated people in the system — with no way to see both plans in a single authenticated session. This was not a UI problem. It was a data architecture problem that the portal had never solved.

Key Innovation: Universal Person ID
The broken member identity model was the hardest problem to solve — and the one our team solved collaboratively, with me leading the product definition and requirements.
The problem: UCare's data structure assigned a new, independent Member ID for every plan a member enrolled in. A member with Medicare Advantage and Medicaid coverage had two completely separate Member IDs with no backend linkage. The portal had no way to know these were the same person.
The solution we designed: Create a Universal Person ID — a persistent identifier generated at the person level, with all plan-specific Member IDs mapped to it in the backend. This allowed the system to recognize a single authenticated member across multiple plans and surface all of them in one session.
What this unlocked for members: A plan-toggle feature in the portal UI — allowing a member to switch between their plans and view plan-specific data for each:
Data available per planClaims historyBenefits displayEnrollment detailsProvider assignmentsDocuments
Why this mattered beyond UX: This was a data architecture decision with downstream implications for compliance reporting, member communications, and future CRM integration. Getting it right at the portal layer established a reusable identity pattern for the organization.

Solution
A centralized member portal built on a new vendor platform, delivered in 8 months, with authenticated role-based access to self-service features — anchored by the Universal Person ID as its identity foundation.
Core Features
FeatureDescriptionMember EnrollmentGuided digital enrollment flow integrated with EDI 834 processingBenefits DisplayReal-time benefits summary by plan type, pulling from eligibility systems via REST APIProvider SearchIn-network provider directory with location, specialty, and acceptance filtersDocument UploadHIPAA-compliant document submission with intake trackingMember Self-ServiceAddress updates, PCP changes, and preference management without agent intervention

My Role
As the Product Owner and Senior Business Analyst, I owned this product from discovery through post-launch — with no dedicated PM above me.
What I did:

Defined the product vision, feature roadmap, and release prioritization across all member populations
Championed and defined product requirements for the Universal Person ID architecture as the solution to fragmented member identity
Translated CMS compliance requirements and HIPAA constraints into actionable user stories and acceptance criteria
Managed the vendor relationship and cross-functional development team through agile delivery cycles
Led UAT planning and execution, coordinating with compliance, operations, and clinical stakeholders
Drove alignment across legal, IT, and member services to resolve competing priorities and unblock delivery


Technical Architecture & Integrations
Member Portal (Web)
       │
       ├── FHIR-Formatted REST APIs → Enrollment, Eligibility, Benefits,
       │                              Claims, Messaging (unified API layer)
       ├── SSO/IAM   → Secure Member Authentication
       └── Third-Party Vendor Integrations → Provider Directory, Document Management

Universal Person ID Layer
       │
       ├── Member ID (Medicare Advantage Plan)  ─┐
       ├── Member ID (Medicaid Plan)             ├── Linked to one Person ID
       └── Member ID (Commercial Plan)          ─┘
Key technical touchpoints:

FHIR-formatted REST APIs — a unified API layer using FHIR data standards across all member data domains: enrollment, eligibility, benefits, claims, and messaging. All portal features consumed data through this single consistent API structure rather than domain-specific integrations
SSO / Identity Management — HIPAA-compliant member authentication and session management
Universal Person ID — backend identity layer linking all plan-specific Member IDs to a single person record


Compliance & Privacy
All features were designed and delivered within a strict HIPAA/PHI compliance framework:

Role-based access control (RBAC) scoped to member-specific data only
Audit logging for all member data access events
PHI data handling requirements embedded in acceptance criteria and UAT scripts
Legal and compliance sign-off gates at each release milestone


Outcomes & Impact

Hit the 8-month hard deadline — full portal migration completed before legacy vendor support cutoff
Led product definition of the Universal Person ID — a new backend identity layer linking fragmented Member IDs across plans, enabling multi-plan self-service for the first time
Delivered five self-service features across all member populations, replacing phone-dependent workflows
Achieved HIPAA compliance across all features with zero post-launch compliance findings
Established a reusable identity pattern — the Universal Person ID became the foundational data model for downstream CRM and member communications work


Key Learnings & PM Reflection
This project reinforced that compliance is a product constraint, not an afterthought. The most consequential decisions weren't feature prioritization — they were the moments where CMS requirements, HIPAA obligations, and member experience goals were in tension, and the product had to find a path through all three simultaneously.
The Universal Person ID also reinforced that the most impactful product decisions are often data architecture decisions in disguise. What looked like a UX problem — members can't see both plans — was actually a data identity problem. Solving it at the right layer made everything above it possible.

This repository documents product ownership work completed at a nonprofit Medicare and Medicaid health plan. All data, member records, and system details have been sanitized or synthesized for public sharing.
