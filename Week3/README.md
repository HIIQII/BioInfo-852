Hi This is Yue Shi, This is my Week3 Assignment

Code Starts here:

#### How I download the data and Analyze it first hand
```bash 
wget https://ftp.ensemblgenomes.ebi.ac.uk/pub/bacteria/current/fasta/bacteria_0_collection/aeromonas_hydrophila_subsp_hydrophila_atcc_7966_gca_000014805/dna/Aeromonas_hydrophila_subsp_hydrophila_atcc_7966_gca_000014805.ASM1480v1.dna.toplevel.fa.gz
gunzip Aeromonas_hydrophila_subsp_hydrophila_atcc_7966_gca_000014805.ASM1480v1.dna.toplevel.fa.gz
mv Aeromonas_hydrophila_subsp_hydrophila_atcc_7966_gca_000014805.ASM1480v1.dna.toplevel.fa Ahsha.fa
cat Ahsha.fa | grep ">"
```
#### How I download the gff file and analyze it
```bash 
wget https://ftp.ensemblgenomes.ebi.ac.uk/pub/bacteria/current/gff3/bacteria_0_collection/aeromonas_hydrophila_subsp_hydrophila_atcc_7966_gca_000014805/Aeromonas_hydrophila_subsp_hydrophila_atcc_7966_gca_000014805.ASM1480v1.62.gff3.gz
gunzip Aeromonas_hydrophila_subsp_hydrophila_atcc_7966_gca_000014805.ASM1480v1.62.gff3.gz
mv Aeromonas_hydrophila_subsp_hydrophila_atcc_7966_gca_000014805.ASM1480v1.62.gff3 Ahsha.gff
```
#### How big is the genome, and how many features of each type does the GFF file contain?
```bash
seqkit stats Ahsha.fa
grep -v "^#" Ahsha.gff | wc -l
```
#### From your GFF file, separate the intervals of type "gene" or "transcript" into a different file.
```bash 
cat Ahsha.gff | grep -v "^#" | grep -w "gene" > Ahsha_genes.gff3
```


