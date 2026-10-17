# Post-Quantum Cryptography on GPUs: Where Does It Leak?

**Side-channel leakage of ML-KEM (and ML-DSA) on GPU accelerators — MSc thesis groundwork**

Post-quantum key exchange is moving onto GPUs. NVIDIA ships ML-KEM and ML-DSA as a GPU library
(cuPQC), and it is being integrated into liboqs. Side-channel attacks on ML-KEM are well studied on
CPUs, microcontrollers and FPGAs, but I found no published side-channel evaluation of an ML-KEM or
ML-DSA implementation running on a GPU.

This repository is my groundwork toward that gap, in three separate steps: first the theory behind the
algorithms, then implementing their core (the NTT) on a GPU, and finally measuring whether, and how,
it leaks.

> **Simulation first.** Everything here runs in code and in a GPU simulator, with no lab equipment.
> Validation on real GPUs comes later.

---

## Research direction

> **Side-channel security of post-quantum cryptography (ML-KEM, ML-DSA) on GPUs**: a platform it is
> already being deployed on, but where, as far as I found, it has not been evaluated.

The direction is fixed; the exact question is not. These are the open questions I want to work
through with a thesis advisor:

- Which GPU-specific channels (warp divergence, memory behaviour, software power telemetry) matter
  for ML-KEM at all, given that its NTT accesses memory independently of the secret?
- Is software telemetry such as NVML enough for an unprivileged attacker, given how coarsely it samples?
- Do CPU-side countermeasures (constant-time code, masking) survive SIMT compilation and execution?
- How far can leakage be evaluated without lab equipment, and where does simulation stop being enough?

**Current working assumptions** (starting points, open to change):

| | |
|---|---|
| **Core** | ML-KEM (FIPS 203, formerly CRYSTALS-Kyber) |
| **Extension** | ML-DSA (FIPS 204, formerly CRYSTALS-Dilithium), which shares the same NTT structure |
| **Platform** | SIMT GPUs, with NVIDIA as the main case study |
| **Method** | Reference implementations → CUDA kernels in a simulator (GPGPU-Sim + AccelWattch) → statistical leakage tests (TVLA-style) |

## Why now

- NIST finalised **FIPS 203 (ML-KEM)** and **FIPS 204 (ML-DSA)** in August 2024.
- NSA's **CNSA 2.0** requires ML-KEM-1024 and ML-DSA-87 for national security systems, with
  category deadlines between 2030 and 2033.
- [**NIST IR 8547**](https://csrc.nist.gov/pubs/ir/8547/ipd) deprecates RSA and ECC from 2030 and disallows them from 2035.
- Servers will run millions of PQC handshakes in batches, which is exactly the workload GPUs accelerate.

*(Dates to be re-verified against the primary NIST and NSA sources.)*

## Roadmap

| Stage | Focus | Dates | Outcome |
|---|---|---|---|
| [**1 · The theory**](stage-1-mlkem/) | From Shamir's secret sharing to Kyber: LWE, the FO transform and FIPS 203 (reading only) | 18 Oct – 7 Nov 2026 | Six paper summaries + explainer + video |
| [**2 · The NTT on a GPU**](stage-2-ntt-gpu/) | Reference code walkthrough, then a batched CUDA NTT for ML-KEM and ML-DSA, and which GPU optimisations matter | 8 Nov – 28 Nov 2026 | Working kernel in simulation + comparison table + video |
| [**3 · Side channels on GPUs**](stage-3-gpu-side-channels/) | GPU-specific leakage sources, a first leakage experiment, and a direction memo for discussion | 29 Nov – 26 Dec 2026 | First result + one-page direction memo + video |

Every task has a due date and a recorded start and finish time: see **[TASKS.md](TASKS.md)**.

## Repository layout

```
.
├── README.md                     ← you are here
├── TASKS.md                      ← task board: due dates, start/finish times, duration
├── stage-1-mlkem/                ← Stage 1: the theory (reading only)
├── stage-2-ntt-gpu/              ← Stage 2: NTT on the GPU
├── stage-3-gpu-side-channels/    ← Stage 3: leakage experiments and direction memo
├── docs/
│   ├── decisions.md              ← project context: what was decided and why
│   └── related-work.md           ← literature map and the research gap
├── tasks/
│   └── tasks.csv                 ← source of truth for the task board
└── tools/
    └── task.py                   ← records task start/finish and regenerates TASKS.md
```

## How progress is tracked

Each task is started and closed from the command line. The tool stamps the time, computes the
duration, regenerates [TASKS.md](TASKS.md), and the change is committed, so the git history is an
independent record of when each piece of work was done.

```bash
python3 tools/task.py list                 # show all tasks and their status
python3 tools/task.py start S1.1           # stamp the start time
python3 tools/task.py done  S1.1 --hours 9 # stamp the finish time, log hands-on hours
git commit -am "S1.1 done: theory notes"   # the commit is the proof
```

Signing commits (SSH or GPG key) makes GitHub mark each one as *Verified*, which ties every
timestamp to my key.

## Decisions and context

What was decided in planning, and why: [docs/decisions.md](docs/decisions.md).

## Related work

The literature map, with the gap this project targets, is in
[docs/related-work.md](docs/related-work.md).

## Responsible disclosure

Any leakage found in a commercial product will be reported to the vendor before publication.

---

*Author: Benjamin ([@BenyHanonove](https://github.com/BenyHanonove)) — MSc in Computer Science*
