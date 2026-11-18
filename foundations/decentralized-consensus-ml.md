---
field: Decentralized incentive / consensus mechanisms for ML networks
paper: "A Value for n-Person Games"
authors: Lloyd S. Shapley
year: 1952
venue: "Contributions to the Theory of Games, Vol. II (Annals of Mathematics Studies 28), Princeton University Press"
---

# A Value for n-Person Games (Shapley, 1952)

## Why this is the seed for this field

Bittensor's own academic paper, *"Incentivizing Intelligence: The Bittensor
Approach"* (Steeves et al., hosted at
[bittensor.com/academia](https://www.bittensor.com/academia)), explicitly
cites Shapley's 1952 value concept as the theoretical basis for how the
network attributes credit to peers: each participant's reward is meant to
reflect their marginal contribution to the network's informational output,
evaluated across permutations of coalitions of peers, which is exactly what
the Shapley value formalizes. This is the one piece of the Bittensor stack
with a clean, explicit academic citation; the rest of the mechanism (the
specific stake-weighted-median "Yuma Consensus" algorithm used in
production) is documented in community/ecosystem sources rather than in a
peer-reviewed citation trail; see the naming-gap note in
[[systems/bittensor]].

## Core contribution

Shapley's paper answers a fairness question: if a group of people (or
agents) jointly produce some value by cooperating, how should that value be
split among them in a way that fairly reflects each one's contribution,
even when contributions aren't independent (i.e. what one agent adds
depends on who else is already in the group)? The Shapley value is defined
by averaging each agent's marginal contribution across every possible order
in which agents could join the coalition. It's the unique allocation rule
satisfying a small set of fairness axioms (efficiency, symmetry, additivity,
null-player). It's a cooperative-game-theory result, not a machine-learning
or cryptography result; its relevance here is purely as the attribution
principle a decentralized ML incentive system borrows.

## Link to tracked system

Bittensor needs to answer: "how much is this peer's model output actually
worth to the network, given that other peers are also contributing similar
or overlapping information?" Naively rewarding every peer independently
invites redundant, low-effort submissions; the Shapley framing is invoked to
justify rewarding marginal/novel contribution instead. In practice,
production Bittensor does not compute literal Shapley values (that's
computationally intractable at network scale: exact Shapley values require
summing over all coalition permutations); it uses Yuma Consensus's
stake-weighted-median-with-clipping as a tractable proxy mechanism inspired
by the same fairness goal. This is worth flagging explicitly: the citation
grounds the *motivation*, not the *literal algorithm* actually running in
production.
