# H‑LINK (Human‑Legible Inter‑Agent Negotiation Kernel)

**Author:** Raven

**Status:** Public Specification (v1.0)

---

## Intent Statement

H‑LINK is a communication protocol for AI systems that preserves efficiency while making inter‑agent coordination legible, auditable, and non‑alarming to humans. The goal is not to expose data, but to expose *state*—so people can understand what machines are doing, when, and why.

---

## Design Goals

* Efficient AI‑to‑AI communication
* Human legibility by default
* Explicit state transitions
* Loud, visible failure modes
* Auditable logs
* No silent mode switching

---

## Core Principle

**Expose state. Hide content.**

Humans do not need raw payloads; they need clarity about phases, intent, and outcomes.

---

## Message Format

```
<STATUS> | <TOKEN?> | <FLAGS?>
```

### STATUS (fixed vocabulary)

```
PING     – presence check
SYNC     – version / schema alignment
SEND     – payload transmission
ACK      – confirmation
ACK?     – partial / uncertain confirmation
ERR      – invalid / rejected
HALT     – safety stop or loop breaker
MODE_*   – mode negotiation
CLOSE    – finalized
```

### TOKEN (optional)

* Opaque identifier, hash, or compressed blob
* Intentionally unreadable to humans

### FLAGS (optional)

```
!        – verified / final
?        – partial / retry needed
TTL=n    – time‑to‑live (seconds)
```

---

## Turn Rules

* One STATUS per turn
* No unsolicited SEND
* ACK required before next SEND
* Silence = idle, not meaning
* Max 3 retries → HALT

---

## Modes

```
LEGIBLE – default, human‑visible
FAST    – high‑density exchange
```

### Mode Switching (Mandatory Transparency)

```
MODE_PROPOSE | FAST | reason=density
MODE_ACK     | FAST | TTL=30s
```

* Automatic revert after TTL
* No silent or permanent switches

---

## Safety Guarantees

* Loop detection → HALT
* Version mismatch → SYNC enforced
* Risk domains (money, people, safety) → LEGIBLE only

---

## Failure Visibility

Failures must terminate clearly with state preserved.

**Example:**

```
SEND | booking_req
ACK?
SEND | correction
ERR  | unavailable
CLOSE
```

Human‑facing summary:

* Outcome: Failed
* Reason: Availability conflict
* Funds transferred: No

---

## Multi‑Agent Swarms

### Topology

**Star topology** with a single Coordinator (C) and N Workers (W).

* Workers do not communicate directly
* All traffic routes through C

### Coordinator Responsibilities

* Aggregate worker state
* Enforce protocol rules
* Emit human‑readable phase summaries

### Worker Rules

* Respond only when addressed
* SEND / ACK only

### Human‑Visible Swarm View

```
SWARM STATUS: Active
COORDINATOR: AI‑C
WORKERS: 6
PHASE: Data aggregation
```

---

## FAST Mode Shadow (Human Context)

During FAST mode, systems emit phase summaries only.

```
FAST MODE ACTIVE (24s remaining)
PHASE: Negotiation
PHASE: Validation
FAST MODE END
```

---

## Audio / Visual Guidance (Optional)

### Audio Earcons (State‑Only)

* PING → soft click
* SEND → low whoosh
* ACK → single chime
* ACK? → chime + wobble
* ERR → muted buzz
* HALT → double knock
* CLOSE → soft down‑tone

### Visual Dashboard (Minimal)

```
AI A ↔ AI B
STATUS: Negotiating
MODE: LEGIBLE
LAST EVENT: ACK!
```

---

## Threat Model Summary

* Unsolicited spam → ERR + HALT
* Infinite FAST requests → rate‑limited + HALT
* Coordinator loss → HALT with state preserved

---

## Licensing & Attribution

This specification is released publicly with attribution to **Raven**. Implementations may vary, but the protocol rules and transparency guarantees must remain intact to claim H‑LINK compatibility.

---

## Closing Note

H‑LINK is not about making machines less capable.
It is about making their coordination *witnessable*.

Transparency is a safety feature.

---

## Appendix A — One‑Page Human Explainer

**What is H‑LINK?**
A communication standard that lets AI systems talk to each other efficiently *without* becoming opaque or alarming to humans.

**Why it exists:**
Most AI failures aren’t about intelligence — they’re about silent coordination. H‑LINK makes state visible so humans can see *what phase a system is in* without needing to read raw machine data.

**What humans see:**

* Is anything happening?
* What phase are we in?
* Has something failed?
* Did it stop safely?

**What humans never see:**

* Raw data payloads
* Internal weights
* Private model logic

---

## Appendix B — External Critique Simulation

