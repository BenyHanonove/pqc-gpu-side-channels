# Secure Order Based Voting Using Distributed Tallying: Tassa, Dery & Zamarin (JISA 2025)

[← back to Stage 1](../README.md) · [Paper](https://arxiv.org/abs/2205.10580) · Read over two days, finished 2026-10-21

**Problem.** Compute the winner of an election under an order-based rule (Copeland, Maximin) while
revealing **nothing but the result**. Anonymity is not enough: in a small election, a full tally
showing zero votes for Bob tells Bob that Alice did not vote for him.

**Key idea.** Replace one trusted tallier with *D* independent talliers. Each voter encodes their
ranking as an *M×M* pairwise-preference matrix and splits every entry with Shamir's scheme
(threshold *D′ = ⌊(D+1)/2⌋*). The talliers then run MPC on the shares: additions are free (shares are
linear), multiplications and comparisons need a short interactive sub-protocol. Secret sharing
replaces the much heavier homomorphic encryption.

**Result.**
- **Perfect ballot secrecy** under an honest majority: no ballot, no aggregate matrix, only the winners.
- **Ballot validation without opening it:** a theorem characterizes legal matrices, so cheating
  ballots (entries outside {±1}, cyclic preferences) are caught by checking polynomials on shares,
  e.g. *(x+1)(x−1) = 0*. The talliers also check that the shares really lie on a degree *D′−1*
  polynomial.
- **Practical:** about 50 ms per ballot (M=20, D=9, batched), over 1.7 million ballots a day; earlier
  work needed hours to days. Piloted in the Gentoo council elections.

**Limitations.** Talliers are assumed semi-honest (follow the protocol); malicious talliers are left
for future work. Privacy breaks if *D′* talliers collude. Voter-to-tallier channels rely on ordinary
public-key encryption and signatures.

**For this thesis.** Shamir moves from paper to a working system. Two links forward: (1) computing
on shares, where linear steps are cheap and non-linear ones are hard, is exactly the problem of
**masking** in Stage 3; (2) the information-theoretic core is quantum-safe, but the channels that
carry the shares rest on public-key cryptography, the part a quantum computer breaks and Kyber
replaces.
