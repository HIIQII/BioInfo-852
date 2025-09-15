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

#### 
By playing with the original gff and simplified gff files, I found that the simplified gff file is well organized compare to the original version, There will be only 1 coding seuquence for a gene, while the original gff usually provide different sequence for the same location of the genome.

#### Upload the IGV screenshot to the Repository
#### Translation table
<img width="1919" height="1077" alt="Translation Table" src="https://github.com/user-attachments/assets/215642d7-3a61-4715-a02c-a253ae9ff5c4" />
#### First coidng sequence start with start codon
<img width="1916" height="1071" alt="First coidng sequence start with start codon" src="https://github.com/user-attachments/assets/a10faebd-ee0c-4668-a37c-f23a0ebe4eb9" />
#### last coidng sequence start with stop codon
<img width="1917" height="1076" alt="Last coding sequence end with Stop Codon" src="https://github.com/user-attachments/assets/bd331a44-12e6-4701-a956-4a20b41d0902" />
