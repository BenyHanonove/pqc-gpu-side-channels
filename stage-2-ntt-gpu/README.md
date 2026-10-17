# Stage 2 · The NTT on a GPU

**8 Nov – 28 Nov 2026** · [← back to overview](../README.md)

## What this stage shows

Why lattice cryptography uses the Number Theoretic Transform (NTT) instead of the FFT, and how to run
it efficiently on a GPU: a **batched CUDA NTT** for both ML-KEM (q = 3329) and ML-DSA (q = 8380417),
run in a GPU simulator with **no physical GPU**, plus a measured comparison of GPU-specific
optimisations. This kernel is the target of the leakage experiments in Stage 3.

## Deliverables

| Deliverable | Description |
|---|---|
| Simulation environment | Accel-Sim (GPGPU-Sim + AccelWattch) running, with install notes in this folder |
| Reference NTT in C | Verified against naive polynomial multiplication |
| Batched CUDA NTT | Same output as the C version, for both values of q |
| Comparison table | 4–6 variants: simulated cycles and simulated power |
| Slides + video | 7 slides, 5–7 minute video |

## Reading list

| # | Source | Focus on | Time |
|---|---|---|---|
| 1 | [A Complete Beginner Guide to the NTT](https://eprint.iacr.org/2024/585) | Negacyclic convolution, NTT/INTT, the toy examples (work them by hand) | ~4 h |
| 2 | [Note on NTTs for Implementers](https://eprint.iacr.org/2026/1546), Bo-Yin Yang | Butterflies, twisting, incomplete NTTs: why Kyber has 7 layers | ~3 h |
| 3 | [A note on the implementation of the NTT](https://eprint.iacr.org/2017/727.pdf), Michael Scott | Lazy reduction, and the warning about NTT leakage | ~1 h |
| 4 | [HI-Kyber](https://eprint.iacr.org/2023/1194) | How the NTT is split across GPU threads and blocks | ~2 h |
| 5 | [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/) | Programming model, warps, shared memory, `__shfl_sync` | ~3 h |

## Environment

[Accel-Sim](https://github.com/accel-sim/accel-sim-framework) bundles GPGPU-Sim and the
[AccelWattch](https://engineering.purdue.edu/tgrogers/publication/kandiah-micro-2021) power model.

1. Install a CUDA Toolkit version supported by Accel-Sim (check its README). Compiling does not need a GPU.
2. `source gpu-simulator/setup_environment.sh`, then build following the README.
3. Run one of the bundled examples and locate the cycle count and power estimate in the output.
4. Record every step and every problem in `stage-2-ntt-gpu/SETUP.md`.

> **Fallback:** if the simulator setup is blocked for more than three days, continue with the C
> version and run the CUDA kernel for a few hours on a cloud GPU. The environment must not stall the
> whole stage.

## Optimisation variants

| Variant | What changes |
|---|---|
| Baseline | All data in global memory |
| Shared memory | Each polynomial is loaded into shared memory once |
| Warp shuffles | Inner layers exchange values with `__shfl_sync` instead of shared memory |
| Montgomery vs Barrett | Reduction used in modular multiplication |
| Batch size | 16, 64 and 256 polynomials |

## Tasks

| ID | Task | Due |
|---|---|---|
| S2.1 | NTT theory and a hand-computed 8-point example | 12 Nov |
| S2.2 | Accel-Sim installed and running | 15 Nov |
| S2.3 | Reference NTT in C, checked against naive multiplication, for both q | 17 Nov |
| S2.4 | Batched CUDA NTT matching the C output | 19 Nov |
| S2.5 | Variant comparison (cycles and simulated power) | 23 Nov |
| S2.6 | Slides | 25 Nov |
| S2.7 | Video | 28 Nov |

Live status: [TASKS.md](../TASKS.md)

## Definition of done

- The CUDA kernel produces exactly the C output in the simulator, for both values of q.
- Someone else can install the simulator and run the kernel from this folder's instructions alone.
- The comparison table is complete for every variant.
- The video is uploaded.

## Results

*To be filled in as the stage progresses.*
