---
name: Google Confidential Federated Learning (TensorFlow Federated + TEE confidentiality)
field: Federated learning / confidential aggregation
category: private-compute
foundation: foundations/secure-aggregation.md
last_updated: 2026-10-20
confidence: grounded
website: "https://research.google/blog/federated-learning-collaborative-machine-learning-without-centralized-training-data/"
---

# Google Confidential Federated Learning

## What it does

Google trains production ML models (e.g. Gboard's next-word prediction) on
data that never leaves users' devices. Each device computes a local model
update on its own data; only the update, not the raw data, is sent to a
server, which aggregates updates across many devices into an improved
global model. The "confidential" layer adds server-side trusted execution
environments (TEEs) so that even the aggregation server itself cannot see
individual updates, with the privacy properties externally auditable rather
than taken on trust.

## Architecture summary

- **Base mechanism**: Federated Averaging: devices compute local SGD
  updates, a server averages them into a new global model, raw data is
  never centralized.
- **Confidentiality layer** ("Confidential Federated Computations", 2024):
  devices encrypt their uploads with keys controlled by a TEE-hosted Key
  Management Service (KMS). The KMS only releases decryption keys to
  specific, pre-approved TEE binaries, so the server operator cannot
  access plaintext updates even with full infrastructure access.
- **External verifiability**: access policies (which binaries may process
  which data) are published to Rekor, Sigstore's public transparency log,
  and the TEE binaries are reproducibly buildable from open-source code,
  so an outside auditor can check that the system is actually doing what
  it claims, not just trusting Google's word for it. This is the piece
  that lets Google claim *externally verifiable* differential privacy
  rather than DP that depends on trusting the service operator.
- Per secondary reporting (MarkTechPost, Oct 2026; primary source page
  returned a 403 during this research pass, so this specific detail is
  less directly confirmed than the rest): this architecture is now live in
  production for Gboard.

## Built on (foundational lineage)

Grounded via a direct citation chain from Google's own federated-learning
announcement, not inference:

- Google's 2017 FL announcement post
  ([research.google/blog](https://research.google/blog/federated-learning-collaborative-machine-learning-without-centralized-training-data/))
  directly links to **"Communication-Efficient Learning of Deep Networks
  from Decentralized Data"** (McMahan, Moore, Ramage, Hampson, y Arcas,
  [arxiv.org/abs/1602.05629](https://arxiv.org/abs/1602.05629), AISTATS
  2017) as the Federated Averaging algorithm itself: the base ML
  mechanism, not the crypto piece.
- The same post directly links to **"Practical Secure Aggregation for
  Privacy-Preserving Machine Learning"** (Bonawitz, Ivanov, Kreuter,
  Marcedone, McMahan, Patel, Ramage, Segal, Seth,
  [eprint.iacr.org/2017/281](http://eprint.iacr.org/2017/281), ACM CCS
  2017) as "a Secure Aggregation protocol that uses cryptographic
  techniques" so the server only ever sees an aggregate, never individual
  updates. This is the actual crypto-for-ML contribution; see
  `/foundations/secure-aggregation.md`.
- Direct authorship continuity: both Kallista Bonawitz (Secure Aggregation
  author) and Brendan McMahan (FedAvg author) are listed authors on the
  2024 "Confidential Federated Computations" paper
  ([arxiv.org/abs/2404.10764](https://arxiv.org/abs/2404.10764)) that
  introduces the TEE-based confidentiality layer: the same people,
  extending their own prior work, not a third party building on it later.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Base algorithm | Federated Averaging (local SGD + server-side averaging) | McMahan et al. 2017, cited directly by Google's FL announcement |
| Aggregation privacy | Cryptographic Secure Aggregation (server sees only the sum/average, never individual updates) | Bonawitz et al. 2017, cited directly by Google's FL announcement |
| Server-side confidentiality | TEE-hosted KMS; decryption keys released only to pre-approved, reproducibly-built binaries | "Confidential Federated Computations," arxiv 2404.10764 |
| External verifiability mechanism | Access policies published to Rekor (Sigstore transparency log); TEE binaries reproducibly buildable from open source | arxiv 2404.10764 |
| Production usage | Gboard (per secondary reporting; primary source page was unreachable this pass, flagged as less directly confirmed) | MarkTechPost, Oct 2026, inferred/secondary, recheck against Google's own primary source when accessible |
| Trust model | Does not require trusting the server operator's word; relies on TEE hardware security + externally auditable transparency log | arxiv 2404.10764 |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | ATTESTATION | TEE-hosted KMS releases keys only to pre-approved, reproducibly built binaries; policies published to Rekor | arxiv 2404.10764, grounded |
| Trust rests on | HARDWARE VENDOR | TEE hardware security plus an externally auditable transparency log, not the operator's word | arxiv 2404.10764, grounded |
| Confidentiality | CRYPTOGRAPHIC + ENCLAVE | Raw data stays on device; Secure Aggregation shows the server only the aggregate; TEEs hide individual updates from the operator | Bonawitz et al. 2017 + arxiv 2404.10764, grounded |
| Lineage | OWN PAPER | FedAvg (AISTATS 2017) and Secure Aggregation (CCS 2017), by Google authors who also wrote the 2024 TEE paper | Google's FL announcement post, grounded |
| Maturity | LIVE | In production for Gboard, per secondary reporting | MarkTechPost, Oct 2026, inferred/secondary |
| On-chain | NO CHAIN | No blockchain component appears in Google's materials | Inferred from absence in the cited sources |

## Known limitations

- TEE-based confidentiality inherits TEE hardware's own threat model:
  side-channel attacks against TEEs are an active research area; this
  entry does not independently assess that risk, it only documents what
  Google's own papers claim.
- The Gboard production-deployment claim in this entry rests on secondary
  reporting, not a directly-fetched primary source (the MarkTechPost
  article returned an HTTP 403 during this research pass); worth
  re-verifying against Google's own blog/papers directly in a future pass.
- This entry does not independently verify the differential-privacy
  epsilon values or noise parameters actually used in production, only
  that the mechanism for achieving verifiable DP is as described in the
  cited papers.
