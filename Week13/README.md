## Week 13 – RNA-Seq Read Alignment and Gene Count Matrix
Yue Shi

---

## Overview

This project performs a complete RNA-Seq analysis using six single-end samples from the UHR–HBR dataset aligned to chromosome 22.  
The workflow includes:

1. Preparing the reference genome and annotation  
2. Building a HISAT2 index  
3. Aligning six RNA-Seq samples  
4. Generating BAM and BigWig coverage files  
5. Running featureCounts to produce a gene-level count matrix  
6. Visualizing aligned reads in IGV to confirm RNA-Seq characteristics  

---

## Download and unpack the UHR–HBR chr22 dataset
```bash
wget -nc http://data.biostarhandbook.com/data/uhr-hbr.tar.gz
tar xzvf uhr-hbr.tar.gz
```

## Build the HISAT2 index for chr22
```bash
make index
```

## Align all six RNA-Seq samples (creates BAM + BigWig files)
```bash
make align
```

## Generate the gene-level count matrix (counts.txt)
```bash
make counts
```

## Discussion
By pulling out the first 50 lines of the count matrix, they mainly contain genes with zero counts in all samples, which is expected because RNA-seq reads were aligned only to chromosome 22, and many chr22 genes simply do not receive reads in the UHR–HBR subset.
However, several genes in this region show clear expression differences between the UHR and HBR samples. These lines illustrate how the count matrix reflects real biological variation that can be validated in IGV.
Inspection of the first several lines of the count matrix shows that most chr22 genes have zero counts across all samples, which is expected for this reduced dataset. However, several genes display clear and consistent expression differences between UHR and HBR. For example, ENSG00000280341.1 shows counts of 3 and 2 in UHR samples but zero in all HBR samples, while ENSG00000279442.1 is expressed only in UHR_2 and UHR_3. In IGV, these genes show small but distinct exon-aligned peaks in UHR tracks and flat coverage in HBR tracks, confirming that the observed numerical differences accurately reflect the underlying read alignments. Genes such as ENSG00000272872.1, which shows high UHR coverage and very low HBR coverage, further illustrate consistent expression patterns that match both the count matrix and the visual RNA-seq signal.

## Visualize in IGV
## HBR-1
<img width="1896" height="286" alt="image" src="https://github.com/user-attachments/assets/e0b46240-88d5-4734-9d3b-4d90c67563fe" />
## HBR-2
<img width="1897" height="508" alt="image" src="https://github.com/user-attachments/assets/3c078ec9-ffbd-42a0-b6bb-220bfe532b82" />
## HBR-3
<img width="1897" height="505" alt="image" src="https://github.com/user-attachments/assets/74150b9f-3143-4147-b632-91692827c77f" />
## UHR-1
<img width="1897" height="622" alt="image" src="https://github.com/user-attachments/assets/85243d7a-e35a-45a2-a14e-a5e57ae1631b" />
## UHR-2
<img width="1897" height="499" alt="image" src="https://github.com/user-attachments/assets/8996a2bb-995f-4d02-bb0b-96e0484d6f43" />
## UHR-3
<img width="1897" height="564" alt="image" src="https://github.com/user-attachments/assets/87abe4ac-c1b2-4b39-85ce-ff0574225cfa" />

## 
Visualization demonstrates it is RNA-Seq Data because only at the exon region it have high covery rate.
