---
name: C2PA (Content Credentials)
field: Content provenance for AI-generated media
category: ownership
foundation:
last_updated: 2026-10-27
confidence: grounded (spec); secondary (adoption news)
website: "https://c2pa.org"
---

# C2PA (Content Credentials)

## What it does

An open standard for tamper-evident provenance metadata on media. It does
not judge content as good or bad, only whether assertions are validly
signed and bound to the asset.

## Architecture summary

- **Assertions** bundle into a signed **claim**; claim + signature +
  assertions form a **manifest**.
- **Hard bindings** (cryptographic hashes) tie a manifest to exact bytes;
  **soft bindings** (fingerprints, invisible watermarks) let a stripped
  manifest be recovered. The spec forbids using a soft binding as a hard one.
- Trust is anchored in X.509 signer certificates and a C2PA Trust List;
  time-stamps keep manifests valid after credentials expire.
- Adoption (secondary): OpenAI joined the steering committee and pledged
  SynthID alongside Content Credentials on 2026-05-19; EU AI Act
  Article 50 is reported enforceable from 2026-08-02.

## Built on (foundational lineage)

Informed by Project Origin and the Content Authenticity Initiative (per
the spec). No academic foundation filed.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Integrity | Hash binding + signature | C2PA spec 2.2 |
| Trust | Trust List of certificate anchors | C2PA spec 2.2 |
| Opt-in | Yes | C2PA spec 2.2 |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | SIGNATURE | Signed claims bound to exact bytes by hash; proves who asserted what, not whether it is true | C2PA spec 2.2, grounded |
| Trust rests on | CERTIFICATE ISSUERS | X.509 signer certificates anchored in the C2PA Trust List | C2PA spec 2.2, grounded |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | STANDARDS | Informed by Project Origin and the Content Authenticity Initiative; no paper | C2PA spec, grounded |
| Maturity | LIVE | Published spec (2.2); OpenAI joined the steering committee in May 2026 | C2PA spec; adoption from secondary sources, inferred |
| On-chain | NO CHAIN | Centralized trust via a certificate list, unlike crypto-native registries | Known limitations; classification inferred |

## Known limitations

- Metadata is lost if stripped; soft bindings and watermarks are the
  fallback. SynthID covers only Google outputs and is detected with
  proprietary infrastructure (secondary source).
- Only ~100k of 580k characters of the spec were read; the threat model
  lives in separate Security Considerations and Harms Modelling docs.
- Centralized trust (a certificate list), unlike the crypto-native
  approaches in `/systems/story-protocol.md`.
