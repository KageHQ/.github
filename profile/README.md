<div align="center">

<img src="./banner.png" alt="Kage" />

# Kage

*The proof steps into the light. Your identity stays in the dark.*

[![Solana](https://img.shields.io/badge/Solana-Anchor-14F195?logo=solana&logoColor=white)](https://www.anchor-lang.com/)
[![Circom](https://img.shields.io/badge/Circom-Groth16-blue)](https://docs.circom.io/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](#)

</div>

The verification is a shadow of you. Its shape is enough to prove you're real and valid, while the substance casting it never leaves the dark. **Kage** (影 / かげ, *shadow, silhouette*) is zero-knowledge identity, by design.

---

## Who we are

We build **zero-knowledge identity** infrastructure. Traditional KYC forces you to hand over your name, ID number, and date of birth, and every verifier that stores it becomes a breach waiting to happen.

Kage flips the model: the user holds their own credential, proves a *predicate* about it (e.g. "age ≥ 18", "is a verified resident") on-device, and the verifier learns **only the answer**, never the underlying data.

> **One breach of a traditional KYC store = mass PII leak.**
> **One breach of a Kage verifier = nothing useful.**

---

## How it works

The user holds a signed credential, generates a Groth16 proof of a predicate, and hands it to the verifier. The verifier submits the proof to a Solana program, which checks it and rejects replays, learning nothing but the result.

> **v1 transport:** the user shares a **6-digit relay code**; the verifier app fetches the matching proof from the issuer relay. **QR-based handoff** (proof encoded directly in a QR payload, no relay round-trip) lands in the next update.

**The short version — proving who you are, without revealing who you are:**

<div align="center">

<img src="./simple-architecture.png" alt="Kage simple flow: get a digital ID, your phone makes a proof, share a 6-digit code, it's checked on Solana, verifier learns only valid and 18+" />

</div>

<details>
<summary><b>The technical version</b> — issuer, holder device, relay, verifier, and Solana program</summary>

<div align="center">

<img src="./tech-architecture.png" alt="Kage technical architecture: issuer EdDSA-Poseidon signs a credential; holder device runs snarkjs fullProve (Groth16) behind a PII trust boundary; relay maps a single-use 6-digit code to the proof payload; verifier builds a transaction; Solana program runs Groth16 verify, issuer/scope match, and nullifier PDA replay rejection" />

</div>

</details>

### Privacy contrast

| | Traditional KYC | Kage |
|---|---|---|
| Verifier stores | `{ id, name, address }` | `{ wallet, nullifier, slot }` |
| One breach | mass PII leak | nothing useful |
| Sybil-resistant | ✅ (but linkable everywhere) | ✅ (via nullifier) |
| User reveals | everything | a single `pass` bit |

---

## Status

**v1 — live demo.** End-to-end flow working: on-device Groth16 proving → 6-digit relay handoff → on-chain verify with replay rejection.

| Component | Repo | State |
|---|---|---|
| Circuits (Groth16, predicate + EdDSA sig) | `kage-circuits` | ✅ v1 |
| Issuer (credential signing + relay) | `kage-issuer` | ✅ v1 |
| Solana program (verify + nullifier PDA) | `kage-program` | ✅ v1 |
| Web verifier UI | `kage-web` | ✅ v1 |
| Mobile holder app | `kage-mobile` | ✅ v1 |
| Shared SDK / IDL | `kage-shared` | ✅ v1 |
| End-to-end harness | `kage-e2e` | ✅ v1 |

### Next update

- **QR handoff** — proof encoded directly in a QR payload, dropping the relay round-trip.

---

## Principles

- **Data minimization:** verifiers learn the predicate result, nothing more.
- **Self-custody of identity:** PII lives in the user's device keystore, not a central DB.
- **On-chain accountability:** proofs are verified trustlessly; replays are rejected by construction.
- **Honest scope:** we ship demos labeled as demos, and document their limits.

---

<div align="center">

**Built on Solana · Powered by Groth16 zk-SNARKs**

*影 · identity you control, verification you can trust.*

</div>
