## Week 13 – RNA-Seq Read Alignment and Gene Count Matrix (UHR–HBR chr22)
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

## Visualize in IGV
