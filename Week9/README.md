Week 9 Assignment
Yue Shi

## Problem in the last assignment

The code does not use GNU parallel

There is no readme.md

There is no use of the design.csv file


## Assignment 9
Added the Parallel command to analyze all the SRA and alignment in parallel.


## The SRR sample I get from the NCBI only has 8 experiments


Get all the SRR sample from the BioProj
```bash
# Get metadata for a BioProject
bio search PPRJNA313294 -H --csv > metadata.csv
```

## Althernative pathway to download SRR name and SRR number
I had trouble downloading the SRR using the provided code because of the site issue, so I directly downloaded the SraRunTable(also attached in the assignment) and converted and aligned the numbers and names using the code below.

## Convert the SraRunTable.csv into design.csv
```bash
awk -F'\t' '
BEGIN { print "sample,srr,layout" }
NR>1 { print $2","$1","$18 }
' SraRunTable.txt > design.csv
```
## Command to analyze all the SRR data based on the SRR number and names on the design.csv
```bash
make all
```

## Parellel Command
```bash
cat design.csv | tail -n +2 | parallel -j 4 --colsep ',' \
		'make sample SRR={2} NAME={1} LAYOUT={3}'
```
## Run a Single SRR Sample
```bash
make sample SRR=SRR3191542 NAME=RNA-Seq LAYOUT=SINGLE
```

## Makefile Workflow Overview

1. **Get genome and annotation files**  
   Downloads the Zika reference genome (`.fa`) and gene annotation (`.gff`) from NCBI.

2. **Index the genome**  
   Creates the BWA index and `.fai` genome index for alignment and coverage calculation.

3. **Read sample list**  
   Reads `design.csv` to iterate through all SRR samples using GNU Parallel.

4. **For each sample:**  
   - **Download reads:** Retrieves up to 100,000 reads from SRA using `fastq-dump`.  
   - **Align reads:** Uses `bwa mem` to align reads to the indexed genome and produces a sorted BAM file.  
   - **Sort & index BAM:** Sorts and indexes BAM files with `samtools sort` and `samtools index`.  
   - **Generate statistics:** Produces alignment summaries with `samtools flagstat` and `samtools coverage`.  
   - **Convert to coverage tracks:**  
     Converts BAM → BedGraph → BigWig for genome browser visualization.  
     If no coverage is detected, it prints:  
     ```
     No coverage data found for <sample_name>; skipping BigWig conversion.
     ```

5. **Manual BigWig regeneration**  
   If `.bw` files are missing or `.fai` was added later, rerun:
   ```bash
   make bigwig
6. ** Clean Up**
   ```bash
   make clean
   ```

---

## Summary
Throughout the alignment, I have met some readings have 0% coverage which really interesting. I didnt expect that would happen, when I look back to the NCBI SRR, There are some sequence that are mock experiments which could potentially explain about the problem.