# GeneWeaver

GPU-accelerated CRISPR off-target search. Scans a genome (FASTA) for sites matching a
20-bp guide RNA plus an NGG PAM, allowing up to *k* mismatches. Python CPU baseline
vs. Numba CUDA kernel, with a Textual terminal dashboard.

## Setup
```bash
pip install -r requirements.txt
```
Requires an NVIDIA GPU + CUDA toolkit for GPU mode. CPU mode works anywhere.

Table of Contents
Features
How It Works
Architecture
Setup
Usage
Testing
Project Structure
Design Decisions
Limitations
Troubleshooting
Roadmap
Benchmarks


How It Works
Parse: read each FASTA record, uppercase it, and encode bases as uint8 (A=0, C=1, G=2, T=3, anything else such as N = 4).
Chunk: split each chromosome into fixed-size chunks that overlap by pattern_length - 1 (22 bp), so every possible site lies fully inside exactly one chunk.
Align: for each window, check the PAM first (positions 21-23 must be NGG), then count mismatches against the guide, stopping early once the limit is exceeded.
Collect: convert chunk-local positions to chromosome positions and report (chromosome, position, mismatches) for every hit.

On the GPU, step 3 runs for all windows of a chunk in parallel. Host-to-device copies and kernel launches use preallocated buffers that are reused for every chunk.


## Features
- FASTA ingestion and cleaning with BioPython
- Overlapping chunking so no site is missed at a chunk boundary
- Pure-Python CPU baseline for correctness checks and timing reference
- Numba `@cuda.jit` kernel: one GPU thread per genome position
- Benchmark harness that times both backends and verifies identical results
- Textual TUI with progress bars (GPU stats panel coming in Week 3)
- Mock genome generator with planted off-target sites


## Structure
| Path | Purpose |
|---|---|
| `geneweaver/parser.py` | BioPython FASTA parsing, encoding, overlapping chunks |
| `geneweaver/cpu_aligner.py` | Pure-Python baseline |
| `geneweaver/gpu_aligner.py` | Numba `@cuda.jit` kernel, buffer management |
| `geneweaver/pipeline.py` | Shared scan loop |
| `geneweaver/tui.py` | Textual dashboard |
| `benchmark.py` | CPU vs GPU timing and result verification |




## Benchmarks
| Genome | CPU (s) | GPU (s) | Speedup |
|---|---|---|---|
| _fill in_ | | | |
