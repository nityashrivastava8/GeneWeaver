# GeneWeaver

GPU-accelerated CRISPR off-target search. Scans a genome (FASTA) for sites matching a
20-bp guide RNA plus an NGG PAM, allowing up to *k* mismatches. Python CPU baseline
vs. Numba CUDA kernel, with a Textual terminal dashboard.

## Setup
```bash
pip install -r requirements.txt
```
Requires an NVIDIA GPU + CUDA toolkit for GPU mode. CPU mode works anywhere.

## Usage
```bash
python generate_mock_genome.py --size-mbp 10
python benchmark.py data/mock_genome.fa --mode both
python -m geneweaver.tui data/mock_genome.fa GACGCATAAAGATGAGACGC --backend gpu
pytest
```

## Structure
| Path | Purpose |
|---|---|
| `geneweaver/parser.py` | BioPython FASTA parsing, encoding, overlapping chunks |
| `geneweaver/cpu_aligner.py` | Pure-Python baseline |
| `geneweaver/gpu_aligner.py` | Numba `@cuda.jit` kernel, buffer management |
| `geneweaver/pipeline.py` | Shared scan loop |
| `geneweaver/tui.py` | Textual dashboard |
| `benchmark.py` | CPU vs GPU timing and result verification |

## Progress
- [x] Week 1: CPU baseline, parser, chunking, benchmark harness
- [x] Week 2: CUDA kernel, host/device transfers, TUI scaffold
- [ ] Week 3: Dask scaling, PAM-proximity scoring
- [ ] Week 4: shared-memory optimisation, mismatch highlighting

## Benchmarks
| Genome | CPU (s) | GPU (s) | Speedup |
|---|---|---|---|
| _fill in_ | | | |