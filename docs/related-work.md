# Related Work and the Research Gap

[← back to overview](../README.md) · *Initial search: October 2026*

## The gap

**No published side-channel evaluation of an ML-KEM or ML-DSA implementation on a GPU was found.**
GPU implementations report performance only, and attacks on ML-KEM target CPUs, microcontrollers
and FPGAs.

- **A real, unevaluated target.** NVIDIA ships cuPQC (ML-KEM and ML-DSA on GPU) with a claim of
  side-channel protections, and it is being integrated into liboqs. No independent evaluation of that
  claim was found.
- **The tools exist; the connection does not.** Owl detects leakage in CUDA code but was evaluated on
  AES, PyTorch and nvJPEG, not on PQC. Energon extracted information through nvidia-smi without root,
  but about AI models, not cryptographic keys.
- **The main obstacle.** On A100 and H100, NVML samples only about a quarter of the runtime and
  updates roughly every 100 ms. This is what makes the question a research question.

## Literature map

### ML-KEM side channels (CPU, microcontroller, FPGA)

| Work | Year | Target | Channel | Key finding | Relevance |
|---|---|---|---|---|---|
| [KyberSlash](https://kannwischer.eu/papers/2024_kyberslash_preprint20240628.pdf) | 2024 | Cortex-M4, Raspberry Pi 2 | Timing | Secret-dependent division leaks; keys in minutes to hours | Check GPU kernels for secret divisions |
| [More Practical Single-Trace Attacks on the NTT](https://eprint.iacr.org/2019/795.pdf) | 2019 | Cortex-M4 (pqm4) | Power | SASCA on the NTT incl. lazy reduction; up to 95% success | The NTT on a GPU as a new target |
| [Single-Trace Attacks on Masked Lattice-Based Encryption](https://eprint.iacr.org/2017/594.pdf) | 2017 | Microcontroller | Power | One trace suffices, even against masking | Basis for an NTT leakage model |
| [Template and CPA on Pair-Pointwise Multiplication](https://eprint.iacr.org/2025/1577) | 2025 | Microcontroller | Power | With LDA, from ~300 to at most 40 traces | A step that runs batched on GPUs |
| [One Time is Enough](https://dl.acm.org/doi/10.1007/978-3-032-01806-9_2) | 2025 | pqm4 | Power (SPA) | Leakage in inverse NTT and decoding | Chosen-ciphertext fits batching |
| [DVFS-based Attacks against NTT](https://eprint.iacr.org/2024/070.pdf) | 2024 | CPU | Frequency (Hertzbleed-style) | First DVFS attack on the NTT | Same idea on GPU DVFS is open |

### Side channels on GPUs (non-PQC)

| Work | Year | Target | Channel | Key finding | Relevance |
|---|---|---|---|---|---|
| [Rendered Insecure](https://www.cs.ucr.edu/~zhiyunq/pub/ccs18_gpu_side_channel.pdf) | 2018 | NVIDIA | Counters, memory | Practical GPU side channels | Attacker model |
| [Cracking Randomized Coalescing](https://dl.acm.org/doi/pdf/10.1145/3337167.3337169) | 2019 | NVIDIA | Timing (coalescing) | AES key even against RCoal | NTT accesses are fixed; likely does not transfer |
| [Bank Conflict-based Timing Leakage](https://dl.acm.org/doi/fullHtml/10.1145/3361870) | 2019 | NVIDIA | Timing (shared memory) | Breaks table-based crypto | Check shared-memory access in the NTT |
| [GPU acceleration of RSA is vulnerable](https://www.researchgate.net/publication/328775402_GPU_acceleration_of_RSA_is_vulnerable_to_side-channel_timing_attacks) | 2018 | NVIDIA | Timing | RSA private key recovered | Precedent: asymmetric crypto on GPUs leaks |
| [Hot Pixels](https://arxiv.org/pdf/2305.12784) | 2023 | GPUs, ARM SoCs | Frequency, power, temperature | Acknowledged by all vendors | GPU DVFS as a possible channel |
| [Spy in the GPU-box](https://arxiv.org/pdf/2203.15981) / [NVBleed](https://arxiv.org/pdf/2503.17847) | 2022 / 2025 | Multi-GPU, NVLink | Contention | Cross-GPU channels | Data-centre attacker model |

### Software power telemetry

| Work | Year | Target | Channel | Key finding | Relevance |
|---|---|---|---|---|---|
| [PLATYPUS](https://www.arcsi.fr/doc/platypus.pdf) | 2021 | Intel (RAPL) | Software power | Keys from SGX; fixed with noise and access control | Main precedent for NVML |
| [PWRLEAK](https://par.nsf.gov/servlets/purl/10439395) | – | AMD CPU | Software power | >80% accuracy on RSA keys in SEV | Same channel at another vendor |
| [Energon](https://arxiv.org/pdf/2508.01768) | 2025 | NVIDIA (nvidia-smi) | Power, thermal | Infers transformer structure without root; not blocked by MIG | NVML leaks, but not yet keys |
| [Part-time Power Measurements](https://arxiv.org/html/2312.02741v2) | 2024 | 70+ NVIDIA GPUs | – | A100/H100 sample only ~25% of runtime | The obstacle to overcome |

### PQC implementations on GPUs

| Work | Year | Target | Key finding | Relevance |
|---|---|---|---|---|
| [cuPQC](https://developer.nvidia.com/cupqc) | 2024–25 | NVIDIA | ML-KEM/ML-DSA on GPU, claims side-channel protections; [in liboqs](https://www.linuxfoundation.org/press/post-quantum-cryptography-alliance-brings-accelerated-computing-to-post-quantum-cryptography-with-nvidia-cupqc) | Main target for independent evaluation |
| [HI-Kyber](https://eprint.iacr.org/2023/1194) | 2023 | NVIDIA | 1,664 kops/s key exchange; no side-channel evaluation | Academic comparison target |
| [On GPU acceleration of PQC algorithms](https://eprint.iacr.org/2025/1596.pdf) | 2025 | A100, RTX 2070 | Batched Kyber benchmarks | Performance reference |
| [High-Throughput GPU Dilithium](https://arxiv.org/pdf/2211.12265) | 2022 | NVIDIA | Constant-time NTT claimed, not tested | Untested constant-time claim |

### Tools and confidential computing

| Work | Year | Target | Key finding | Relevance |
|---|---|---|---|---|
| [Owl](https://dsn2024uq.github.io/Proceedings/pdfs/DSN2024-6rvE3SSpzFYmysif75Dkid/410500a362/410500a362.pdf) | 2024 | CUDA | Leakage detection in libgpucrypto, PyTorch, nvJPEG | The code-analysis tool; not yet applied to PQC |
| [Survey: Confidential Computing on CPU-GPU Systems](https://arxiv.org/html/2408.11601v3) | 2025 | H100 | CC mode disables performance counters; side channels still open | PhD-level extension |

## Search scope and next checks

About 15 targeted searches across IACR ePrint, arXiv, ACM and vendor sites (October 2026). Details
come from abstracts and excerpts, not full reads.

- [ ] Read the hardening section of the cuPQC documentation: which protections exactly are claimed
- [ ] Forward citations (Google Scholar) of Owl, Rendered Insecure and Part-time Power Measurements
- [ ] Scan CHES/TCHES, USENIX Security, CCS and S&P proceedings for 2025–2026
- [ ] Check whether Owl's code is public and runnable
