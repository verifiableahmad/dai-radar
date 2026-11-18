---
field: Secure MPC for ML
paper: Evaluation of Arithmetic Sum-of-Products Expressions in Linear Secret Sharing Schemes with a Non-Interactive Computation Phase
authors: Miguel de Vega, Andrei Lapets, Stanisław Jarecki, Wicher Malten, Mehmet Ugurbil, Wyatt Howe
year: 2023
venue: self-published / IACR ePrint 2023/1740 (withdrawn 2024-06-28; mirrored at nillion.pub)
---

# Evaluation of Arithmetic Sum-of-Products Expressions in Linear Secret Sharing Schemes with a Non-Interactive Computation Phase

## Why this is the seed for this field

Nillion's own team announced this paper as the direct basis for their
non-interactive MPC protocol for non-linear computation; see
"Introducing the Nillion Non-Interactive MPC Protocol for Non-Linear
Computations"
(https://medium.com/@Nillion_Network/introducing-the-nillion-non-interactive-mpc-protocol-for-non-linear-computations-67e9310ef479),
corroborated by the MPC Alliance's write-up of the same announcement
(https://www.mpcalliance.org/blog/nillion-introduces-a-non-interactive-mpc-protocol-for-non-linear-computations).
Four of the six listed authors (de Vega, Malten, Ugurbil, and one more)
are Nillion staff; this is effectively the project publishing its own
core protocol paper, which is about the strongest grounding available
for a "foundational paper" claim in this exercise.

## Core contribution

Classic linear secret sharing schemes (LSSS) handle addition of shared
secrets for free but need rounds of communication between parties to
handle multiplication, the operation needed for anything non-linear.
This paper describes a way to evaluate arithmetic sum-of-products
expressions (sums of products of shared values, which covers the multiply
step) using a **preprocessing phase that doesn't depend on the private
inputs**, so the actual computation-on-private-data step becomes
non-interactive. In plain terms: move the expensive "parties have to
talk to each other" part of secure multiplication to a setup phase that
can happen before anyone's real data is involved, so the live computation
is faster and doesn't require synchronous coordination between nodes.

## Link to tracked system

This is the specific technique underneath Nillion's **Curl** protocol,
which extends LSSS to support the non-linear math (division, square
roots, trig, logarithms) that ML workloads need but plain secret sharing
can't do cheaply. It's the piece that lets Nillion's MPC layer process
more than just linear operations on encrypted data, which is the gap
between "toy secret sharing demo" and "can actually run ML inference
on shared data."

## Open question for future monitoring

The paper was withdrawn from IACR ePrint (2023/1740, withdrawn
2024-06-28) and now only exists as a Nillion-hosted PDF. No public
explanation found for the withdrawal. If a future paper-watch run
surfaces commentary on *why* it was withdrawn (correction vs. priority
dispute vs. something substantive), that's worth a dedicated note since
it bears directly on how much to trust this as the field's seed paper.
