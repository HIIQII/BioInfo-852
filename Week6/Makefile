# Makefile: Align short reads with BWA
# Author: Yue Shi
# Example: Ebola genome alignment

# Variables
ACC = AF086833
SRR = SRR1972739
REF_DIR = refs
READ_DIR = reads
BAM_DIR = bam
REF = $(REF_DIR)/ebola-1976.fa
R1 = $(READ_DIR)/$(SRR)_1.fastq
R2 = $(READ_DIR)/$(SRR)_2.fastq
SAM = $(BAM_DIR)/$(SRR).sam
BAM = $(BAM_DIR)/$(SRR).bam

# Default target: run everything
all: $(BAM) $(BAM).bai

# 1. Create directories
$(REF_DIR) $(READ_DIR) $(BAM_DIR):
	mkdir -p $(REF_DIR) $(READ_DIR) $(BAM_DIR)

# 2. Download the reference genome
$(REF): | $(REF_DIR)
	bio fetch --format fasta $(ACC) > $(REF)

# 3. Download the sequencing reads
$(R1) $(R2): | $(READ_DIR)
	fastq-dump -X 10000 --outdir $(READ_DIR) --split-files $(SRR)

# 4. Index the genome
index: $(REF)
	bwa index $(REF)

# 5. Align reads and produce SAM
$(SAM): index $(R1) $(R2) | $(BAM_DIR)
	bwa mem -t 4 $(REF) $(R1) $(R2) > $(SAM)

# 6. Sort and produce BAM
$(BAM): $(SAM)
	samtools sort $(SAM) -o $(BAM)

# 7. Index BAM file
$(BAM).bai: $(BAM)
	samtools index $(BAM)

# 8. Clean intermediate files
clean:
	rm -rf $(REF_DIR) $(READ_DIR) $(BAM_DIR)

.PHONY: all index clean
