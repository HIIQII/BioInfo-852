Week 11 – Variant Calling and Functional Annotation
Author: Yue Shi


## Overview
This assignment extends the Week 9 multi-sample alignment pipeline to perform variant calling and produce multi-sample VCF files.  
Using RNA-Seq reads from the Zika virus BioProject (PRJNA313294), I aligned multiple SRA samples to the reference genome, evaluated genome coverage, and identified SNP variants across all samples.


## Workflow Summary

1. Genome and Annotation
   - Downloads the Zika virus reference genome (.fa) and annotation (.gff) from NCBI.
   - Indexes the reference for alignment (bwa index) and variant calling (samtools faidx).
   - The GFF file is sorted, compressed, and indexed for VEP annotation.

2. Per-Sample Alignment (make bam)
   - For each SRR sample, the pipeline:
     1. Downloads 100,000 reads using fastq-dump.
     2. Aligns reads to the reference genome with bwa mem.
     3. Sorts and indexes BAM files using samtools sort and samtools index.
     4. Computes alignment statistics and coverage using samtools flagstat and samtools coverage.
     5. Generates a coverage track (bedGraph → bigWig) for visualization in IGV.

3. Variant Calling (make vcf)
   - Uses bcftools mpileup and bcftools call to identify SNPs and small indels.
   - Compresses the results as .vcf.gz and indexes with tabix.
   - The resulting VCF files can be loaded into IGV with the reference and GFF.

4. Multi-Sample VCF Merge (make merge_vcf)
   - Collects all .vcf.gz files in data/.
   - Merges them into one multi-sample VCF using bcftools merge.
   - Indexes the merged VCF for IGV visualization.

5. Variant Effect Annotation (make vep)
   - Uses Ensembl VEP in offline mode to annotate each variant.
   - Employs the Zika virus GFF and FASTA as the annotation source.
   - Produces:
       results/<sample>_vep.txt              (annotation table)
       results/<sample>_vep.txt_summary.html (HTML summary)
       results/<sample>_vep.txt_warnings.txt (ignored feature log)

## Commands to Run
```bash
make all SRR=SRR3191544 SAMPLE=Zika_SRR3191544 LAYOUT=SINGLE
make all SRR=SRR3194430 SAMPLE=Zika_SRR3194430 LAYOUT=SINGLE
make all SRR=SRR3194431 SAMPLE=Zika_SRR3194431 LAYOUT=SINGLE
```

Analyze Effect of Variants
## VEP
SRR3191544
<img width="820" height="319" alt="image" src="https://github.com/user-attachments/assets/57312d5e-acf4-4788-8923-80e06dda1aaa" />

SRR3194431
<img width="812" height="316" alt="image" src="https://github.com/user-attachments/assets/07d609bf-1470-4adb-bae9-ef9bb67f058c" />

SRR3194430
<img width="818" height="299" alt="image" src="https://github.com/user-attachments/assets/8838cc1e-6800-4875-8c80-589c90f6cdf3" />

## IGV
<img width="1683" height="444" alt="image" src="https://github.com/user-attachments/assets/1c4c6c04-7bc7-4578-9160-f33995afcf33" />

## Summary
Some interesting variants I found:
### Position	Codon (ref→alt)  Nucleotide change*	  Amino-acid change	 Mutation type	    Likely effect (brief)
### 8,289	   GTG → GTC	     G → C (3rd base)	  Val → Val (V→V)	    Synonymous (SNP)	 Likely neutral (no AA change)
### 10,331   CTT → CCT	     T → C (1st base)	  Leu → Pro (L→P)	    Missense (SNP)	 Possibly disruptive; Pro can kink/break helices
### 6,352	   AGT → AGC	     T → C (3rd base)	  Ser → Ser (S→S)	    Synonymous (SNP)	 Likely neutral (no AA change)
### 6,340	   TTT → TGT	     T → G (2nd base)	  Phe → Cys (F→C)	    Missense (SNP)	 Potentially impactful; introduces cysteine (possible disulfide/thiol effects)

