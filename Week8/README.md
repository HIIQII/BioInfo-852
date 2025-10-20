Week 8 Assignment
Yue Shi

## The SRR sample I get frmo the NCBI only have 8 experiments

## 
Get all the SRR sample from the BioProj
```bash
# Get metadata for a BioProject
bio search PPRJNA313294 -H --csv > metadata.csv
```

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

