---
name: Render Network
field: Decentralized GPU / compute marketplace
category: compute-markets
last_updated: 2026-10-20
confidence: inferred (no academic lineage found; patent citation is billing-related, not verification-related)
website: "https://renderfoundation.com"
---

## What it does

Render Network is a decentralized GPU marketplace: idle GPU capacity from
node operators is matched to rendering jobs (3D/VFX) submitted by
creators, paid in the RENDER token. Historically this is a rendering
network, not an AI network. In December 2025 it launched **Dispersed**,
a separate GPU-compute subnet (approved via governance proposals RNP-019
and RNP-021) explicitly marketed for AI workloads, hosting 600+ AI
models at launch, priced at $1.75/hour. ([Solana Compass: Render Network
Launches Dispersed AI Compute
Platform](https://solanacompass.com/learn/breakpoint-25/product-keynote-render-network-render-network-foundation);
[Render Network Foundation Monthly Report: December
2025](https://rendernetwork.medium.com/render-network-foundation-monthly-report-december-2025-43d956808e3f))

**The AI angle is real but recent and structurally separate from the
core network**, not evidence that the original rendering architecture
was AI-specific. Dispersed is general-purpose decentralized GPU compute
being sold into the AI market, not a cryptographically-verified
AI-inference or AI-training primitive like the other systems tracked in
this repo. Treat it as adjacent infrastructure (compute supply), not a
verifiable-AI system.

## Architecture summary

- **Proof-of-Render (PoR)**: the network's anti-fraud / quality-assurance
  mechanism. A node completes a job and submits cryptographic data plus
  a watermarked preview; the job creator visually and technically
  verifies the output before payment release; other nodes reportedly
  spot-check frames before final settlement. ([Web3@KAIST 2025: Render
  Network: Blockchain meets
  AI](https://web3classdao.github.io/kaist2025/reports/render/); general
  sourcing, not Render's own technical docs; see confidence note below)
- Payment is escrow-based: funds are held until PoR verification
  completes.
- **Dispersed** (the AI subnet) aggregates distributed GPUs into a
  single on-demand compute layer for AI/general-purpose workloads,
  separate from the rendering job-matching logic above.

## Built on (foundational lineage)

**No academic paper lineage was found.** The official whitepaper
(renderfoundation.com/whitepaper, v4.0, originally 2017, last updated
2023) is a product/vision document, not an academic-citation-style
paper. The only external reference recoverable from the whitepaper PDF
is a Google Patents citation:

- **US9197642B1, "Token-based billing model for server-side rendering
  service"** ([patents.google.com/patent/US9197642B1](https://patents.google.com/patent/US9197642B1/en)).
  This patent is about billing/tokenization for server-side rendering
  delivery, **not** about Proof-of-Render's verification mechanism or
  any cryptographic technique. Cited here for completeness, not as
  grounding for the verification claims above.

The whitepaper PDF was heavily corrupted in automated extraction
(compressed/binary streams), so some content may not have been
recoverable; this entry should be rechecked against a clean copy of
the PDF if someone wants to confirm nothing else was missed. Proof-of-
Render's actual verification mechanics (watermarking, spot-checks,
escrow) are documented through third-party explainers and community
docs, not found quoted directly from Render's own primary technical
documentation in this pass.

No new foundations/ file created: there is nothing to ground it to.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Core use case | GPU rendering marketplace (3D/VFX); AI compute is a newer, separate subnet | renderfoundation.com, Render Foundation Dec 2025 report |
| Verification mechanism | Proof-of-Render: watermarked preview + node spot-checks + escrowed payment | Third-party explainers (Web3@KAIST), **not confirmed against primary docs** |
| AI-specific offering | "Dispersed" compute subnet, governance-approved (RNP-019, RNP-021), launched Dec 2025, 600+ models at $1.75/hr | Solana Compass, Render Foundation Medium report, grounded |
| Base chain | Solana (migrated from Ethereum/Polygon in prior iterations) | General sourcing; recheck if precise chain history matters |
| Maturity | Rendering side long-established (since 2017); AI subnet (Dispersed) is new as of Dec 2025 | Render Foundation's own announcement |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | SPOT CHECK | Proof-of-Render: watermarked preview, creator review and node spot-checks before escrow releases | Third-party explainers (Web3@KAIST), inferred, not confirmed against primary docs |
| Trust rests on | OPERATORS | Node operators, checked by the job creator and other nodes before payment | Third-party explainers, inferred |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | NONE FOUND | The only external reference is a billing patent, unrelated to verification | Whitepaper v4.0 check, grounded (as a negative finding) |
| Maturity | LIVE | Rendering since 2017; the AI compute subnet (Dispersed) launched Dec 2025 | Render Foundation announcement |
| On-chain | EXISTING CHAINS | Solana, after earlier Ethereum/Polygon iterations | General sourcing, inferred |

## Known limitations

- **The "AI" framing should be treated with caution.** Most of what's
  written about Render Network and AI is community/press coverage
  describing Dispersed as a new product layer, not evidence that
  Proof-of-Render or the core rendering protocol has any AI-specific
  cryptographic property. This entry exists in the repo as decentralized
  *compute supply* adjacent to the AI space, not as a verifiable-AI
  system comparable to EZKL, Gensyn, etc.
- Proof-of-Render's exact verification algorithm was not confirmed
  against Render's own primary technical documentation in this pass;
  third-party summaries agree on the shape (watermark + spot-check +
  escrow) but no single authoritative source was fetched cleanly.
- The whitepaper PDF extraction was unreliable (binary corruption);
  worth a manual recheck if precise claims from it matter later.
