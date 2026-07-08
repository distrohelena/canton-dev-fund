## Development Fund Proposal

**Author:** Helena Assis
**Status:** Draft
**Created:** 2026-07-08
**Last Revised:** 2026-07-08
**Champion:** need Champion
**SIG:** daml-tooling

---

## Abstract

This proposal seeks funding to harden, document, expand, and publicly mature an already-usable open-source TypeScript SDK for Canton. The SDK already delivers practical value today and is being used in real projects, but it still requires additional work to reach the level of completeness, stability, test coverage, and documentation needed for broad ecosystem adoption as shared public infrastructure.

The goal is to make Canton's gRPC-driven workflows broadly accessible from TypeScript through a production-grade SDK that supports not only application-facing flows, but also the administrative and operational workflows teams repeatedly need in practice, including topology administration, signatures, and related node operations. This is not speculative greenfield work. The core product exists today because substantial engineering effort has already been invested to retire the discovery risk, prove the architecture, and make the SDK usable in real projects.

That prior investment should not be interpreted as "the work is already free." It means the Development Fund can support a lower-risk proposal whose remaining work is concrete, bounded, and high leverage for the broader ecosystem: full practical coverage of important gRPC workflows, documentation, examples, compatibility work, production hardening, and public adoption support.

This proposal aligns directly with the Development Fund's interest in developer tooling, reference implementations, and infrastructure that lowers adoption friction for teams building on Canton.

---

## Specification

### 1. Objective

Full delivery of this proposal will result in a production-grade, open-source TypeScript SDK that enables developers and operators to interact with Canton through a consistent, documented, and well-tested TypeScript surface across both application and administrative workflows.

The intended outcome is not merely a basic client wrapper. It is a public SDK that supports the workflows teams actually need in practice:

- core gRPC interactions
- topology-related administration
- signing-related operations
- other relevant Canton node capabilities exposed through gRPC
- compiler-style DAML artifact tooling for TypeScript developers working with `DAR` and `DALF`
- examples, documentation, and tests sufficient for third parties to adopt the SDK without private onboarding

### 2. Problem and Ecosystem Value

TypeScript is already a natural implementation language for integration services, internal tools, application backends, automation, and developer tooling. Despite that, teams working with Canton still face repeated integration overhead when they need deeper or broader access to Canton gRPC workflows, especially once their needs move beyond narrow application flows into administrative and operational tasks.

In practice, this creates several recurring problems:

- teams write bespoke wrappers for the same gRPC workflows
- administrative flows such as topology and signatures remain harder to access ergonomically
- onboarding still depends too much on private context and trial-and-error
- coverage gaps and weak examples slow down adoption by otherwise qualified engineering teams

The ecosystem value of this proposal is to replace repeated one-off integration work with a shared, reusable public SDK that lowers total cost of ownership for builders and operators who prefer or require TypeScript.

### 3. Current Status

The SDK is already usable today. It is published in a public GitHub repository at `https://github.com/distrohelena/canton-typescript-sdk`, distributed as an npm package at `https://www.npmjs.com/package/@distrohelena/canton-typescript-sdk`, and is already in use in real projects, although some of that usage cannot be named publicly due to NDA constraints.

The current public SDK already includes more than a thin transport wrapper:

- a shared `CantonClient` with split Ledger, Ledger Admin, and Participant Admin endpoint surfaces
- both `grpc` and `json` transports under one public API shape
- gRPC external signing through `ICommandSigner`
- implemented service coverage across version, health, command submission, active contracts, updates, party management, user rights, package management, participant package inspection, and participant status
- generated gRPC bindings and a public service model shaped around real Ledger API and admin service boundaries
- a separate `canton-typescript-sdk/daml-lf` subpath that already loads `DAR` and `DALF` artifacts, decodes LF `2.x`, builds an immutable package/module/definition model, and exposes workspace, compilation, symbol-resolution, and semantic-query layers
- a separate `canton-typescript-sdk/daml-interface` subpath that generates in-memory TypeScript binding projects from compiled DAML artifacts and can write those generated bindings to disk or drive a CLI flow

The DAML-LF front-end is especially important because it establishes a path beyond request/response transport code. It already behaves as a compiler-style, Roslyn-like semantic surface over compiled DAML artifacts: immutable model types, explicit symbols, workspace compilation, and interpreter-oriented semantic queries are present today. Real DAML-LF execution is not complete yet, but the interpreter scaffold and semantic contracts already exist, which substantially de-risks future interpreter work.

This matters for the proposal in two ways:

- the primary technical risk of "can this be built at all?" has already been retired
- the grant is funding completion, hardening, coverage, and adoption of a proven base rather than paying for an unvalidated idea

This proposal should therefore be read as a public-infrastructure acceleration request, not a discovery-phase request.

### 4. Implementation Mechanics

The SDK will be matured as a cohesive public product with the following workstreams:

#### Workstream A: Practical gRPC coverage expansion

