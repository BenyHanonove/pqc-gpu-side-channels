# Stage 3 · Side Channels on GPUs

**29 Nov – 26 Dec 2026** · [← back to overview](../README.md)

## What this stage shows

That ML-KEM on a GPU has **leakage sources a CPU does not have**: warp divergence, memory-access
behaviour, and software-readable power telemetry (NVML). The stage ends with one measured result from
simulation and a one-page research proposal built on it.

## Deliverables

| Deliverable | Description |
|---|---|
| Literature table | 8 papers, fixed columns (see below) |
| Kernel analysis | Every branch, division or memory address that depends on a secret |
| First experiment | TVLA-style t-test on simulated cycles and power, with one graph |
| Research proposal | One page, in English |
| Slides + video | 8 slides, 6–8 minute video |

## Literature, in priority order

Links in [docs/related-work.md](../docs/related-work.md).

1. **Owl** (DSN 2024): leakage detection for CUDA. The tool to use or emulate. Read in full.
2. **Energon** (2025): reading power through nvidia-smi without root, and what it revealed.
3. **Part-time Power Measurements** (SC'24): the limits of NVML. Shapes the attacker model.
4. **PLATYPUS** (2021): how a slow software power channel became key extraction.
5. **KyberSlash** (2024): a leakage source that may also exist in GPU kernels.
6. **More Practical Single-Trace Attacks on the NTT** (2019): attacking the NTT through power.
7. **Template and CPA on Pair-Pointwise Multiplication** (2025): a recent attack on a batched step.
8. **Rendered Insecure** (CCS 2018): the attacker model on a shared GPU.

Revisit from Stage 1: **Shamir 1979** (masking is secret sharing applied to every secret value) and
the **FO transform** (the re-encryption comparison in Decaps).

Columns for each paper: target · channel · attacker model · traces/measurements needed ·
countermeasure · does it transfer to a GPU?

## Experiment design

**Kernel analysis**

1. Extend the Stage 2 kernel to the secret-dependent part of decapsulation: NTT of *u*, `basemul`
   with the secret *s*, inverse NTT, `poly_tomsg`.
2. Inspect PTX and SASS (`nvcc -ptx`, `cuobjdump -sass`) and mark every secret-dependent branch,
   division or memory address.
3. If Owl's code is available, run it on the kernel.
4. For cuPQC, check the licence before analysing the binary. If in doubt, black-box testing only.

**TVLA-style test**

1. Two input groups: a fixed secret key and random secret keys, about 100 runs each (the simulator is slow).
2. Record simulated cycles and AccelWattch power for every run.
3. Welch's t-test between the groups; |t| > 4.5 indicates leakage.
4. Repeat on a deliberately leaky variant (a KyberSlash-style secret division) to show the method
   detects leakage when it is there.

> **Known limitation.** AccelWattch estimates power from activity counts (instructions, memory
> accesses), not from data values. It can reveal control-flow and memory leakage, but not
> Hamming-weight leakage. This is stated explicitly in the results, and it is exactly why the thesis
> needs real measurements through NVML.

## Research proposal outline

1. **Problem:** PQC is moving to GPUs (cuPQC, liboqs) and its leakage there is unevaluated.
2. **Attacker model:** a co-located tenant reading NVML without root and submitting ciphertexts for batched decapsulation.
3. **Hypothesis and research question.**
4. **Method:** simulation → measurement on a cloud GPU → countermeasures.
5. **Preliminary result:** the graph from this stage.
6. **Extensions:** ML-DSA, other GPU vendors.
7. **Responsible disclosure.**

## Tasks

| ID | Task | Due |
|---|---|---|
| S3.1 | Literature table (8 papers) | 7 Dec |
| S3.2 | Kernel analysis (PTX/SASS, Owl) | 11 Dec |
| S3.3 | TVLA-style experiment in simulation | 16 Dec |
| S3.4 | One-page research proposal | 19 Dec |
| S3.5 | Slides | 22 Dec |
| S3.6 | Video | 26 Dec |

Live status: [TASKS.md](../TASKS.md)

## Definition of done

- One graph shows in numbers whether the kernel leaks, and the deliberately leaky variant is detected.
- The proposal fits on one page and states the limits of the simulation.
- All code and experiments in this folder re-run from the README.
- All three videos are uploaded.

## Results

*To be filled in as the stage progresses.*
