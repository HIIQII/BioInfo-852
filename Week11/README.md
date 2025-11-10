Week 11 – Variant Calling and Functional Annotation
Author: Yue Shi

---

## Overview
This assignment extends the Week 9 multi-sample alignment pipeline to perform variant calling and produce multi-sample VCF files.  
Using RNA-Seq reads from the Zika virus BioProject (PRJNA313294), I aligned multiple SRA samples to the reference genome, evaluated genome coverage, and identified SNP variants across all samples.

---

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

---

## Commands to Run
```bash
make all SRR=SRR3191544 SAMPLE=Zika_SRR3191544 LAYOUT=SINGLE
make all SRR=SRR3194430 SAMPLE=Zika_SRR3194430 LAYOUT=SINGLE
make all SRR=SRR3194431 SAMPLE=Zika_SRR3194431 LAYOUT=SINGLE
```

## Each make all command performs the following:
1. Builds the reference index.
2. Downloads and aligns SRA reads.
3. Generates the BAM file and coverage report.
4. Calls variants and produces the compressed VCF.
5. Annotates variants using VEP (with GFF and FASTA).

## VEP 


## IGV