Week 10 Assignment
Yue Shi

---

## Overview
This assignment extends the Week 9 multi-sample alignment pipeline to perform variant calling and produce multi-sample VCF files.  
Using RNA-Seq reads from the Zika virus BioProject (PRJNA313294), I aligned multiple SRA samples to the reference genome, evaluated genome coverage, and identified SNP variants across all samples.

---

## Workflow Summary

### 1. Genome and Annotation
- Downloads the Zika virus reference genome (.fa) and annotation (.gff) from NCBI.  
- Indexes the reference for alignment (bwa index) and variant calling (samtools faidx).  

### 2. Per-Sample Alignment (make bam)
For each SRR sample, the pipeline:
1. Downloads 100,000 reads using fastq-dump.  
2. Aligns reads to the reference with bwa mem.  
3. Sorts and indexes the BAM with samtools sort and samtools index.  
4. Computes alignment statistics and coverage with samtools flagstat and samtools coverage.  
5. Generates a coverage track (.bedgraph → .bigwig) for visualization.  

### 3. Variant Calling (make vcf)
- Runs bcftools mpileup and bcftools call to identify SNPs and small variants from each sample’s BAM.  
- Compresses the output as .vcf.gz and indexes with tabix.  

### 4. Multi-Sample VCF Merge (make merge_vcf)
- Collects all individual .vcf.gz files in data/.  
- Merges them into a single multi-sample VCF using bcftools merge.  
- Indexes the final file for IGV visualization.  

---

## Commands to Run

### Step 1:  Generate BAM and VCF for each sample with coverage
3 Examples to generate bam files and vcf files. 
SRR3191544, SRR3194430, and SRR3194431

```bash
make all SRR=SRR3191544 SAMPLE=Zika_SRR3191544 LAYOUT=SINGLE
make all SRR=SRR3194430 SAMPLE=Zika_SRR3194430 LAYOUT=SINGLE
make all SRR=SRR3194431 SAMPLE=Zika_SRR3194431 LAYOUT=SINGLE
```

This will create .bam, vcf.gz, and coverage data.

### Step 2: Merge all the VCF files
```bash
make merge_vcf
```

### Step 3: Visualize in IGV
1. bam alignment and VCF for 1 sample 
<img width="1701" height="920" alt="Highest Coverage Sample" src="https://github.com/user-attachments/assets/a2cd6533-07b8-4e7b-be67-b4f68f56c6b7" />
2. VCF for 3 samples that have coverage
<img width="1705" height="471" alt="VCF" src="https://github.com/user-attachments/assets/95475bcf-a8c0-4f58-8da4-f88a716b2bae" />

### Step 4: Clean Up
```bash
make clean
```
