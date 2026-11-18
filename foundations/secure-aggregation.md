---
field: Federated learning / confidential aggregation
paper: "Practical Secure Aggregation for Privacy-Preserving Machine Learning"
authors: Keith Bonawitz, Vladimir Ivanov, Ben Kreuter, Antonio Marcedone, H. Brendan McMahan, Sarvar Patel, Daniel Ramage, Aaron Segal, Karn Seth
year: 2017
venue: ACM CCS 2017
link: http://eprint.iacr.org/2017/281
---

# Practical Secure Aggregation for Privacy-Preserving Machine Learning

## Why this is the seed for this field

Grounded via a direct citation, not inference: Google's own 2017 federated
learning announcement
([research.google/blog](https://research.google/blog/federated-learning-collaborative-machine-learning-without-centralized-training-data/))
links to this exact paper when describing "a Secure Aggregation protocol
that uses cryptographic techniques so that a coordinating server need
only combine encrypted data from hundreds or thousands of users." It's the
specific crypto contribution underneath Google's federated learning
deployments, distinct from the plain ML algorithm (Federated Averaging,
see `/systems/google-confidential-federated-learning.md`).

## Core contribution

A protocol letting a server compute the *sum* of values held by many
clients (e.g. their model-update vectors) without learning any individual
client's value, only the aggregate. It's built from secret-sharing and
pairwise-masking: each client masks its value with random numbers shared
pairwise with other clients such that the masks cancel out exactly when
all values are summed, revealing the sum but nothing about any individual
contribution. The protocol is also fault-tolerant to clients dropping out
mid-protocol (a real concern on mobile devices), which earlier secure-sum
constructions generally weren't designed to handle well at this scale.

## Link to tracked system

This is the cryptographic mechanism that lets Google's federated learning
systems claim the server never sees an individual device's model update,
only the aggregate across many devices. The later TEE-based "Confidential
Federated Computations" work (2024) layers additional server-side
confidentiality on top of this, authored by overlapping people (Bonawitz
and McMahan both appear on both papers), but Secure Aggregation is the
earlier, foundational crypto piece specifically for the aggregation step
itself. See `/systems/google-confidential-federated-learning.md` for how
the two layers relate.
