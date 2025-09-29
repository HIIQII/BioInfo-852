I Select the Zika Paper

#### How I access and down load the data 
```bash 
datasets summary genome accession GCF_000882815.3 | jq
datasets download genome accession GCF_000882815.3 --include genome,gff3,gtf
unzip -n ncbi_dataset.zip
mv GCF_000882815.3_ViralProj36615_genomic.fna Zikagenome.fa
```
#### Visualize genome gff gtf on IGV
<img width="1678" height="475" alt="屏幕截图 2025-09-21 143528" src="https://github.com/user-attachments/assets/7e386d9e-308a-4bf1-8985-5c700957fa1e" />



#### Analyze genome data 
#### genome size / feature
```bash 
grep -v ">" Zikagenome.fa | wc -m
cut -f 3 genomic.gff | sort | uniq -c
```

###  What is the longest gene? What is its name and function?

#### Since there's only 1 gene encodes the Zika Virus protein, the gene is called Polyprotein for Zika Virus and it is to  serve as a precursor that gets processed into all functional proteins required for Zika virus structure and replication.

### Using IGV, estimate what proportion of the genome is covered by coding sequences.
#### The polyprotein CDS covers almost the entire genome, with mature_protein_region_of_CDS features nested inside.
#### There is no intragenic “junk” space like in human genomes.
#### Only very short UTRs at the ends are non-coding.


#### alternative genome builds
```bash
Accession numbers : GCF_002366285.1
```

### the focus of the paper and alternative questions I can answer

#### By using a different genome build, I could shift the research question from “What is the RNA structure of Zika virus?” to “How has the Zika virus genome and its structural elements evolved between African and Asian lineages, and what might this mean for viral transmission and pathogenesis?”

