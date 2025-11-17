#
# GIAB HG008 – Multi-Platform Region Extraction (Remote BAM)
# Yue Shi – Week 11
#

# ------------------------------------------------------------
# Reference Genome (GRCh38-GIABv3)
# ------------------------------------------------------------
REF_URL = https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/references/GRCh38/GRCh38_GIABv3_no_alt_analysis_set_maskedGRC_decoys_MAP2K3_KMT2C_KCNJ18.fasta.gz
REF = refs/GRCh38_GIABv3.fasta.gz

# Region of interest (KRAS)
REGION = chr12:25,205,246-25,250,936


# ------------------------------------------------------------
# GFF Annotation (NCBI GRCh38)
# ------------------------------------------------------------
GFF_URL = https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/001/405/GCF_000001405.40_GRCh38.p14/GCF_000001405.40_GRCh38.p14_genomic.gff.gz
GFF = refs/GRCh38.gff.gz


# ------------------------------------------------------------
# Platform BAM URLs (remote BAMs) – GRCh38-GIABv3
# ------------------------------------------------------------

# Element Aviti (Normal–Duodenum)
AVITI_URL = https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data_somatic/HG008/Liss_lab/Element-AVITI-20241216/HG008-N-D_Element-StdInsert_77x_GRCh38-GIABv3.bam
AVITI_REGION = bam/Aviti_region.bam

# Illumina NovaSeq (Normal–118×)
ILLUMINA_URL = https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data_somatic/HG008/Liss_lab/NYGC_Illumina-WGS_20231023/HG008-N-D_Illumina_118x_GRCh38-GIABv3.bam
ILLUMINA_REGION = bam/Illumina_region.bam

# Nanopore ONT (Normal–Pancreas, 41×)
ONT_URL = https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data_somatic/HG008/Liss_lab/Northeastern_ONT-std_20240422/HG008-N-P_GRCh38-GIABv3_ONT-R1041-dorado_0.5.3_5mC_5hmC_41x.bam
ONT_REGION = bam/ONT_region.bam


# ------------------------------------------------------------
# Make settings
# ------------------------------------------------------------
SHELL = bash
.ONESHELL:
.DELETE_ON_ERROR:
MAKEFLAGS += --warn-undefined-variables
MAKEFLAGS += --no-builtin-rules


# ------------------------------------------------------------
# Help
# ------------------------------------------------------------
usage:
	@echo "# GIAB HG008 – Multi-Platform Region Extraction (Remote BAM)"
	@echo "# make ref         # download reference genome"
	@echo "# make gff         # download annotation"
	@echo "# make aviti       # extract region from Aviti (remote)"
	@echo "# make illumina    # extract region from Illumina (remote)"
	@echo "# make ont         # extract region from ONT (remote)"
	@echo "# make stats       # show stats for all extracted BAMs"
	@echo "# make all         # run everything"
	@echo "# make clean       # remove all outputs"


# ------------------------------------------------------------
# Reference genome
# ------------------------------------------------------------
$(REF):
	mkdir -p refs
	curl -L $(REF_URL) > $(REF)
	samtools faidx $(REF)
	gunzip -c $(REF) > refs/GRCh38_GIABv3.fasta
	samtools faidx refs/GRCh38_GIABv3.fasta

ref: $(REF)


# ------------------------------------------------------------
# GFF annotation
# ------------------------------------------------------------
$(GFF):
	mkdir -p refs
	curl -L $(GFF_URL) | gunzip -c > $(GFF)

gff: $(GFF)


# ------------------------------------------------------------
# Region extraction (remote BAMs)
# ------------------------------------------------------------

# ---- Aviti ----
aviti: $(REF)
	mkdir -p bam
	samtools view -b $(AVITI_URL) $(REGION) > $(AVITI_REGION)
	samtools index $(AVITI_REGION)
	samtools flagstat $(AVITI_REGION)

# ---- Illumina ----
illumina: $(REF)
	mkdir -p bam
	samtools view -b $(ILLUMINA_URL) $(REGION) > $(ILLUMINA_REGION)
	samtools index $(ILLUMINA_REGION)
	samtools flagstat $(ILLUMINA_REGION)

# ---- Nanopore ----
ont: $(REF)
	mkdir -p bam
	samtools view -b $(ONT_URL) $(REGION) > $(ONT_REGION)
	samtools index $(ONT_REGION)
	samtools flagstat $(ONT_REGION)



# ------------------------------------------------------------
# Run everything
# ------------------------------------------------------------
all: ref gff aviti illumina ont stats


# ------------------------------------------------------------
# Clean
# ------------------------------------------------------------
clean:
	rm -rf refs bam
	@echo "# Workspace cleaned."

.PHONY: usage ref gff aviti illumina ont stats all clean
