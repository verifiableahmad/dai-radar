---
field: Confidential computing (TEE) for AI inference
paper: "CCF: A Framework for Building Confidential Verifiable Replicated Services"
authors: Mark Russinovich, Edward Ashton, Christine Avanessians, Miguel Castro, et al.
year: 2019
venue: Microsoft Research Technical Report MSR-TR-2019-16
refined_paper: "Confidential Consortium Framework: Secure Multiparty Applications with Confidentiality, Integrity, and High Availability"
refined_venue: Proceedings of the VLDB Endowment, Vol 17(2), 2023 (VLDB nominee)
refined_link: https://arxiv.org/abs/2310.11559
---

# CCF: A Framework for Building Confidential Verifiable Replicated Services

## Why this is the seed for this field

Grounded directly, not inferred: Microsoft's own Azure documentation
states that Azure confidential ledger, the transparency/auditability
component used in Azure AI Confidential Inferencing, "is built on the
Confidential Consortium Framework (CCF)." CCF is itself a Microsoft
Research project (github.com/microsoft/CCF) with its own research page
listing this 2019 technical report as the original design paper, later
refined and published in the peer-reviewed 2023 VLDB paper linked above.
See `/systems/azure-confidential-ai.md` for how this plugs into the
actual Azure product.

## Core contribution

CCF combines two previously separate ideas into one system: (1)
hardware TEEs for remotely-verifiable confidentiality and code
integrity of the compute itself, and (2) state-machine replication
backed by an append-only, auditable ledger for data integrity and high
availability. The result is a framework for building multi-party
applications that can run on cloud infrastructure none of the
participants fully trust, with governance (who can change the rules,
who operates nodes) kept separate from, and verifiable independent of,
the cloud operator. The 2023 refinement formalizes CCF's custom
distributed-consensus and client-consistency model and was itself the
subject of a separate 2025 NSDI verification paper ("Smart Casual
Verification of the Confidential Consortium Framework"), which is
outside this seed paper's scope but worth noting as follow-on
scrutiny of the same system.

## Link to tracked system

Azure's confidential ledger product (and, through it, the
transparency-ledger component of Azure AI Confidential Inferencing)
is built directly on CCF. This is the one part of Microsoft's
"Confidential AI" story with a real, citable Microsoft Research paper
behind it; the TEE hardware layer underneath (AMD SEV-SNP, Intel TDX,
NVIDIA H100 confidential computing) is vendor technology with its own
specs, not an academic contribution from this lineage; see
`/systems/azure-confidential-ai.md`'s "Built on" section for why those
two lineages are kept separate rather than merged into one claim.
