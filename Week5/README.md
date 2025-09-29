Week 4 Assignment

#### How I access and down load the data 
```bash 
datasets summary genome accession GCF_000882815.3 | jq
datasets download genome accession GCF_000882815.3 --include genome,gff3,gtf
unzip -n ncbi_dataset.zip
mv GCF_000882815.3_ViralProj36615_genomic.fna Zikagenome.fa
```
#### Visualize genome gff gtf on IGV
....


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
#### Since there's only 1 gene encodes the Zika Virus protein, the gene is called Polyprotein for Zika Virus and it is to  serve as a precursor that gets processed into all functional proteins required for Zika virus structure and replication.


####
The polyprotein CDS covers almost the entire genome, with mature_protein_region_of_CDS features nested inside.

There is no intragenic “junk” space like in human genomes.

Only very short UTRs at the ends are non-coding.


#### alternative genome builds
```bash
Accession numbers : GCF_002366285.1
```

#### the focus of the paper and alternative questions I can answer
```bash
By using a different genome build, I could shift the research question from “What is the RNA structure of Zika virus?” to “How has the Zika virus genome and its structural elements evolved between African and Asian lineages, and what might this mean for viral transmission and pathogenesis?”
```

Week 5 Assignment
#### The accession number for the Paper
```bash
BioProject: PRJNA313294
SRA Study: SRR3194431	
```

#### Access the data through the SRR accession number
```bash
bio search SRR3194431
```

#### Get the first 1000000 reads if subset date from SRA
```bash
mkdir -p reads
fastq-dump -X 1000000 -F --outdir reads --split-files SRR3194431
ls reads
```
#### 10X genome coverage
#### Since the size of read from the SRA is too large, in order to get a 10X coverage I think 1000000 reads would be appropriate to get a 10X coverage rate.

#### Quality assessment
```bash
seqkit stats reads/SRR3194431_1.fastq
fastqc reads/SRR3194431_1.fastq
```

#### Evaluation 

#### The per base sequence quality and per sequence quality modules show consistently high read quality, which indicates the dataset is technically sound. However, the per base sequence content is uneven, especially at the beginning and end of reads. This is a common artifact in RNA-seq libraries caused by priming and adapter effects, and while flagged by FASTQC, it is not unexpected and should not seriously impact downstream analyses after trimming.

#### uality Control
```bash
set -uex
SRR=SRR3194431
N=100000
R1=reads/${SRR}_1.fastq
T1=reads/${SRR}_1.trimmed.fastq
ADAPTER=AGATCGGAAGAGCACACGTCTGAACTCCAGTCA
READS=reads
REPORTS=reports
mkdir -p ${READS} ${REPORTS}
fastq-dump -X ${N} --split-files -O ${READS} ${SRR}
fastqc -o ${REPORTS} ${R1}
fastp --adapter_sequence=${ADAPTER} \
      --cut_tail \
      -i ${R1} -o ${T1}
fastqc -o ${REPORTS} ${T1}
micromamba run -n menv multiqc -o ${REPORTS} ${REPORTS}
```
#### Altenative SRA 

#### SRR3191545 Sequenced on Illumina as well but this project use MiSeq instead of NextSeq.

#### The Illumina NextSeq 500 dataset (SRR3194431) provides very high throughput short reads (~75 bp), making it adapt for transcript quantification but less informative for assembly. By contrast, an Illumina MiSeq(SRR3191545) run of the Zika virus genome yields fewer reads but at longer lengths (150–300 bp, often paired-end), which facilitates de novo assembly and improves mapping accuracy. Both platforms generate high-quality data, but they are complementary: NextSeq excels in depth, while MiSeq excels in read length and assembly power.


