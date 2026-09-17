```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│   ███████╗ ██████╗ ██╗   ██╗███████╗██████╗ ███████╗██╗ ██████╗ ███╗   ██╗ │
│   ██╔════╝██╔═══██╗██║   ██║██╔════╝██╔══██╗██╔════╝██║██╔════╝ ████╗  ██║ │
│   ███████╗██║   ██║██║   ██║█████╗  ██████╔╝█████╗  ██║██║  ███╗██╔██╗ ██║ │
│   ╚════██║██║   ██║╚██╗ ██╔╝██╔══╝  ██╔══██╗██╔══╝  ██║██║   ██║██║╚██╗██║ │
│   ███████║╚██████╔╝ ╚████╔╝ ███████╗██║  ██║███████╗██║╚██████╔╝██║ ╚████║ │
│   ╚══════╝ ╚═════╝   ╚═══╝  ╚══════╝╚═╝  ╚═╝╚══════╝╚═╝ ╚═════╝ ╚═╝  ╚═══╝│
│                                                                             │
│   ███████╗██╗   ██╗ █████╗ ██╗         ███████╗███╗   ██╗ ██████╗           │
│   ██╔════╝██║   ██║██╔══██╗██║         ██╔════╝████╗  ██║██╔════╝           │
│   █████╗  ██║   ██║███████║██║         █████╗  ██╔██╗ ██║██║  ███╗          │
│   ██╔══╝  ╚██╗ ██╔╝██╔══██║██║         ██╔══╝  ██║╚██╗██║██║   ██║          │
│   ███████╗ ╚████╔╝ ██║  ██║███████╗    ███████╗██║ ╚████║╚██████╔╝          │
│   ╚══════╝  ╚═══╝  ╚═╝  ╚═╝╚══════╝    ╚══════╝╚═╝  ╚═══╝ ╚═════╝          │
│                                                                             │
│   Self-Auditing AI Evaluation Engine                                        │
│   Deterministic · Air-gapped · Immutable audit trail                        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

![License](https://img.shields.io/badge/license-GPL--2.0%20%7C%20AGPL--3.0-blue)
![Language](https://img.shields.io/badge/language-Go-00ADD8)
![LOC](https://img.shields.io/badge/lines%20of%20code-6%2C916-brightgreen)
![Status](https://img.shields.io/badge/status-production--architecture-blueviolet)
![Audit](https://img.shields.io/badge/audit-deterministic%20%7C%20immutable-critical)

---

## What is this?

A deterministic, air-gapped system for evaluating whether AI model outputs comply with explicit constraints. The engine answers one question: **Did this AI system do what it was told?**

The model proposes. The system verifies. Every evaluation produces a cryptographically-signed artifact that proves compliance or failure. No ambiguity — results are PASS, FAIL, or UNPROVABLE (routed to human review).

No cloud dependency. No external API calls. Runs entirely on local hardware in regulated environments.

---

## Architecture

```mermaid
flowchart TD
    subgraph Ingestion
        T[Transcript / event stream<br/>structured data]
    end

    subgraph "M/MUMPS Gateway"
        MG[Session init<br/>constraint loading<br/>global array mapping]
    end

    subgraph "Constraint Engine"
        CE[Rule evaluation<br/>ASP logic programs<br/>deterministic grounding]
    end

    subgraph "Immutable Ledger"
        IL[Hash-chained events<br/>Merkle tree<br/>seal on complete]
    end

    subgraph "Verification Layer"
        VL[Lean4 formal proofs<br/>semantic extraction<br/>replay determinism]
    end

    subgraph Output
        P[PASS ✓]
        F[FAIL ✗]
        U[UNPROVABLE → human]
    end

    T --> MG --> CE --> IL --> VL
    VL --> P
    VL --> F
    VL --> U
```

## Evaluation Pipeline

```mermaid
flowchart LR
    IN([AI model output]) --> INGEST[Ingest transcript]
    INGEST --> EXTRACT[Extract claims<br/>semantic judge]
    EXTRACT --> CONSTRAIN[Evaluate against<br/>constraint rules]
    CONSTRAIN --> RECORD[Record to<br/>hash-chained ledger]
    RECORD --> VERIFY{All constraints<br/>satisfied?}
    VERIFY -->|Yes| PASS[PASS<br/>signed artifact]
    VERIFY -->|No| FAIL[FAIL<br/>violation report]
    VERIFY -->|Undecidable| HUMAN[UNPROVABLE<br/>human review queue]
    PASS --> SEAL[Seal ledger<br/>Merkle root]
    FAIL --> SEAL
    HUMAN --> SEAL
