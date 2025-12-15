############################################################
# Variables
############################################################

REF         = refs/chr22.genome.fa
GTF         = refs/chr22.gtf

# hisat2 index prefix (will generate refs/chr22_index.*.ht2)
IDX_PREFIX  = refs/chr22_index

SAMPLES     = HBR_1 HBR_2 HBR_3 UHR_1 UHR_2 UHR_3

BAM_DIR     = bam
BAMS        = $(addprefix $(BAM_DIR)/,$(addsuffix .bam,$(SAMPLES)))

COUNT_FILE  = counts.txt


############################################################
# Phony targets
############################################################

.PHONY: all index align counts clean

# Full pipeline: build index, align all samples, generate counts
all: index align counts

# Build the hisat2 index and fasta index
index: $(IDX_PREFIX).1.ht2 $(REF).fai

# Align all samples to BAM (and make BigWig)
align: $(BAMS)

# Generate count matrix
counts: $(COUNT_FILE)


############################################################
# Index rules
############################################################

# hisat2 index (refs/chr22_index.1.ht2 etc.)
$(IDX_PREFIX).1.ht2: $(REF)
	hisat2-build $< $(IDX_PREFIX)

# samtools fai index for genome
$(REF).fai: $(REF)
	samtools faidx $<


############################################################
# Alignment + coverage rules
############################################################

# Pattern rule: one BAM per sample.
# Input:  reads/<sample>_R1.fq
# Output: bam/<sample>.bam and bam/<sample>.bw
$(BAM_DIR)/%.bam: reads/%_R1.fq index
	mkdir -p $(BAM_DIR)
	# Align with hisat2 (single-end), write SAM to temp
	hisat2 -x $(IDX_PREFIX) -U $< -S $*.sam
	# Sort SAM to BAM
	samtools sort -o $@ $*.sam
	# Index BAM
	samtools index $@
	# Make bedGraph and BigWig coverage
	bedtools genomecov -ibam $@ -split -bg | sort -k1,1 -k2,2n > $(BAM_DIR)/$*.bedgraph
	bedGraphToBigWig $(BAM_DIR)/$*.bedgraph $(REF).fai $(BAM_DIR)/$*.bw
	rm -f $*.sam $(BAM_DIR)/$*.bedgraph


############################################################
# FeatureCounts rule
############################################################

$(COUNT_FILE): $(BAMS)
	featureCounts -a $(GTF) -o $(COUNT_FILE) $^


############################################################
# Cleanup
############################################################

clean:
	rm -rf $(BAM_DIR) $(COUNT_FILE) $(REF).fai $(IDX_PREFIX).*ht2
