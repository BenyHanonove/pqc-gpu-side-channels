# Stage 1 · The Theory Behind ML-KEM and the CRYSTALS Family

**18 Oct – 7 Nov 2026** · [← back to overview](../README.md)

## What this stage shows

A solid theoretical understanding of Kyber and the CRYSTALS family: why lattice problems are believed
to be hard, how Module-LWE turns that hardness into encryption, how Kyber grew out of NewHope, how
its sibling Dilithium is built, and how both became the NIST standards ML-KEM and ML-DSA.

This stage is **reading and writing only**. No code, no implementation details, no side channels:
those belong to Stage 2 and Stage 3.

## Deliverables

| Deliverable | Description |
|---|---|
| Paper summaries | One page per source, using the template below, in `summaries/` |
| Explainer | 3–4 pages: *How ML-KEM and ML-DSA work, from lattices to the standard* |
| Slides + video | 8 slides, 5–7 minute video |

## Reading list

### Week 1 (18–24 Oct): Lattice foundations

| # | Source | Focus on | Time |
|---|---|---|---|
| 1 | [On Lattices, Learning with Errors, Random Linear Codes, and Cryptography](https://cims.nyu.edu/~regev/papers/qcrypto.pdf), Regev (J. ACM 2009) | The definition of LWE, the intuition behind the hardness reduction, the LWE public-key scheme | ~5 h |
| 2 | [A Decade of Lattice Cryptography](https://eprint.iacr.org/2015/939), Peikert | The big picture: SIS, LWE, Ring-LWE, and how they relate | ~4 h |
| 3 | [Worst-case to Average-case Reductions for Module Lattices](https://eprint.iacr.org/2012/090), Langlois & Stehlé | What Module-SIS and Module-LWE are, and why they sit between LWE and Ring-LWE | ~3 h |

### Week 2 (25–31 Oct): From NewHope to Kyber, and the CRYSTALS family

| # | Source | Focus on | Time |
|---|---|---|---|
| 4 | [Post-Quantum Key Exchange – A New Hope](https://eprint.iacr.org/2015/1092), Alkim, Ducas, Pöppelmann & Schwabe (USENIX Security 2016) | Kyber's Ring-LWE predecessor: what it got right, and what Kyber changed by moving to Module-LWE | ~3 h |
| 5 | [CRYSTALS-Kyber: a CCA-secure module-lattice-based KEM](https://eprint.iacr.org/2017/634) | The CPA-secure scheme, parameter choices, the move to CCA security | ~4 h |
| 6 | [A Modular Analysis of the Fujisaki-Okamoto Transformation](https://eprint.iacr.org/2017/604), Hofheinz, Hövelmanns & Kiltz | How a CPA scheme becomes CCA-secure; implicit rejection | ~3 h |
| 7 | [CRYSTALS-Dilithium: A Lattice-Based Digital Signature Scheme](https://eprint.iacr.org/2017/633) | Fiat-Shamir with aborts, and why signatures rely on Module-SIS as well | ~4 h |

### Week 3 (1–3 Nov): From CRYSTALS to the NIST standards

| # | Source | Focus on | Time |
|---|---|---|---|
| 8 | [FIPS 203 (ML-KEM)](https://csrc.nist.gov/pubs/fips/203/final) | The standardised algorithms and parameter sets; what changed from Kyber | ~4 h |
| 9 | [FIPS 204 (ML-DSA)](https://csrc.nist.gov/pubs/fips/204/final) | Overview and parameter sets; what changed from Dilithium | ~2 h |
| 10 | [NIST IR 8413-upd1](https://nvlpubs.nist.gov/nistpubs/ir/2022/NIST.IR.8413-upd1.pdf) | Why NIST selected Kyber and Dilithium over the other third-round candidates | ~2 h |

## Summary template

Each summary in `summaries/` answers the same five questions:

1. **Problem:** what question does the paper answer?
2. **Key idea:** the one idea that makes it work.
3. **Result:** what is proven or achieved.
4. **Limitations:** what it does not cover.
5. **For this thesis:** why it matters here.

## Self-check questions

- [ ] What is LWE, and why does adding small noise turn an easy linear system into a hard one?
- [ ] What do Ring-LWE and Module-LWE add, and what does each trade off?
- [ ] Why did Kyber move from NewHope's Ring-LWE to Module-LWE?
- [ ] How does a CPA-secure scheme differ from a CCA-secure one, and why does a KEM need CCA?
- [ ] What does the Fujisaki-Okamoto transform do, and what is implicit rejection?
- [ ] Why does Dilithium need "aborts" when signing?
- [ ] What are the ML-KEM-512/768/1024 parameter sets, and what does each parameter change?
- [ ] Why did NIST choose Kyber and Dilithium?

## Tasks

| ID | Task | Due |
|---|---|---|
| S1.1 | Lattice foundations: sources 1–3 + summaries | 24 Oct |
| S1.2 | NewHope, Kyber and the FO transform: sources 4–6 + summaries | 28 Oct |
| S1.3 | Dilithium: source 7 + summary | 31 Oct |
| S1.4 | Standards and NIST selection: sources 8–10 + summaries | 3 Nov |
| S1.5 | Explainer (3–4 pages) | 5 Nov |
| S1.6 | Slides | 6 Nov |
| S1.7 | Video | 7 Nov |

Live status: [TASKS.md](../TASKS.md)

## Definition of done

- Every source has a one-page summary in `summaries/`.
- I can answer every self-check question without notes.
- The explainer reads end to end without referring to code.
- The video is uploaded and is at most 7 minutes long.

## Results

*To be filled in as the stage progresses.*