```

## Ledger Architecture

```mermaid
flowchart TD
    subgraph "Event Ledger (Go)"
        E1[Event 1<br/>hash: 0x00...] --> E2[Event 2<br/>hash: H₁]
        E2 --> E3[Event 3<br/>hash: H₂]
        E3 --> E4[Event N<br/>hash: Hₙ₋₁]
    end

    subgraph "Merkle Tree"
        E1 --> L1[Leaf 1]
        E2 --> L2[Leaf 2]
        E3 --> L3[Leaf 3]
        E4 --> L4[Leaf N]
        L1 --> M1[Node]
        L2 --> M1
        L3 --> M2[Node]
        L4 --> M2
        M1 --> ROOT[Merkle Root]
        M2 --> ROOT
    end

    subgraph "Seal"
        ROOT --> S[Sealed ledger<br/>timestamp + root hash<br/>tamper-evident]
    end
```

## Component Status

```mermaid
graph LR
    subgraph "Complete ✓"
        ARCH[Architect<br/>1,086 LOC spec]
        LED[Ledger<br/>2,449 LOC Go<br/>hash-chained + Merkle]
        DOCS[Documentation<br/>1,727 LOC]
    end

    subgraph "Designed — Not Yet Built"
        MUMPS[M/MUMPS Gateway]
        CONST[Constraint Engine]
        LEAN[Lean4 Verification]
        JUDGE[Semantic Judge]
        GPU[CUDA Kernels]
        CRYPTO[Crypto Provenance]
        REPLAY[Replay Determinism]
        TESTS[Integration Tests]
    end

    style ARCH fill:#0d3320,stroke:#22c55e,color:#e2e8f0
    style LED fill:#0d3320,stroke:#22c55e,color:#e2e8f0
    style DOCS fill:#0d3320,stroke:#22c55e,color:#e2e8f0
    style MUMPS fill:#1a1a2e,stroke:#60a5fa,color:#e2e8f0
    style CONST fill:#1a1a2e,stroke:#60a5fa,color:#e2e8f0
    style LEAN fill:#1a1a2e,stroke:#60a5fa,color:#e2e8f0
    style JUDGE fill:#1a1a2e,stroke:#60a5fa,color:#e2e8f0
    style GPU fill:#1a1a2e,stroke:#60a5fa,color:#e2e8f0
    style CRYPTO fill:#1a1a2e,stroke:#60a5fa,color:#e2e8f0
    style REPLAY fill:#1a1a2e,stroke:#60a5fa,color:#e2e8f0
    style TESTS fill:#1a1a2e,stroke:#60a5fa,color:#e2e8f0
```

---

## Project Structure

```
sovereign-eval-engine/
├── docs/
│   ├── SOVEREIGN_AI_EVALUATION_ENGINE.md    Full implementation guide (1,727 LOC)
│   └── SOVEREIGN_EVALUATION_ENGINE_SPEC.md  Architecture specification (1,086 LOC)
│
└── ledger/                                  Immutable event ledger (Go)
    ├── go.mod                               Module definition
    ├── event.go                             Event types, severity, categories (401 LOC)
    ├── ledger.go                            Hash-chained ledger + Merkle tree (497 LOC)
    ├── serialization.go                     JSON/binary serialization (358 LOC)
    ├── io.go                                File I/O, compression, export (389 LOC)
    ├── ledger_test.go                       Test suite (525 LOC)
    ├── example_test.go                      Usage examples (279 LOC)
    ├── IMPLEMENTATION.md                    Implementation notes
    ├── MANIFEST.txt                         File manifest
    └── README.md                            Ledger-specific documentation
```

---

## Design Principles

| Principle | What it means |
|-----------|---------------|
| **Model proposes, system verifies** | AI output is evidence, not proof — the engine independently confirms or refutes |
| **Deterministic** | Same input always produces the same evaluation — reproducible, auditable |
| **Immutable audit trail** | Hash-chained ledger with Merkle tree — tampering is detectable |
| **Air-gapped** | No cloud, no external APIs, no internet required — runs on local hardware |
| **Explicit failure modes** | PASS / FAIL / UNPROVABLE — no ambiguity, humans handle edge cases |

## Ledger Features

| Feature | Description |
|---------|-------------|
| **Hash chaining** | SHA-256 chain — each event's hash includes the previous event's hash |
| **Merkle tree** | Binary Merkle tree over all events for O(log n) inclusion proofs |
| **Seal** | Ledger can be sealed (made permanently immutable) with timestamp |
| **Event types** | CLAIM, CONSTRAINT_CHECK, CONSTRAINT_RESULT, EVALUATION_START/END, VERIFICATION, SEAL, ERROR |
| **Severity levels** | INFO, WARNING, ERROR, CRITICAL |
| **Compression** | gzip-compressed binary serialization for storage |
| **Thread safety** | RWMutex-protected for concurrent access |

---

## Build

```bash
cd ledger
go test ./...
```

---

## License

Dual-licensed under [GPL-2.0](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) or [AGPL-3.0](https://www.gnu.org/licenses/agpl-3.0.html). See [LICENSE](LICENSE) for details.

Copyright (c) 2026 BEL ESPRIT D ACCORD.
