Hi This is My Week 2 Assignment

#### The Species I picked 
poecilia_mexicana
Poecilia mexicana (Atlantic molly) is a small freshwater fish in the family Poeciliidae, native to Central America. It’s closely related to the guppy and is often studied in evolutionary biology and environmental adaptation.


#### How I download the GFF document and analze the files when I firstly get them
```bash
wget https://ftp.ensembl.org/pub/current_gff3/poecilia_mexicana/Poecilia_mexicana.P_mexicana-1.0.115.gff3.gz
rm *.txt
gunzip Poecilia_mexicana.P_mexicana-1.0.115.gff3.gz
ls -l
cat Poecilia_mexicana.P_mexicana-1.0.115.gff3 | wc -l
cat Poecilia_mexicana.P_mexicana-1.0.115.gff3 | head
bash

#### How many sequence regions (chromosomes) does the file contain? Does that match with the expectation for this organism?
```bash
cat Poecilia_mexicana.P_mexicana-1.0.115.gff3 | grep KQ | wc -l
bash
#### The output is 813379 which is unexpect for this organism

#### Remove the useless data from the file 
```bash 
cat Poecilia_mexicana.P_mexicana-1.0.115.gff3 | grep -v '##sequence-' | head
cat Poecilia_mexicana.P_mexicana-1.0.115.gff3 | grep -v '#' | head -1
cat Poecilia_mexicana.P_mexicana-1.0.115.gff3 | grep -v '#' > pm.gff3
bash

#### How many features does the file contain
```bash
grep -v "^#" Poecilia_mexicana.P_mexicana-1.0.115.gff3 | wc -l
bash

#### How many genes are listed for this organism?
cat pm.gff3 | cut -f 3 | sort | uniq |head
cat pm.gff3 | cut -f 3 | sort | uniq -c |head
cat pm.gff3 | cut -f 3 | sort | uniq -c | sort -rn |head
bash

#### Output
 353979 exon
 345083 CDS
  34933 mRNA
  26104 five_prime_UTR
  24074 gene
  18400 three_prime_UTR
  18105 region
  17522 biological_region
    359 ncRNA_gene
    190 snoRNA

#### Is there a feature type that you may have not heard about before? What is the feature and how is it defined?
#### The feature name that I havent heard is snoRNA, 
