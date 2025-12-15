## Week 14 – RNA-Seq differential expression
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

## Continue the work from last week we will analyze the differential expression of RNA seq data.

## Convert to Stats environment
```bash 
conda activate stats
```

## Convert the raw count matrix to CSV format
```bash
awk 'BEGIN{OFS=","}
NR==2{ gsub("bam/","",$0); gsub(".bam","",$0); print "name",$7,$8,$9,$10,$11,$12; next }
NR>2{ print $1,$7,$8,$9,$10,$11,$12 }' counts.txt > counts.csv
```

## Run differential expression analysis with edgeR
```bash
Rscript src/r/edger.r -d design.csv -c counts.csv
```

## PCA Plot
```bash
src/r/plot_pca.r -c edger.csv -d design.csv -o pca.pdf
```
<img width="988" height="271" alt="image" src="https://github.com/user-attachments/assets/425f2366-b591-44bb-a5e0-7e042fe08414" />

PCA interpretation: Principal component analysis (PCA) shows clear separation between HBR and UHR samples. PC1 explains 97% of the total variance and distinctly separates the two conditions, indicating that biological differences between HBR and UHR dominate the expression signal. Replicates within each group cluster tightly together, while PC2 explains only 1% of the variance,suggesting low technical variation and good reproducibility among samples. Overall, the PCA confirms strong condition-specific transcriptional differences in the RNA-Seq data.

## Heat Map
```bash
src/r/plot_heatmap.r -c edger.csv
```
<img width="861" height="827" alt="image" src="https://github.com/user-attachments/assets/23187594-a345-4b4e-bc6b-8ad8a20d27b1" />

The heatmap shows clear separation of samples by biological condition.
HBR samples cluster together and display highly similar expression patterns,
while UHR samples form a distinct cluster with opposing expression profiles.
Large blocks of genes show inverse regulation between the two groups,
with genes highly expressed in UHR appearing low in HBR, and vice versa.
This indicates strong, coordinated transcriptional differences rather than
random variation.


## Functional Enrichment
```bash
bio gprofiler -c edger.csv -d hsapiens
bio enrichr -c edger.csv
```

