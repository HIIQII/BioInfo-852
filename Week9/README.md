Week 9 Assignment
Yue Shi

## Problem in the last assignment
The code does not use GNU parallel
There is no readme.md
There is no use of the design.csv file

Appologize for that, there's something wrong when i push my files to repository and it just lost all the files that i upload to the github.

## Assignment 9
Used Parallel command to analyze all the SRA and alignment parellely


## The SRR sample I get frmo the NCBI only have 8 experiments

## 
Get all the SRR sample from the BioProj
```bash
# Get metadata for a BioProject
bio search PPRJNA313294 -H --csv > metadata.csv
```
## Althernative pathway to download SRA name and SRA number
I had trouble to download the SRA using the previous code, so i directly download the SraRunTable(also attached in the assignment) and convert align the number and names using the code below.

## Convert the metadate.csv into design.csv
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

## Makefile Work Flow


1. **Get genome and GFF files**  
   Downloads the Zika reference genome (`.fa`) and gene annotation (`.gff`) from NCBI.

2. **Index the genome**  
   Builds the BWA index files required for alignment (only needs to run once).

3. **Read design.csv**  
   Reads the list of SRR accessions and sample names to process in parallel using GNU Parallel.

4. **For each sample:**  
   - **Download reads:** Retrieves FASTQ files from SRA using `fastq-dump` (subset of 100,000 reads).  
   - **Align reads:** Runs `bwa mem` to align reads to the genome, generating sorted BAM files.  
   - **Index BAM:** Uses `samtools index` for IGV visualization.  
   - **Generate stats:** Summarizes alignment quality with `samtools flagstat` and `samtools coverage`.  
   - **Create coverage tracks:** Converts BAM files to `.bedgraph` and `.bw` for genome browser viewing.

5. **Clean up**  
   `make clean` removes all generated data and resets the workspace.

---

## Summary
