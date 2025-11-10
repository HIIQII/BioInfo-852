#
# Zika Virus Variant Calling Workflow
# Yue Shi – Week 11
#

# ------------------------------------------------------------
# Reference and annotation
# ------------------------------------------------------------
ACC = GCF_000882815.3
DATA = data
THREADS = 4
REF = $(DATA)/Zikagenome.fa
GFF = $(DATA)/Zikagenome.gff
INDEX_PREFIX = $(DATA)/ZikaIndex

# ------------------------------------------------------------
# One sample example (for single-sample run)
# ------------------------------------------------------------
SRR = SRR3191542
SAMPLE = Zika_SRR3191542
LAYOUT = SINGLE
N = 100000

# ------------------------------------------------------------
# Derived paths
# ------------------------------------------------------------
R1 = $(DATA)/${SRR}_1.fastq
R2 = $(DATA)/${SRR}_2.fastq
BAM = $(DATA)/${SAMPLE}.sorted.bam
VCF = $(DATA)/${SAMPLE}.vcf.gz

# ------------------------------------------------------------
# Make settings
# ------------------------------------------------------------
SHELL = bash
.ONESHELL:
.SHELLFLAGS = -eu -o pipefail -c
.DELETE_ON_ERROR:
MAKEFLAGS += --warn-undefined-variables
MAKEFLAGS += --no-builtin-rules

# ------------------------------------------------------------
# Help / usage
# ------------------------------------------------------------
usage:
	@echo "#"
	@echo "# Zika Virus SNP calling pipeline"
	@echo "#"
	@echo "# ACC = ${ACC}"
	@echo "# SRR = ${SRR}"
	@echo "# SAMPLE = ${SAMPLE}"
	@echo "# REF = ${REF}"
	@echo "# GFF = ${GFF}"
	@echo "# BAM = ${BAM}"
	@echo "# VCF = ${VCF}"
	@echo "#"
	@echo "# make bam          # Run download, alignment, stats, coverage"
	@echo "# make vcf          # Call SNPs and produce VCF file"
	@echo "# make variants_all # Call variants for all samples (from design.csv)"
	@echo "# make merge_vcf    # Merge all per-sample VCFs"
	@echo "# make vep          # Analyze variants with Ensembl VEP"
	@echo "# make all          # Run everything end-to-end"
	@echo "# make clean        # Remove all generated data"
	@echo "#"

# ------------------------------------------------------------
# Download genome + annotation
# ------------------------------------------------------------
GENOME_URL = https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/882/815/GCF_000882815.3_ViralProj36615/GCF_000882815.3_ViralProj36615_genomic.fna.gz
GFF_URL = https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/882/815/GCF_000882815.3_ViralProj36615/GCF_000882815.3_ViralProj36615_genomic.gff.gz

$(REF):
	mkdir -p $(DATA)
	curl -L $(GENOME_URL) | gunzip -c > $(REF)
	samtools faidx $(REF)

$(GFF):
	mkdir -p $(DATA)
	curl -L $(GFF_URL) | gunzip -c > $(GFF)
	
# ------------------------------------------------------------
# BAM generation
# ------------------------------------------------------------
bam: $(REF) $(GFF)
	@echo "# Step 1: Build BWA index"
	bwa index -p $(INDEX_PREFIX) $(REF)

	@echo "# Step 2: Download reads"
	if [ "$(LAYOUT)" = "PAIRED" ]; then \
		fastq-dump -X $(N) --split-files -O $(DATA) $(SRR); \
	else \
		fastq-dump -X $(N) -O $(DATA) $(SRR); \
	fi

	@echo "# Step 3: Align reads"
	if [ "$(LAYOUT)" = "PAIRED" ]; then \
		bwa mem -t $(THREADS) $(INDEX_PREFIX) $(R1) $(R2) | samtools sort -o $(BAM); \
	else \
		bwa mem -t $(THREADS) $(INDEX_PREFIX) $(DATA)/$(SRR).fastq | samtools sort -o $(BAM); \
	fi
	samtools index $(BAM)

	@echo "# Step 4: Alignment stats and coverage"
	samtools flagstat $(BAM) | tee $(BAM:.bam=.stats.txt)
	samtools coverage $(BAM) | tee -a $(BAM:.bam=.stats.txt)

	@echo "# Step 5: Coverage track (bedGraph → BigWig)"
	LC_ALL=C bedtools genomecov -ibam $(BAM) -split -bg | sort -k1,1 -k2,2n > $(BAM:.bam=.bedgraph) || true
	if [ -s "$(BAM:.bam=.bedgraph)" ]; then \
		bedGraphToBigWig $(BAM:.bam=.bedgraph) $(REF).fai $(BAM:.bam=.bw) || true; \
	else \
		echo "No coverage data for $(SAMPLE), skipping BigWig conversion."; \
	fi

# ------------------------------------------------------------
# Call variants (single-sample)
# ------------------------------------------------------------
vcf: $(BAM)
	@echo "# Step 6: Calling variants for $(SAMPLE)"
	bcftools mpileup -f $(REF) -q 20 -Q 20 -Ou $(BAM) \
	| bcftools call --ploidy 1 -mv -Oz -o $(VCF)
	tabix -p vcf $(VCF)
	@echo "# VCF generated: $(VCF)"

# ------------------------------------------------------------
# Call variants for all samples listed in design.csv
# ------------------------------------------------------------
variants_all:
	@echo "# Calling variants for all samples in design.csv..."
	@cat design.csv | tail -n +2 | parallel -j 4 --colsep ',' \
		'make vcf SRR={2} SAMPLE={1}_{2} LAYOUT={3}'

# ------------------------------------------------------------
# Merge all VCFs into a multisample VCF
# ------------------------------------------------------------
merge_vcf:
	@echo "# Step 7: Merging all per-sample VCFs..."
	@ls data/*.vcf.gz > data/vcf.list
	bcftools merge -m none -Oz -o data/multisample.vcf.gz -l data/vcf.list
	tabix -p vcf data/multisample.vcf.gz
	@echo "# Multisample VCF created: data/multisample.vcf.gz"

# ------------------------------------------------------------
# VEP annotation
# ------------------------------------------------------------
${GFF}.gz: 
	# Sort and compress the GFF file
	cat ${GFF} | sort -k1,1 -k4,4n -k5,5n -t$$'\t' | bgzip -c > ${GFF}.gz
	# Index the GFF file
	tabix -p gff ${GFF}.gz

vep: ${GFF}.gz
	mkdir -p results
	micromamba run -n vep \
		~/src/ensembl-vep/vep \
		-i ${VCF} \
		-o results/vep.txt \
		-gff ${GFF}.gz \
		-fasta ${REF} \
		--force_overwrite

	# Show the resulting files
	ls -lh results/*

# ------------------------------------------------------------
# Run all steps
# ------------------------------------------------------------
all: bam vcf vep

# ------------------------------------------------------------
# Cleanup
# ------------------------------------------------------------
clean:
	rm -rf $(DATA) results
	@echo "# Workspace cleaned."

.PHONY: usage bam vcf all clean variants_all merge_vcf vep