# Week 6 – Zika Virus Genome Alignment (Project)

This project demonstrates a complete short-read alignment workflow using **BWA** and **Samtools**.  
The dataset consists of Zika virus genomic data (GCF_000882815.3) and SRA sequencing reads (SRR3194431).
We are discovering which part of human genome are getting affected by the Zika Virus.
---

### Workflow Overview

The automated workflow (controlled by `Makefile`) performs:

1. **Download reference genome** from NCBI RefSeq  
2. **Download sequencing reads** from SRA (SRR3194431)  
```bash
fastq-dump -X 100000 --split-files --outdir $(DATA) $(SRA)
```
3. **Index the genome** (`bwa index`)  
4. **Align reads** (`bwa mem` → `samtools sort + index`)  
5. **Generate statistics** (`samtools flagstat` + `samtools coverage`)  
6. **Visualize** the resulting BAM file in IGV  

### Output
| Metric               | Value           |
| -------------------- | --------------- |
| Total Reads          | 10 954 371      |
| Mapped Reads         | 26 502 (0.24 %) |
| Genome Coverage      | 99.77 %         |
| Mean Depth           | 184×            |
| Mean Base Quality    | 32.9            |
| Mean Mapping Quality | 59.9            |



