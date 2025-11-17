# Week 12 – Multi-Platform Read Alignment Comparison (GIAB HG008)
Yue Shi

---

## Overview
This project assesses the performance of various sequencing platforms when aligned to the human genome.  
A small genomic region—the **KRAS locus** (chr12:25,205,246–25,250,936)—is extracted from three platforms for comparison.

## Command
```bash
make illumina
make ont
make aviti
```

Workflow steps:  
1. Download reference genome  
2. Extract region directly from remote BAMs  
3. Generate regional BAMs and indexes  
4. Compute alignment statistics  
5. Visualize and compare in IGV  

---

## Sequencing Platforms Analyzed

### 1. Illumina NovaSeq 6000 (118×, PCR-free)
Short-read 2×150 bp sequencing  
High accuracy for small variants  
Dense and uniform coverage  

### 2. Element Aviti (77×, PCR-free)
Short-read, PCR-free chemistry  
Very low duplicate rate  
Clean and uniform alignments  

### 3. Oxford Nanopore ONT R10.41 (41×)
Long-read sequencing (10–200 kb)  
Useful for structural variant detection  
Lower read count in small windows  

---

## Alignment Statistics for the KRAS Region

| Platform | Total Reads | Primary | Supplementary | Duplicates | Mapping Rate |
|----------|-------------|----------|----------------|------------|--------------|
| Illumina NovaSeq | 45,834 | 45,783 | 51 | 9,384 | 99.78% |
| Element Aviti | 28,035 | 27,992 | 43 | 0 | 99.85% |
| Oxford Nanopore ONT | 159 | 143 | 16 | 0 | 100% |

---

## Pros and Cons of Each Platform

### Illumina NovaSeq

Illumina provides the highest read depth and uniform short-read coverage, enabling accurate detection of small variants in the KRAS region. However, its reads are short and cannot span structural variants, and duplicate reads are common due to the library preparation process.

---

### Element Aviti
Aviti delivers clean, PCR-free short-read alignments with almost no duplicates and highly uniform base quality. It performs similarly to Illumina but with slightly fewer reads, and it still shares the same intrinsic limitations of short-read sequencing.

---

### Oxford Nanopore ONT
ONT produces long reads capable of spanning full introns and complex regions, making it powerful for structural variants and phasing. Its coverage in the KRAS window is sparse and indel error rates are higher, resulting in less detailed local alignment compared to short-read platforms.
---

## Interpretation
Illumina and Aviti produce dense short-read coverage, ideal for small variant detection in a confined region.  
ONT produces long-range reads but fewer overlapping the KRAS window.  
The three technologies provide complementary strengths.

---

## Visualization in IGV

illumina
```
<img width="1915" height="1069" alt="illumina" src="https://github.com/user-attachments/assets/617e63f3-fb2b-435c-8503-35ca43ce3a18" />

```

Nanopore oxford
```
<img width="1701" height="1057" alt="ont" src="https://github.com/user-attachments/assets/7f09f969-06d0-4a52-998c-3c370d53d6de" />

```

aviti
```
<img width="1919" height="1071" alt="aviti" src="https://github.com/user-attachments/assets/6fc88e37-8833-45cf-aacd-f3eb038d933d" />

```


