# Data Flow Diagram — Member Portal Modernization

## Portal Integration Architecture

```mermaid
flowchart TD
    Member(["👤 Member (Web)"])
    Portal["Member Portal\n(New Vendor Platform)"]
    API["Unified FHIR-Formatted REST API Layer"]
    Enroll["Enrollment\nSystem"]
    Eligibility["Eligibility &\nBenefits System"]
    Claims["Claims\nSystem"]
    Messaging["Messaging\nSystem"]
    SSO["SSO / IAM\n(Authentication)"]
    PersonID["🔑 Universal Person ID Layer\nLinks all plan-specific Member IDs to one person record"]
    MA["Member ID\n(Medicare Advantage)"]
    MC["Member ID\n(Medicaid)"]
    COM["Member ID\n(Commercial)"]

    Member -->|"Authenticated session"| Portal
    Portal -->|"FHIR REST APIs"| API
    Portal -->|"Secure login"| SSO
    API --> Enroll
    API --> Eligibility
    API --> Claims
    API --> Messaging
    Portal --> PersonID
    PersonID --> MA
    PersonID --> MC
    PersonID --> COM
```

---

## Universal Person ID — Before & After

### Before: Fragmented Identity Model

```mermaid
flowchart LR
    Jane["Member: Jane Doe"]
    MA1["Member ID: MA-00123\n(Medicare Advantage)"]
    MC1["Member ID: MC-00456\n(Medicaid)"]
    COM1["Member ID: CM-00789\n(Commercial)"]

    Jane --- MA1
    Jane --- MC1
    Jane --- COM1
    MA1 -. "No linkage" .- MC1
    MC1 -. "No linkage" .- COM1
```

> Portal sees 3 separate people. No cross-plan view possible.

---

### After: Universal Person ID Model

```mermaid
flowchart TD
    Jane["Member: Jane Doe"]
    PID["🔑 Person ID: PID-99999\n(Universal Person ID)"]
    MA2["Member ID: MA-00123\nMedicare Advantage"]
    MC2["Member ID: MC-00456\nMedicaid"]
    COM2["Member ID: CM-00789\nCommercial"]

    Jane -->|"Single login"| PID
    PID --> MA2
    PID --> MC2
    PID --> COM2
```

> Single authentication. Member toggles between plans. Per-plan data loads dynamically.

---

## Plan Toggle Data Flow

```mermaid
flowchart TD
    Auth["Member authenticates\nPerson ID resolved"]
    Toggle["Member selects plan\nvia plan toggle UI"]
    PlanA["Medicare Advantage"]
    PlanB["Medicaid"]
    PlanC["Commercial"]
    Data["Per-plan data loads via FHIR REST API\nusing plan-specific Member ID"]
    Items["Claims · Benefits · Enrollment · Providers · Documents"]

    Auth --> Toggle
    Toggle --> PlanA & PlanB & PlanC
    PlanA & PlanB & PlanC --> Data
    Data --> Items
```

---

*Diagram sanitized — system names, vendor identities, and internal identifiers removed.*