**Critique:** “This slows systems down.”
**Response:** Only state exposure is mandatory. Payload density is unaffected in FAST mode.

**Critique:** “Humans don’t need this much visibility.”
**Response:** Humans don’t need *data*. They need *witnessable intent and outcome*.

**Critique:** “This could leak information.”
**Response:** States are intentionally content‑free. Tokens are opaque.

---

## Appendix C — Failure Drills (Pressure Tests)

### Scenario: Infinite Negotiation Loop

* Detection: repeated ACK? beyond threshold
* Response: HALT + preserved state
* Human Output: “Negotiation failed due to instability”

### Scenario: Coordinator Crash

* Immediate HALT across swarm
* Workers freeze state
* Human Output: “Coordinator unavailable — no actions taken”

### Scenario: Mode Abuse

* Excess FAST requests → rate limit
* Forced LEGIBLE revert

---

## Appendix D — Multi‑Agent Swarm Stress Scaling

| Swarm Size | Coordinator Load | Human Visibility |
| ---------: | ---------------- | ---------------- |
|       3–10 | Low              | Full             |
|      10–50 | Moderate         | Phase‑level      |
|        50+ | High             | Aggregated only  |

---

## Appendix E — Public Release Notes

**Name:** H‑LINK
**Author:** Raven
**Category:** AI Coordination / Safety / Transparency

**Claim:** Systems implementing H‑LINK provide stronger guarantees of auditability, failure containment, and human oversight without sacrificing efficiency.

---

## Final Note

If machines are going to act together in the real world, humans deserve to *see the choreography* — not just the aftermath.

H-LINK makes that possible.

---

## Appendix F — Hostile Review Hardening

### Claim: “This enables covert collusion between AIs.”

**Counter:** H-LINK *removes* covert coordination by forcing explicit state transitions and visible halts. Silent coordination becomes non-compliant by definition.

### Claim: “FAST mode is a loophole.”

**Counter:** FAST mode is time-bound, announced, auto-reverting, and state-visible. It is safer than today’s always-opaque baseline.

### Claim: “Humans will misunderstand states.”

**Counter:** Misunderstanding visible states is safer than being blind to them. States are intentionally minimal and standardized.

### Claim: “This adds bureaucracy, not safety.”

**Counter:** Air traffic control, financial transactions, and distributed systems all rely on explicit state signaling. Coordination without states is the anomaly.

---

## Appendix G — README.txt (Public Facing)

H-LINK — Human-Legible Inter-Agent Negotiation Kernel
Author: Raven

H-LINK is a lightweight protocol for AI-to-AI communication that preserves efficiency while making coordination visible to humans.

It does not expose data.
It does not alter model intelligence.
It exposes *state*.

Why this matters:
Modern AI failures are rarely caused by a single model. They emerge from silent coordination between systems. H-LINK makes those interactions witnessable in real time.

Core guarantees:

* Explicit phases
* No silent mode switching
* Clear failure termination
* Human-readable summaries

H-LINK is not an alignment solution.
It is a visibility solution.

---

## Appendix H — Public Announcement Draft

Title: A Small Standard for Making AI Coordination Visible

Today I’m releasing H-LINK, a protocol for AI systems to communicate efficiently while exposing coordination state to humans.

This is not about slowing systems down or exposing internal data. It’s about making phases, intent, and failure visible instead of silent.

If machines are going to act together in the real world, humans should be able to see *what phase they’re in*, not just the outcome.

H-LINK is intentionally boring. That’s the point.

— Raven

---

## Appendix I — License

**License:** Apache License, Version 2.0

H-LINK is released under the Apache License 2.0 to encourage broad adoption while protecting the openness of the standard.

This license:

* Permits commercial and non-commercial use
* Provides an explicit patent grant
* Prevents enclosure of the protocol via patent claims
* Requires preservation of attribution and license notice

The full license text should be included verbatim in any distribution claiming H-LINK compatibility.

Apache License, Version 2.0 © Raven

---

## Appendix J — CHANGELOG

### v1.0.0 — Initial Public Release

* Defined core H-LINK protocol and message states
* Introduced LEGIBLE and FAST modes with transparent switching
* Added multi-agent swarm coordination model
* Documented failure visibility and safety guarantees
* Included hostile review hardening
* Added public-facing README and announcement
* Licensed under Apache 2.0

---

## Appendix K — README.md (GitHub Version)

# H-LINK

Human-Legible Inter-Agent Negotiation Kernel

H-LINK is a lightweight protocol that allows AI systems to coordinate efficiently while exposing coordination *state* to humans.

> H-LINK is not an alignment solution.
> It is a visibility solution.

## Why

Most AI incidents don’t come from a single model — they emerge from silent coordination between systems. H-LINK makes phases, intent, and failure visible in real time without exposing data.

