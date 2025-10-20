# ------------------------------------------------------------
# Makefile for Zika Virus Genome Alignment Project
# Author: Yue Shi
# ------------------------------------------------------------
# This Makefile automates:
#   1. Genome download
#   2. Read download (SRA)
#   3. Genome indexing
#   4. Alignment (BWA + Samtools)
#   5. Coverage statistics
# ------------------------------------------------------------

# === Variables ===
GENOME_URL = https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/882/815/GCF_000882815.3_ViralProj36615/GCF_000882815.3_ViralProj36615_genomic.fna.gz
SRA = SRR3194431
THREADS = 4
DATA = data
GENOME = $(DATA)/Zikagenome.fa
INDEX_PREFIX = $(DATA)/ZikaIndex
READS_1 = $(DATA)/$(SRA)_1.fastq
READS_2 = $(DATA)/$(SRA)_2.fastq
BAM = $(DATA)/$(SRA).sorted.bam
STATS = $(DATA)/$(SRA).stats.txt
BG = $(DATA)/$(SRA).bedgraph
BW = $(DATA)/$(SRA).bw

# ------------------------------------------------------------
# Main target (run everything)
# ------------------------------------------------------------
all: index align stats

# ------------------------------------------------------------
# 1. Download the Zika reference genome
# ------------------------------------------------------------
$(GENOME):
	mkdir -p $(DATA)
	curl -L $(GENOME_URL) | gunzip -c > $(GENOME)
	
# ------------------------------------------------------------
# 1b. Download the Zika genome annotation (GFF3)
# ------------------------------------------------------------
GFF_URL = https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/882/815/GCF_000882815.3_ViralProj36615/GCF_000882815.3_ViralProj36615_genomic.gff.gz
GFF = $(DATA)/Zikagenome.gff

$(GFF):
	mkdir -p $(DATA)
	curl -L $(GFF_URL) | gunzip -c > $(GFF)

# ------------------------------------------------------------
# 2. Download SRA reads (⚠️ NOTE: this step can be very slow)
#    - The full dataset SRR3194431 is >5 GB
#    - To test faster, you can uncomment the line with "-X 100000"
# ------------------------------------------------------------
$(READS_1) $(READS_2):
	mkdir -p $(DATA)
	# ⚠️ This full download may hang if network is slow or sra-tools misconfigured
	#fastq-dump --split-files --outdir $(DATA) $(SRA)
	# 🧪 For testing only (small subset ~100k reads, finishes fast):
	fastq-dump -X 100000 --split-files --outdir $(DATA) $(SRA)

# ------------------------------------------------------------
# 3. Index the genome (creates .amb, .ann, .bwt, .pac, .sa files)
# ------------------------------------------------------------
index: $(GENOME)
	bwa index -p $(INDEX_PREFIX) $(GENOME)

# ------------------------------------------------------------
# 4. Align reads to the genome and sort
# ------------------------------------------------------------
align: index $(READS_1)
	@if [ -f "$(READS_2)" ]; then \
		echo "🧬 Paired-end detected for $(SRA)"; \
		bwa mem -t $(THREADS) $(INDEX_PREFIX) $(READS_1) $(READS_2) | samtools sort -o $(BAM); \
	else \
		echo "🧬 Single-end detected for $(SRA)"; \
		bwa mem -t $(THREADS) $(INDEX_PREFIX) $(READS_1) | samtools sort -o $(BAM); \
	fi
	samtools index $(BAM)


# ------------------------------------------------------------
# 5. Generate alignment and coverage statistics
# ------------------------------------------------------------
stats: $(BAM)
	samtools flagstat $(BAM) > $(STATS)
	samtools coverage $(BAM) >> $(STATS)
	cat $(STATS)

# ------------------------------------------------------------
# 6. Convert BAM → BigWig
# ------------------------------------------------------------
bigwig: $(BAM)
	@echo "📈 Generating BigWig coverage file..."
	samtools faidx $(GENOME)
	LC_ALL=C; bedtools genomecov -ibam $(BAM) -split -bg | sort -k1,1 -k2,2n > $(BG)
	bedGraphToBigWig $(BG) $(GENOME).fai $(BW)
	@echo "✅ BigWig file created: $(BW)"

# ------------------------------------------------------------
# 7. Clean workspace (remove generated data)
# ------------------------------------------------------------
clean:
	rm -rf $(DATA) reads refs bam
	@echo "🧹 Cleaned up all data files."

.PHONY: all index align stats bigwig clean