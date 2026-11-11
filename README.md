# GeneWeaver
GPU-Accelerated CRISPR Alignment Engine ----- Domain: Bioinformatics
Problem Statement: Searching the human genome (3.2 billion base pairs) for unintended "off-target"
CRISPR DNA mutations requires massive compute. Standard Python string matching (regex or simple loops)
takes days to process a single genome, forcing researchers to rely on expensive, proprietary C++ software
clusters.

Objective: Establish a performance baseline and build the genomic data ingestion pipeline.

Work Done --

Implemented a pure-Python sliding-window DNA alignment that scans a genome for candidate CRISPR off-target sites, allowing up to k mismatches against a 20-bp guide RNA plus PAM (NGG).
Built the genomic parser with BioPython (SeqIO) to read .fasta files, clean the sequences (uppercase, handle N bases, strip headers), and split them into fixed-size overlapping chunks. The overlap is guide length minus 1, so matches on chunk boundaries aren’t missed.
Encoded bases as uint8 NumPy arrays (A=0, C=1, G=2, T=3) so the data can later be copied directly to the GPU.
Created a benchmarking harness to record runtime, throughput (base pairs/sec), and memory use on a mock genome.

Challenges & Solutions --

Slow pure-Python loops: this was expected and is the baseline to beat. Results were recorded as the reference.
Boundary misses between chunks: fixed with overlapping chunk windows.
High RAM use on large files: switched to streaming iteration and compact uint8 arrays.


Outcome: A working, validated CPU aligner and chunked data pipeline, with a baseline of [X] seconds on [N] Mbp, ready for GPU porting.