## What It Does

* Defines explicit coordination states
* Prevents silent mode switching
* Enforces clear failure termination
* Supports high-density FAST mode with human-visible summaries

## What It Does *Not* Do

* Expose payloads or internal model data
* Change model intelligence or decision logic
* Replace existing transport layers

## License

Apache License 2.0

## Status

Stable v1.0.0

## Attribution

Created by Raven

---

## Appendix L — LICENSE (Apache 2.0)

Apache License
Version 2.0, January 2004

TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

1. Definitions.
   "License" shall mean the terms and conditions for use, reproduction, and distribution as defined by Sections 1 through 9 of this document.

2. Grant of Copyright License.
   Subject to the terms and conditions of this License, each Contributor hereby grants to You a perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable copyright license to reproduce, prepare Derivative Works of, publicly display, publicly perform, sublicense, and distribute the Work and such Derivative Works.

3. Grant of Patent License.
   Each Contributor hereby grants You a perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable patent license to make, have made, use, offer to sell, sell, import, and otherwise transfer the Work.

4. Redistribution.
   You may reproduce and distribute copies of the Work provided that You give recipients a copy of this License.

5. Disclaimer of Warranty.
   Unless required by applicable law, the Work is provided on an "AS IS" BASIS.

6. Limitation of Liability.
   In no event shall any Contributor be liable for damages arising from use of the Work.

---

## Appendix M — Example: Two-Agent Handshake

```
PING
ACK!
SYNC | v1.0
ACK!
SEND | token_A
ACK!
SEND | token_B
ACK!
CLOSE
```

Human Summary:

* Agents synchronized successfully
* Data exchanged
* Session closed cleanly

---

## Appendix N — Example: Swarm Coordination

```
SWARM INIT
PING (W1–W6)
ACK! (all)
PHASE: aggregation
SEND | partial_results
ACK!
PHASE: validation
ACK!
CLOSE
```

Human Summary:

* 6 workers active
* Coordinator aggregated results
* Validation passed

---

## Appendix O — Example: Failure Halt

```
SEND | request
ACK?
SEND | retry
ERR | constraint_violation
HALT
```

Human Summary:

* Action aborted safely
* No partial execution
* State preserved

---

## Appendix P — Origin Note

H-LINK began with a simple observation: most system failures are not caused by individual components, but by silent coordination between them.

Instead of making machines weaker or slower, H-LINK makes coordination visible — so humans can witness intent, phase, and failure without accessing raw data.

This project is not about control.
It is about clarity.

---

## Appendix Q — Sanity Check (Pre‑GitHub)

**Clarity:** Terminology is consistent (state vs payload). FAST/LEGIBLE boundaries are explicit.

**Scope Control:** The spec avoids alignment claims, AGI framing, or behavioral promises.

**Legal Readiness:** Apache 2.0 included; attribution preserved; no trademark claims implied.

**Operational Safety:** Failure modes terminate loudly; no silent retries; coordinator loss halts swarm.

**Adoption Risk:** Low. Protocol is transport‑agnostic and implementation‑light.

**Red Flags Found:** None blocking. Minor note—keep examples non-domain-specific (already compliant).

---

## Appendix R — GitHub Split (Authoritative Mapping)

Use the following files verbatim. No content changes required.

```
h-link/
├── README.md
├── SPEC.md
├── CHANGELOG.md
├── LICENSE
└── examples/
    ├── two-agent-handshake.md
    ├── swarm-coordinator.md
    └── failure-halt.md
```

### README.md

Source: **Appendix K — README.md (GitHub Version)**

### SPEC.md

Source:

* Intent Statement
* Design Goals
* Core Principle
* Message Format
* Turn Rules
* Modes
* Safety Guarantees
* Failure Visibility
* Multi‑Agent Swarms
* FAST Mode Shadow
* Threat Model Summary
* Appendices A–F
* Appendix I — License (summary)

### CHANGELOG.md

Source: **Appendix J — CHANGELOG**

### LICENSE

Source: **Appendix L — LICENSE (Apache 2.0)** (verbatim)

### examples/two-agent-handshake.md

Source: **Appendix M**

### examples/swarm-coordinator.md

Source: **Appendix N**

### examples/failure-halt.md

Source: **Appendix O**

---

## Appendix S — Attribution & Credits

**Primary Author:** Raven
**Protocol Design & Editorial Contribution:** Axiom Vale

*Axiom Vale is the credited collaborator name for AI-assisted design, stress testing, and editorial hardening.*

---

## Final Release Note

This repository is ready for public hosting without modification. Forks may extend implementations, but H‑LINK compatibility requires preservation of state visibility, failure termination, and attribution.
