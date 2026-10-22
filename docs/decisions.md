# Project Context and Decisions

[← back to overview](../README.md) · *Last updated: 17 Oct 2026*

This file records what was decided in planning and **why**, so work can resume from here without
re-deriving it. Read it first when starting a new working session.

---

## 1. The research direction

> **Side-channel security of post-quantum cryptography (ML-KEM, ML-DSA) on GPUs**: a platform it is
> already being deployed on, but where, as far as I found, it has not been evaluated.

**Decision: a sharp direction, not a fixed question.** The first contact with potential advisors is
an email and a conversation: what I know, what I have read, what interests me, and what interests
them. A fixed question would leave no room for that, so the direction and the gap stay fixed, and
the exact question is narrowed together with an advisor. Open questions to bring to that
conversation:

- Which GPU-specific channels (warp divergence, memory behaviour, software power telemetry) matter
  for ML-KEM at all, given that its NTT accesses memory independently of the secret?
- Is software telemetry such as NVML enough for an unprivileged attacker, given how coarsely it samples?
- Do CPU-side countermeasures (constant-time code, masking) survive SIMT compilation and execution?
- How far can leakage be evaluated without lab equipment, and where does simulation stop being enough?

The scope choices below (algorithm, platform, method, attacker model) are **current working
assumptions**: they make the three stages concrete, and any of them can change after that conversation.

**Placeholder thesis title** (for orientation only, not used in the email): *Side-Channel Leakage of
ML-KEM on SIMT Accelerators*

## 2. Scope decisions

| Decision | Choice | Why |
|---|---|---|
| Algorithm (core) | **ML-KEM** (FIPS 203, formerly CRYSTALS-Kyber) | The only finalised NIST key-establishment standard; required by NSA CNSA 2.0 (ML-KEM-1024); already deployed in TLS |
| Algorithm (extension) | **ML-DSA** (FIPS 204, formerly CRYSTALS-Dilithium) | Same CRYSTALS family, same ring and NTT structure, also in CNSA 2.0 and cuPQC. Its leakage sources differ (rejection sampling, the nonce *y*), so it gets its own analysis |
| Out of scope | HQC, FN-DSA (Falcon), SLH-DSA (SPHINCS+) | Not Kyber-based: HQC is code-based; Falcon uses floating-point FFT (a possible PhD topic); SLH-DSA is hash-based |
| Platform | **GPUs (SIMT accelerators)**, NVIDIA as the main case study | Clear literature gap (section 3); strong industry relevance as PQC moves to batched server workloads |
| Cross-vendor | Frame the model for SIMT in general (warp ↔ wavefront ↔ sub-group; NVML ↔ AMD SMI ↔ Level Zero) | More general result; one deep case study is worth more than several shallow ones. Full cross-vendor comparison is PhD scope |
| Method | **Simulation first, no lab hardware** | No access to oscilloscopes or ChipWhisperer. Real-GPU measurement (cloud) comes later |
| Fallback direction | Leakage-aware NTT optimisation on Cortex-M, in simulation (Rainbow/ELMO, SCALib) | Lower risk, always yields a publishable result. Not the main plan |

**Policy context (re-verify at primary sources before citing):** FIPS 203/204/205 final on
13 Aug 2024. CNSA 2.0 requires ML-KEM-1024 and ML-DSA-87, with category deadlines between 2030 and
2033. NIST IR 8547 deprecates RSA/ECC from 2030 and disallows them from 2035 (its draft/final status
is reported inconsistently).

## 3. What the literature search found

Full table: [related-work.md](related-work.md). Initial search, October 2026, about 15 targeted
queries; details from abstracts and excerpts.

- **The gap:** no published side-channel evaluation of an ML-KEM or ML-DSA implementation on a GPU.
- **cuPQC** (NVIDIA) ships ML-KEM/ML-DSA for GPUs, claims side-channel protections, and is being
  integrated into liboqs. No independent evaluation found. This is the main real-world target.