Expand coverage of important Canton gRPC workflows so that the SDK is useful across the operational surface that teams actually encounter. This includes application-facing flows as well as administrative operations such as topology management, signatures, and related node interactions.

The target is practical completeness rather than a narrow happy-path wrapper. The SDK should meaningfully reduce the need for downstream teams to drop into ad hoc low-level integration code for common real-world workflows.

This workstream also includes continuing the transition from generated low-level gRPC bindings into a higher-level public SDK surface that feels coherent across:

- Ledger API service clients
- Ledger Admin and Participant Admin flows
- signing and topology-oriented operations
- package and artifact inspection workflows that connect the transport SDK with the DAML tooling surfaces

#### Workstream B: Stability, compatibility, and production hardening

Improve the reliability of the SDK through:

- stronger automated testing
- broader coverage of supported flows
- better error handling
- compatibility validation against current Canton versions
- cleanup of unstable edges that still rely on implicit author knowledge
- hardening of the DAML-LF and interface-generation surfaces so they can be treated as supported public tooling rather than internal experiments

#### Workstream C: Documentation and adoption package

Package the SDK for broader public use with:

- onboarding documentation
- reference documentation
- quickstarts
- worked examples for both common and administrative flows
- documentation for the `daml-lf` parser and `daml-interface` generator surfaces
- clearer explanation of the interpreter roadmap: what already exists today, what is scaffolded, and what remains for full DAML-LF execution
- versioning and usage guidance that helps third parties adopt the SDK independently

### 5. Out of Scope

This proposal does not seek to fund:

- a graphical explorer or operational UI
- a wallet product
- proprietary integrations for private clients
- a claim of official status or replacement of Digital Asset-maintained tooling

The proposal is specifically about delivering a strong shared TypeScript infrastructure layer for the ecosystem.

### 6. Sustainability

The SDK will remain open-source and maintained after the grant period by the proposal author through the public repository. The grant funds a focused phase of coverage expansion, hardening, documentation, and adoption support; it does not represent a plan to abandon the project after milestone completion.

Because the codebase already exists and is already in practical use, the sustainability story is stronger than for a greenfield proposal: maintenance continues from an active base rather than from a one-off prototype.

### 7. Milestones

#### M1: Coverage Expansion and API Completion

**Funding:** 250,000 CC

Deliver:

- expanded support for the most important missing or incomplete Canton gRPC workflows
- clear surface area for administrative operations, including topology and signatures
- updated examples demonstrating representative flows

Acceptance criteria:

- important targeted workflows are publicly documented as supported
- repository examples cover both application and administrative usage
- third-party readers can identify the supported surface without private explanation

#### M2: Hardening, Stability, and Compatibility

**Funding:** 200,000 CC

Deliver:

- stronger automated tests and improved coverage
- compatibility validation against the targeted Canton version range
- hardening of error handling, API consistency, and operational reliability

Acceptance criteria:

- automated test suite materially improved from current baseline
- compatibility expectations documented in the repository
- identified unstable behaviors from the pre-grant version addressed or documented

#### M3: Documentation, Onboarding, and Public Adoption Package

**Funding:** 150,000 CC

Deliver:

- structured public documentation
- quickstarts and worked examples
- clearer onboarding path for new adopters
- public release packaging suitable for broader ecosystem consumption

Acceptance criteria:

- a new user can follow the published documentation to complete at least one meaningful integration flow
- examples are public, maintained, and aligned with the documented APIs
- repository presents the SDK as a coherent public product rather than author-only infrastructure

#### M4: Independent Audit / Security Review and Remediation

**Funding:** 160,000 CC

Deliver:

- independent review of relevant SDK security and reliability concerns
- remediation of findings within the funded scope
- publication of an appropriate summary or disposition of findings

Acceptance criteria:

- review completed by an external reviewer
- in-scope findings triaged and addressed
- remediation status documented for reviewers and the community

### 8. Funding Request

Total requested funding: **760,000 CC**

Breakdown:

- M1: 250,000 CC
- M2: 200,000 CC
- M3: 150,000 CC
- M4: 160,000 CC

At a reference price of **0.1263 USD / CC**, this is approximately **95,988 USD** in total, including the independent review milestone.

### 9. Alignment with Canton Priorities

This proposal aligns with several current Development Fund priorities:

- **App Building and Developer Experience:** reduces developer friction and lowers the cost of building TypeScript-based integrations on Canton
- **Stability and Maintainability:** replaces bespoke integration code with a shared maintained public SDK
- **Security and Resilience:** includes an independent review and remediation milestone

### 10. Why This Proposal Is a Good Fit for the Fund

This proposal creates a shared public good rather than funding private bespoke work. It expands the usable Canton tooling surface for one of the most important integration languages in the ecosystem. It is also lower risk than a greenfield proposal because the author has already paid the cost of discovery, iteration, and product validation.

That prior effort is part of the reason the proposal is attractive: the fund is not being asked to subsidize experimentation with unknown feasibility. It is being asked to help convert a working, high-effort, already-proven base into durable public infrastructure that more teams can rely on.
