<div align="center">

<img src="./banner.png" alt="Kage" width="420" />

# Kage

*The proof steps into the light. Your identity stays in the dark.*

[![Solana](https://img.shields.io/badge/Solana-Anchor-14F195?logo=solana&logoColor=white)](https://www.anchor-lang.com/)
[![Circom](https://img.shields.io/badge/Circom-Groth16-blue)](https://docs.circom.io/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](#)

</div>

The verification is a shadow of you — its shape is enough to prove you're real and valid, while the substance casting it never leaves the dark. **Kage** (影 / かげ — *shadow, silhouette*) is zero-knowledge identity, by design.

---

## Who we are

We build **zero-knowledge identity** infrastructure. Traditional KYC forces you to hand over your name, ID number, and date of birth — and every verifier that stores it becomes a breach waiting to happen.

Kage flips the model: the user holds their own credential, proves a *predicate* about it (e.g. "age ≥ 18", "is a verified resident") on-device, and the verifier learns **only the answer** — never the underlying data.

> **One breach of a traditional KYC store = mass PII leak.**
> **One breach of a Kage verifier = nothing useful.**

---

## How it works

The user holds a signed credential on their device, generates a Groth16 proof of a predicate on-device, and shows it as a QR. The verifier submits the proof to a Solana program, which checks it and rejects replays — learning nothing but the result.

```mermaid
%%{init: {"flowchart": {"htmlLabels": true, "nodeSpacing": 55, "rankSpacing": 55}, "themeVariables": {"fontSize": "18px"}}}%%
flowchart TB
    subgraph issuer["ISSUER — one-time onboarding"]
        ISS["Identity provider<br/>signs credential<br/>maps identity → nullifier secret"]
    end

    subgraph device["USER DEVICE — trust boundary, PII never leaves"]
        direction LR
        KS["Encrypted keystore<br/>signed credential"]
        PR["snarkjs fullProve · Groth16<br/>on-device<br/>proves predicate + valid signature"]
        KS --> PR
    end

    QR["QR PAYLOAD<br/>Groth16 proof + public signals<br/>256 B base64 — no PII"]

    subgraph verifier["VERIFIER — stores no PII"]
        direction LR
        SCAN["Scan QR"]
        TX["Build + submit transaction"]
        SCAN --> TX
    end

    subgraph chain["SOLANA PROGRAM — Anchor"]
        direction LR
        V["Verify<br/>Groth16 proof"]
        IK["Check<br/>issuer pubkey"]
        NUL["Init nullifier PDA<br/>exists → reject replay"]
        EV["Emit<br/>Verified{wallet, slot}"]
        V --> IK --> NUL --> EV
    end

    ISS -->|signed credential| KS
    PR -->|encode| QR
    QR -.->|scan| SCAN
    TX -->|proof + signals| V
    EV -->|pass / fail| TX

    classDef trust fill:#1e1e2e,stroke:#6c6c8a,color:#e8e8f0,stroke-width:2px;
    classDef onchain fill:#0f2a20,stroke:#14F195,color:#d8ffe9,stroke-width:2px;
    classDef neutral fill:#26263a,stroke:#9090a8,color:#e8e8f0,stroke-width:2px;
    class device,KS,PR trust;
    class chain,V,IK,NUL,EV onchain;
    class issuer,ISS,verifier,SCAN,TX,QR neutral;
```

### Privacy contrast

| | Traditional KYC | Kage |
|---|---|---|
| Verifier stores | `{ id, name, address }` | `{ wallet, nullifier, slot }` |
| One breach | mass PII leak | nothing useful |
| Sybil-resistant | ✅ (but linkable everywhere) | ✅ (via nullifier) |
| User reveals | everything | a single `pass` bit |

---

## Principles

- **Data minimization** — verifiers learn the predicate result, nothing more.
- **Self-custody of identity** — PII lives in the user's device keystore, not a central DB.
- **On-chain accountability** — proofs are verified trustlessly; replays are rejected by construction.
- **Honest scope** — we ship demos labeled as demos, and document their limits.

---

<div align="center">

**Built on Solana · Powered by Groth16 zk-SNARKs**

*影 — identity you control, verification you can trust.*

</div>
