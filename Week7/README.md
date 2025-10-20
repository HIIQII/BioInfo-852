Week 7 Assignment

## 
Two datasets are analyzed:
- **SRR3194431** — Illumina NextSeq 500 (short, high-depth reads)
- **SRR3191545** — Illumina MiSeq (longer, lower-depth reads)

##
Command to run the first datasets(NextSeq 500)

```bash
make all SRR=SRR3194431
```

##
Command to run the second datasets(Illumina MiSeq )

```bash
make all SRR=SRR3191545
```
##
The Makefile performs:  
1. Genome download and indexing  
2. SRA read download  
3. Alignment using BWA + Samtools  
4. Coverage and mapping statistics  
5. BigWig coverage track generation 


### Q1: Differences between the two alignments  

Both datasets align to the same Zika virus reference genome.  
The MiSeq dataset (**SRR3191545**) shows **smoother and denser coverage**, while the NextSeq dataset (**SRR3194431**) has more gaps and uneven regions.  
This reflects platform differences: MiSeq reads are **longer (150–300 bp)** and **paired-end**, so they better resolve repetitive regions and produce more uniform coverage.

---

### Q2: Comparison of statistics for the two BAM files  

| Metric               | SRR3194431 (NextSeq 500) | SRR3191545 (MiSeq) |
| :------------------- | :----------------------- | :----------------- |
| Total Reads          | 100 000                  | 200 001            |
| Mapped Reads         | 273 (0.27 %)             | 473 (0.24 %)       |
| Properly Paired      | 0                        | 468 (0.23 %)       |
| Genome Coverage      | 82.43 %                  | 93.82 %            |
| Mean Depth           | 1.89×                    | 3.29×              |
| Mean Base Quality    | 33.1                     | 37.2               |
| Mean Mapping Quality | 59.8                     | 60.0               |


**Summary:**  
The MiSeq dataset (SRR3191545) shows higher genome coverage and mean depth due to its paired-end, longer reads, which produce smoother and more complete alignments compared to the single-end NextSeq dataset (SRR3194431).

---

### Q3: How many primary alignments does each BAM file contain?  

From the `samtools flagstat` results:  
- **SRR3194431:** 100 000 primary alignments  
- **SRR3191545:** 200 000 primary alignments  

These values match the total number of reads in each subset used for testing.

---

### Q4: Coordinate with the Largest Observed Coverage
```bash
samtools depth data/SRR3194431.sorted.bam | sort -k3,3nr | head
samtools depth data/SRR3191545.sorted.bam | sort -k3,3nr | head
```
| Dataset                      | Highest Coverage | Coordinate (NC_012532.1)  | Depth                        |
| :--------------------------- | :--------------- | :------------------------ | :--------------------------- |
| **SRR3194431 (NextSeq 500)** | Peak 9×          | ~position **9592 bp**     | within 3′ end of polyprotein |
| **SRR3191545 (MiSeq)**       | Peak 11×         | ~positions **754–765 bp** | near 5′ end of polyprotein   |

### Q5: Select a gene of interest. How many alignments on the forward strand cover the gene?
The Zika virus genome encodes a single polyprotein gene (NC_012532.1:1–10794) that is processed into all viral proteins.
```bash
samtools view data/SRR3194431.sorted.bam NC_012532.1:1-10794 | grep -v "XS:A:-" | wc -l
samtools view data/SRR3191545.sorted.bam NC_012532.1:1-10794 | grep -v "XS:A:-" | wc -l
```
| Dataset                      | Forward-Strand Alignments | Total Mapped Reads | % Forward |
| :--------------------------- | :------------------------ | :----------------- | :-------- |
| **SRR3194431 (NextSeq 500)** | 273                       | 273                | 100 %     |
| **SRR3191545 (MiSeq)**       | 475                       | 473                | 100 %     |


### IGV visualization
