# ------------------------------------------------------------
# Week 8 Makefile – Multi-sample Zika Virus Alignment Pipeline
# Author: Yue Shi
# ------------------------------------------------------------
# Usage:
#   make all
#   make sample SRR=SRR3191542 NAME=MiSeq_mock1 LAYOUT=PAIRED
# ------------------------------------------------------------

SHELL := /bin/bash
.IGNORE:  # Prevent harmless non-zero exits (like empty coverage) from halting make


THREADS = 4
DATA = data
GENOME_URL = https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/882/815/GCF_000882815.3_ViralProj36615/GCF_000882815.3_ViralProj36615_genomic.fna.gz
GFF_URL    = https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/882/815/GCF_000882815.3_ViralProj36615/GCF_000882815.3_ViralProj36615_genomic.gff.gz
GENOME = $(DATA)/Zikagenome.fa
GFF     = $(DATA)/Zikagenome.gff
INDEX_PREFIX = $(DATA)/ZikaIndex

# ------------------------------------------------------------
# Default target: run all samples in parallel from design.csv
# ------------------------------------------------------------
all: $(GENOME) $(GFF) index
	@echo "Running all samples from design.csv using GNU Parallel..."
	cat design.csv | tail -n +2 | parallel -j 4 --colsep ',' \
		'make sample SRR={2} NAME={1} LAYOUT={3}'

# ------------------------------------------------------------
# Download genome and annotation
# ------------------------------------------------------------
$(GENOME):
	mkdir -p $(DATA)
	curl -L $(GENOME_URL) | gunzip -c > $(GENOME)

$(GFF):
	mkdir -p $(DATA)
	curl -L $(GFF_URL) | gunzip -c > $(GFF)

# ------------------------------------------------------------
# Index genome for BWA alignment
# ------------------------------------------------------------
index: $(GENOME)
	bwa index -p $(INDEX_PREFIX) $(GENOME)

# ------------------------------------------------------------
# Per-sample workflow
# ------------------------------------------------------------
sample:
	@echo "Processing $(NAME) ($(SRR))..."
	mkdir -p $(DATA)

	# Step 1: Download reads
	if [ "$(LAYOUT)" = "PAIRED" ]; then \
		fastq-dump -X 100000 --split-files -O $(DATA) $(SRR); \
	else \
		fastq-dump -X 100000 -O $(DATA) $(SRR); \
	fi

	# Step 2: Align reads to genome
	if [ "$(LAYOUT)" = "PAIRED" ]; then \
		bwa mem -t $(THREADS) $(INDEX_PREFIX) $(DATA)/$(SRR)_1.fastq $(DATA)/$(SRR)_2.fastq | samtools sort -o $(DATA)/$(NAME)_$(SRR).sorted.bam; \
	else \
		bwa mem -t $(THREADS) $(INDEX_PREFIX) $(DATA)/$(SRR).fastq | samtools sort -o $(DATA)/$(NAME)_$(SRR).sorted.bam; \
	fi
	samtools index $(DATA)/$(NAME)_$(SRR).sorted.bam

# Step 3: Generate stats
	samtools flagstat $(DATA)/$(NAME)_$(SRR).sorted.bam > $(DATA)/$(NAME)_$(SRR).stats.txt
	samtools coverage $(DATA)/$(NAME)_$(SRR).sorted.bam >> $(DATA)/$(NAME)_$(SRR).stats.txt

# Step 4: Convert BAM → BigWig (safe + make-compatible)
	@( \
		if [ -s $(DATA)/$(NAME)_$(SRR).sorted.bam ]; then \
			echo "Generating coverage track for $(NAME)_$(SRR)..."; \
			LC_ALL=C bedtools genomecov -ibam $(DATA)/$(NAME)_$(SRR).sorted.bam -split -bg 2>/dev/null | \
				sort -k1,1 -k2,2n > $(DATA)/$(NAME)_$(SRR).bedgraph || true; \
			if [ -s $(DATA)/$(NAME)_$(SRR).bedgraph ]; then \
				bedGraphToBigWig $(DATA)/$(NAME)_$(SRR).bedgraph $(GENOME).fai $(DATA)/$(NAME)_$(SRR).bw || true; \
			else \
				echo "No coverage data for $(NAME)_$(SRR), skipping BigWig conversion."; \
			fi; \
		else \
			echo "Empty BAM file for $(NAME)_$(SRR), skipping coverage generation."; \
		fi \
	) || true

# ------------------------------------------------------------
# Convert all BedGraph files to BigWig (manual regeneration)
# ------------------------------------------------------------
bigwig: $(GENOME).fai
	@echo "Converting all BedGraph files to BigWig..."
	@for f in $(DATA)/*.bedgraph; do \
		base=$$(basename $$f .bedgraph); \
		if [ -s "$$f" ]; then \
			echo "$$base.bedgraph → $$base.bw"; \
			bedGraphToBigWig "$$f" $(GENOME).fai "$(DATA)/$$base.bw" || echo "⚠️ Failed on $$base"; \
		else \
			echo "Skipping empty $$f"; \
		fi; \
	done

# ------------------------------------------------------------
# Cleanup
# ------------------------------------------------------------
clean:
	rm -rf $(DATA)
	@echo "🧹 Cleaned all generated data."
