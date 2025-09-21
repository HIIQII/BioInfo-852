I Select the Zika Paper

#### How I access and down load the data 
```bash 
datasets summary genome accession GCF_000882815.3 | jq
datasets download genome accession GCF_000882815.3 --include genome,gff3,gtf
unzip -n ncbi_dataset.zip
mv GCF_000882815.3_ViralProj36615_genomic.fna Zikagenome.fa
```
#### Visualize genome gff gtf on IGV
<img width="1678" height="475" alt="image" src="https://github.com/user-attachments/assets/100ed144-96fb-4499-b40d-449626fc17f7" />





#### Analyze genome data 
#### genome size / feature
```bash 
grep -v ">" Zikagenome.fa | wc -m
cut -f 3 genomic.gff | sort | uniq -c
```

#### Output
```bash
      1 #!genome-build ViralProj36615
      1 #!genome-build-accession NCBI_Assembly:GCF_000882815.3
      1 #!gff-spec-version 1.21
      1 #!processor NCBI annotwriter
      1 ###
      1 ##gff-version 3
      1 ##sequence-region NC_012532.1 1 10794
      1 ##species https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=64320
      1 CDS
      1 five_prime_UTR
      1 gene
     14 mature_protein_region_of_CDS
      1 region
      1 three_prime_UTR
```

####  What is the longest gene? What is its name and function?
```bash
Since there's only 1 gene encodes the Zika Virus protein, the gene is called Polyprotein for Zika Virus and it is to  serve as a precursor that gets processed into all functional proteins required for Zika virus structure and replication.
```

#### alternative genome builds
```bash
Accession numbers : GCF_002366285.1
```

#### the focus of the paper and alternative questions I can answer
```bash
By using a different genome build, I could shift the research question from “What is the RNA structure of Zika virus?” to “How has the Zika virus genome and its structural elements evolved between African and Asian lineages, and what might this mean for viral transmission and pathogenesis?”
