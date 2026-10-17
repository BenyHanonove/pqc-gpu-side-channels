# Stage 1 · Dissecting ML-KEM

**18 Oct – 7 Nov 2026** · [← back to overview](../README.md)

## What this stage shows

ML-KEM explained end to end, from the math to the code, with **every secret value located**:
where the secret key, the message and the shared key are created, transformed and compared.
You cannot look for leakage until you know where the secrets are, so this map is the foundation for
Stage 3.

## Deliverables

| Deliverable | Description |
|---|---|
| Theory notes | 2–3 pages, written as answers to the self-check questions below |
| Function map | Table: function → FIPS 203 step → touches a secret? |
| Tracing script | Builds the reference code and prints secret intermediates during decapsulation |
| Data-flow diagram | KeyGen / Encaps / Decaps with secrets in red and known leakage points marked |
| ML-DSA comparison | What ML-DSA shares with ML-KEM and what differs |
| Slides + video | 8 slides, 5–7 minute video |

## Reading list

| # | Source | Focus on | Time |
|---|---|---|---|
| 1 | [A Decade of Lattice Cryptography](https://eprint.iacr.org/2015/939), Peikert | Definitions of LWE and Ring-LWE, and why they are hard | ~3 h |
| 2 | [CRYSTALS-Kyber: a CCA-secure module-lattice-based KEM](https://eprint.iacr.org/2017/634) | Module-LWE, the CPA scheme, the move to CCA | ~4 h |
| 3 | [FIPS 203 (ML-KEM)](https://csrc.nist.gov/pubs/fips/203/final) | K-PKE and ML-KEM algorithms, parameter sets, Compress/Decompress, the NTT section | ~5 h |
| 4 | [A Modular Analysis of the Fujisaki-Okamoto Transformation](https://eprint.iacr.org/2017/604) | Introduction only: re-encryption and implicit rejection | ~2 h |
| 5 | [FIPS 204 (ML-DSA)](https://csrc.nist.gov/pubs/fips/204/final) + [Dilithium paper](https://eprint.iacr.org/2017/633) | Overview only, for the comparison | ~2 h |

## Self-check questions

- [ ] What is Module-LWE, and why does adding small noise turn an easy linear system into a hard one?
- [ ] How does K-PKE (CPA-secure) differ from ML-KEM (CCA-secure)?
- [ ] Why does Decaps re-encrypt and compare, and what happens when the comparison fails (implicit rejection)?
- [ ] What do Compress and Decompress do, and why do they lose information on purpose?
- [ ] What are k, η1, η2, du, dv for ML-KEM-512/768/1024, and what does each one change?
- [ ] Why q = 3329, and why is Kyber's NTT incomplete (7 layers plus `basemul`)?

## Code walkthrough

Built on the [pq-crystals reference implementation](https://github.com/pq-crystals/kyber) because it is
the easiest to read. It is no longer maintained, so [mlkem-native](https://github.com/pq-code-package/mlkem-native)
is the production-quality comparison point.

```bash
git clone https://github.com/pq-crystals/kyber && cd kyber/ref
make test/test_kyber768 test/test_vectors768
./test/test_kyber768
```

Functions to map:

| File | Functions | Notes |
|---|---|---|
| `kem.c` | `crypto_kem_keypair`, `crypto_kem_enc`, `crypto_kem_dec` | Top-level KEM |
| `indcpa.c` | `indcpa_keypair_derand`, `indcpa_enc`, `indcpa_dec`, `gen_matrix` | K-PKE |
| `poly.c` | `poly_compress`, `poly_decompress`, `poly_frommsg`, `poly_tomsg`, `poly_getnoise_eta1`, `poly_basemul_montgomery` | Message encoding: a known leakage point |
| `ntt.c` | `ntt`, `invntt`, `basemul`, `fqmul` | Core of Stage 2 |
| `reduce.c` | `montgomery_reduce`, `barrett_reduce` | Modular reduction |
| `verify.c` | `verify`, `cmov` | Constant-time comparison in the FO transform |

Known leakage points from the literature to mark on the map: the division in `poly_tomsg`
(KyberSlash), `poly_frommsg`, the NTT and `basemul`, and the comparison in `verify`.

## Tasks

| ID | Task | Due |
|---|---|---|
| S1.1 | Read sources 1–4 and write the theory notes | 24 Oct |
| S1.2 | Verify CNSA 2.0 and NIST IR 8547 timelines at the primary source | 26 Oct |
| S1.3 | Build the reference code, function map, tracing script | 29 Oct |
| S1.4 | Data-flow diagram with secrets and leakage points | 1 Nov |
| S1.5 | ML-DSA comparison | 2 Nov |
| S1.6 | Slides | 4 Nov |
| S1.7 | Video | 7 Nov |

Live status: [TASKS.md](../TASKS.md)

## Definition of done

- I can explain, without slides, the path of the secret key from KeyGen to the end of Decaps.
- This folder builds from scratch with one command and the tests pass.
- Every function in the map is marked as touching a secret or not.
- The video is uploaded and is at most 7 minutes long.

## Results

*To be filled in as the stage progresses.*