- **Owl** (DSN 2024) detects leakage in CUDA code but was not applied to PQC.
- **Energon** (2025) extracted information through nvidia-smi without root, but about AI models, not keys.
- **NVML limits:** on A100/H100 only about 25% of runtime is sampled, with ~100 ms updates. This is
  what makes NVML key recovery a research question rather than an engineering task.
- **Existing GPU timing attacks** target table-based AES (coalescing, shared-memory bank conflicts).
  The ML-KEM NTT has fixed, secret-independent memory access, so those attacks likely do not transfer;
  leakage must come from elsewhere (compress/decompress, secret divisions à la KyberSlash, the FO
  comparison, power).

## 4. Known limitations to state explicitly

- **AccelWattch** estimates power from activity counts (instructions, memory accesses), not data
  values. It can show control-flow and memory leakage, not Hamming-weight leakage. This is exactly why
  real NVML measurements are needed.
- **cuPQC** is distributed as a binary: check the licence before analysing it; otherwise black-box only.
- **Responsible disclosure:** any leakage in a commercial product is reported to the vendor before publication.

## 5. How the work is structured

Three stages with a **strict separation**: each stage has its own reading list and nothing overlaps.

| Stage | Content | Dates |
|---|---|---|
| [1 · The theory](../stage-1-mlkem/) | Reading and writing only: no code, no NTT, no side channels | 18 Oct – 7 Nov 2026 |
| [2 · The NTT on a GPU](../stage-2-ntt-gpu/) | All implementation: reference code walkthrough, NTT theory, C and CUDA NTT, Accel-Sim, optimisation variants | 8 – 28 Nov 2026 |
| [3 · Side channels on GPUs](../stage-3-gpu-side-channels/) | Literature on side channels, kernel analysis, TVLA-style experiment, one-page direction memo | 29 Nov – 26 Dec 2026 |

Each stage ends with slides and a 5–8 minute video. The three videos, this repository and the
one-page direction memo form the package sent to potential thesis advisors.

### Stage 1 reading choices

Six papers, chosen for a single narrative, *polynomials as the foundation of cryptography*:
Shamir 1979 → Tassa, Dery & Zamarin → Regev (LWE) → Kyber → FO transform → FIPS 203.

- Shamir and Tassa et al. are included deliberately: they are where my interest in the field
  started, and secret sharing reappears in Stage 3 as **masking**, the main side-channel countermeasure.
- Most papers are read **in part**. Only the Kyber paper and FIPS 203 are read almost in full,
  because the thesis rests on them. The README lists what to read and skip for each.
- **Deferred:** NewHope, NIST IR 8413, Peikert's survey, Langlois & Stehlé, Dilithium/FIPS 204.

## 6. Working conventions

- Repository content is in **English**; planning discussions are in Hebrew.
- Every task is opened and closed with `tools/task.py`, then committed, so git records when work was done.
- All work is committed directly to `main` and pushed; no feature branches (single-author project).
- Signing commits (SSH or GPG) is recommended so GitHub marks them *Verified*.

## 7. Related groundwork

[cpp-under-the-hood](https://github.com/BenyHanonove/cpp-under-the-hood): low-level C++ missions on
what the compiler does with code. One finding there is directly relevant: a "branchless"
constant-time equality check compiled with a conditional branch at `gcc -O0`, branchless at `-O2`,
and was rewritten by `clang -O2` into a plain compare. **Constant-time properties are not preserved
by the language; they depend on the compiler and flags**, the same root cause as KyberSlash.

## 8. Open items

- [x] Start S1.1 (Shamir and Tassa et al.), due 21 Oct: Shamir read 2026-10-18 17:11, Tassa et al. read over two days, finished 2026-10-21 14:59
- [x] S1.2 (Regev), due 24 Oct: read 2026-10-21 15:08 to 2026-10-22 17:03 (about 26 h, 4 hands-on hours)
- [ ] Re-verify CNSA 2.0 and NIST IR 8547 dates at the primary sources
- [ ] Read the hardening section of the cuPQC documentation
- [ ] Forward citations of Owl, Rendered Insecure and Part-time Power Measurements
- [ ] Check whether Owl's code is public
- [ ] Set up commit signing
