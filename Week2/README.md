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

#### 

