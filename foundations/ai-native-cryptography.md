---
field: AI model ownership / monetization protocols
paper: "OML: Open, Monetizable, and Loyal AI"
authors: Zerui Cheng, Edoardo Contente, Ben Finch, Oleg Golev, Jonathan Hayase, Andrew Miller, Niusha Moshrefi, Anshul Nasery, Sandeep Nailwal, Sewoong Oh, Himanshu Tyagi, Pramod Viswanath
year: 2024
venue: IACR Cryptology ePrint Archive, 2024/1573
link: https://eprint.iacr.org/2024/1573
---

## Why this is the seed for this field

Directly authored by the tracked system's own team: 5 of the 12 authors
(Contente, Finch, Golev, Nailwal, Tyagi) are affiliated with Sentient
Foundation, alongside academics from Princeton, University of Washington,
and UIUC. This is not Sentient citing prior art; it is Sentient's founding
protocol paper. See `systems/sentient-oml.md` for how the system implements
it.

## Core contribution

Introduces **"AI-native cryptography"**: a proposed new branch of
cryptography built specifically around the structure of AI model
representations, rather than adapting traditional binary (secure/broken)
cryptographic security models to AI. The core observation is that AI data
representations live on continuous, low-dimensional manifolds, which can be
exploited to build security primitives traditional cryptography can't
express, e.g. a fingerprint that survives fine-tuning/distillation of a
model without being a literal bit-for-bit watermark. The paper's first
concrete primitive built in this framework is **model fingerprinting**
(labeled OML 1.0): an undetectable marker embedded into a model via
fine-tuning, usable to detect unauthorized use/redistribution of an
ostensibly "open" model.

## Link to tracked system

Sentient's entire ownership/monetization pitch depends on this primitive:
if a model is fully open-weight, nothing stops anyone from copying and
reselling it unless there's a way to detect that a given deployed model is
derived from a specific owned original. OML's fingerprinting is that
detection mechanism. The protocol layers described in
`systems/sentient-oml.md` (storage/distribution/access/incentive) are the
economic/infrastructure machinery built on top of this cryptographic
primitive, not the primitive itself.
