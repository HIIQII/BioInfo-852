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
<img width="443" height="362" alt="a2cd6064fb9ae9a715f6594e1437d57" src="https://github.com/user-attachments/assets/aa47f0a7-9d4c-482a-8bac-ca245f45112d" />
<img width="353" height="432" alt="83d367016fe9bd1e4a2a575be4a31c6" src="https://github.com/user-attachments/assets/1787e985-fb3b-4d15-9fe2-d254cedb3c41" />

## Discussion


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

