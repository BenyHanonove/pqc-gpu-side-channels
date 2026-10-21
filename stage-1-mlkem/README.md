# Stage 1 · The Theory: From Secret Sharing to Kyber

**18 Oct – 7 Nov 2026** · [← back to overview](../README.md)

## What this stage shows

The theory behind the thesis, told through one thread: **polynomials as the foundation of
cryptography**. Shamir used polynomials to split a secret so that no single party learns it; Kyber
uses polynomial rings to build encryption that a quantum computer cannot break. Between the two sit
LWE, the hard problem behind Kyber, and the Fujisaki-Okamoto transform that makes Kyber CCA-secure,
ending with the standard itself, FIPS 203 (ML-KEM).

This stage is **reading and writing only**. No code, no implementation details, no side channels:
those belong to Stage 2 and Stage 3.

## Deliverables

| Deliverable | Description |
|---|---|
| Paper summaries | One page per paper, using the template below, in `summaries/` |
| Explainer | 3–4 pages: *Polynomials as the foundation of cryptography, from Shamir to Kyber* |
| Slides + video | 8 slides, 5–7 minute video |

## Reading list

Six papers. Most are read **in part**: the table says what to read, what to skip, and why. Only the
Kyber paper and FIPS 203 are read almost in full, because the whole thesis rests on them.

| # | Paper | Why it is here | Read | Skip | Time | Used again in |
|---|---|---|---|---|---|---|
| 1 ✅ <sub>read 18 Oct 17:11 · [summary](summaries/01-shamir-1979.md)</sub> | [How to Share a Secret](https://dl.acm.org/doi/10.1145/359168.359176), Shamir (CACM 1979) | How a polynomial of degree k−1 splits a secret into n shares: any k recover it, fewer reveal nothing | Everything (about two pages) | – | ~45 min (refresh) | **Stage 3**: masking, the main side-channel countermeasure, is secret sharing |
| 2 ✅ <sub>read over two days, finished 21 Oct 14:59 · [summary](summaries/02-tassa-dery-zamarin-2025.md)</sub> | [Secure Order Based Voting Using Distributed Tallying](https://arxiv.org/abs/2205.10580), Tassa, Dery & Zamarin (JISA 2025) | Secret sharing and multiparty computation in a real system; where my interest in this field began | Introduction, security model, protocol overview, security analysis | MPC protocol details, experiments | ~2 h | Motivation: videos and the direction memo |
| 3 | [On Lattices, Learning with Errors, Random Linear Codes, and Cryptography](https://cims.nyu.edu/~regev/papers/qcrypto.pdf), Regev (J. ACM 2009) | The hard problem Kyber is built on | Introduction, the definition of LWE, the LWE public-key scheme | The quantum reduction proof | ~2 h | – |
| 4 | [CRYSTALS-Kyber: a CCA-secure module-lattice-based KEM](https://eprint.iacr.org/2017/634) | The paper the thesis rests on | Almost everything: the CPA scheme, the CCA transform, parameters, security analysis | Implementation and performance sections | ~4 h | **Stage 2**: parameter choices shape the implementation |
| 5 | [A Modular Analysis of the Fujisaki-Okamoto Transformation](https://eprint.iacr.org/2017/604), Hofheinz, Hövelmanns & Kiltz | What makes Kyber CCA-secure | Introduction and the definitions of the transforms, especially implicit rejection | All proofs | ~1.5 h | **Stage 3**: the re-encryption comparison in Decaps is a known leakage point |
| 6 | [FIPS 203 (ML-KEM)](https://csrc.nist.gov/pubs/fips/203/final) | The official definition of what will be implemented and attacked | Overview, the K-PKE and ML-KEM algorithms, parameter sets, differences from Kyber (appendix) | The NTT section | ~3 h | **Stage 2**: the NTT section is read there |

**Total:** about 14 hours of reading.

### Order

| Week | Papers | The step in the story |
|---|---|---|
| 1 (18–24 Oct) | Shamir → Tassa → Regev | From a secret split by a polynomial to a hard problem built from noise |
| 2 (25–31 Oct) | Kyber → FO transform | From the hard problem to a secure KEM |
| 3 (1–7 Nov) | FIPS 203, then explainer, slides, video | From the paper to the standard |

### Deferred to later

Useful background, not required for this stage: NewHope (Kyber's predecessor), NIST IR 8413 (why
NIST chose Kyber), Peikert's survey, Langlois & Stehlé (Module-LWE), and Dilithium/FIPS 204 (the
ML-DSA extension).

## Summary template

Each summary in `summaries/` answers the same five questions:

1. **Problem:** what question does the paper answer?
2. **Key idea:** the one idea that makes it work.
3. **Result:** what is proven or achieved.
4. **Limitations:** what it does not cover.
5. **For this thesis:** why it matters here.

## Self-check questions

- [ ] Why does a polynomial of degree k−1 need exactly k points to be recovered, and why do k−1 points reveal nothing?
- [ ] Why is Shamir's scheme secure even against a quantum computer?
- [ ] In the voting protocol, what does each tallier see, and what stays hidden?
- [ ] What is LWE, and why does adding small noise turn an easy linear system into a hard one?
- [ ] How does a CPA-secure scheme differ from a CCA-secure one, and why does a KEM need CCA?
- [ ] What does the Fujisaki-Okamoto transform do, and what is implicit rejection?
- [ ] What are the ML-KEM-512/768/1024 parameter sets, and what does each parameter change?

## Tasks

| ID | Task | Due |
|---|---|---|
| S1.1 | Shamir and Tassa et al. + summaries | 21 Oct |
| S1.2 | Regev + summary | 24 Oct |
| S1.3 | Kyber and the FO transform + summaries | 31 Oct |
| S1.4 | FIPS 203 + summary | 3 Nov |
| S1.5 | Explainer (3–4 pages) | 5 Nov |
| S1.6 | Slides | 6 Nov |
| S1.7 | Video | 7 Nov |

Live status: [TASKS.md](../TASKS.md)

## Definition of done

- Every paper has a one-page summary in `summaries/`.
- I can answer every self-check question without notes.
- The explainer reads end to end without referring to code.
- The video is uploaded and is at most 7 minutes long.

## Results

*To be filled in as the stage progresses.*
