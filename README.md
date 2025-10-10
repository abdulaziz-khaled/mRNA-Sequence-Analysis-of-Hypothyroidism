# Executive Data Analysis Summary
I analyzed a total of 4 Human samples [Study id- HRA001684](https://www.nature.com/articles/s41467-022-28120-2#data-availability). Samples and Grouping information are provided in Table 1 and Table 2. The raw reads were filtered using Cutadapt for quality scores and adapters. Filtered reads were aligned to the Human genome (GRCh38.primary_assembly.genome.) using splice aware aligner STAR to quantify reads mapped to each gene. Percentage of uniquely aligned reads ranged between 83% – 89% across all the samples. The aligned data quality check was performed using Qualimap. rRNA contamination was screened using RSeqQC package. Total number of uniquely mapped reads were counted using FeatureCounts. The uniquely mapped reads were then subjected to differential gene expression using DESeq2 in R. Total number of differentially expressed genes are given in the Table 5 for the respective comparisons.

# Methods – Data Analysis
The following bioinformatics steps were performed for analysis of the data
![image alt](https://github.com/abdulaziz-khaled/mRNA-Sequence-Analysis-of-Hypothyroidism/blob/fa2e71426d05b1a0a0b164630077f35975712851/photo_2025-10-10_14-50-56.jpg)

# Bioinformatic Steps
**1.Read quality check:** The following parameters from raw fastq files were checked using `fastqc tool` (version – 0.11.9) as part of quality check- Base quality score distribution,Sequence quality score distribution, Average base content per read, GC distribution in the reads, PCR amplification issue, Check for over-represented sequences and Adapter trimming. Based on the quality report of fastq files trimming on raw read was performed to only retain high quality sequence for further analysis. In addition, the low-quality sequence reads were excluded from the analysis. The adapter trimming was performed using `Cutadapt` (Version 4.9).

**2.Read alignment:** The paired-end reads are aligned to the reference Human genome (GRCh38.primary_assembly.genome). Alignment of reads was performed using `STAR` (version - 2.7.11b)

**3.Alignment quality screening:** Aligned reads were analyzed for their quality check related statistics; against both mapping quality as well as read alignment distribution against reference transcriptome features using `Qualimap`. Factors like read distribution, Alignment distribution as well as splice junction distribution were analyzed separately. rRNA contamination was screened using `rSeqQC` package.

**4.Expression estimation:** The aligned reads were used for estimating expression of the genes. The raw read counts were estimated using `FeatureCount` (version - 2.0.8) across all samples The raw read count across all samples were then normalized using `DESeq2` package in `R`.

**5.Differential expression analysis:** The raw read count data were normalized using `DESeq2`. The ratio of normalized read counts for HT over Non-HT was taken as the fold change. Genes were first filtered based on the p-value (<= 0.05) for statistical significance. Those genes which were found to have log2(foldchange) <= -1 and log2(foldchange) >= 1 were considered as downregulated or upregulated, respectively.

**6.Functional Enrichment Analysis:** Functional over-representation analysis was performed for HT and Non-HT comparision separately for upregulated and downregulated genes using `clusterprofiler` R package ontologies such as Biological Process(BP), Molecular Function(MF),Cellular Component(CC), `Kegg` Pathways were analysed.

# Samples and Grouping information
Following samples were analyzed.
### Table 1. Samples Information

| Sl.no | Sample ID | Sample Type |
| :---: | :---------: | :-----------: |
| 1 | HRR568844 | HT |
| 2 | HRR568887 | Non-HT |
| 3 | HRR568888 | Non-HT |
| 4 | HRR568894 | HT |

---

### Table 2. Grouping information

| Comparision | Grouping Type |
| :---------: | :-------------: |
| 1. | HT vs Non-HT |

# Results 

**1. Data QC Summary:** Table 3 summarizes the overall data generated and the average read quality
observed across the reads.

---
### Table 3. Raw Data QC Summary

| Sample Name | Sample Type | Total paired reads (Before adapter trimming) | Data generated (Gb) | Total paired reads (After adapter trimming) | Percentage of reads remaining after trimming (%) | Avg base quality (phred) after trimming (%) | Total (reads) >=Q30% after trimming (%) | GC% after trimming (%) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| HRR568844 | HT | 23772792 | 7.13 | 23553623 | 99.0 | 38.9 | 99.2 | 47 |
| HRR568887 | Non-HT | 8438394 | 2.5 | 7689624 | 91.1 | 36.5 | 97.7 | 47 |
| HRR568888 | Non-HT | 7535140 | 2.2 | 6890927 | 91.4 | 36.6 | 97.7 | 50 |
| HRR568894 | HT | 8186467 | 2.4 | 7497321 | 91.5 | 36.4 | 97.7 | 50.5 |

Data generation for each sample was in the range of 2 GB to 7 GB and the
average % of reads with quality > phred score 30 is~98%.

**2. Alignment Summary:** Samples were aligned to Human reference genome. The mapping statistics
showed the alignment of uniquely mapped reads were at the range of 87-
89%. Unaligned read were at the range of 3-7% and multimapped reads
~7.5%. The below Table 4 summarizes the alignment summary of the
sequenced reads.

---
### Table 4. Read Alignment Summary

| Sample id | Sample Name | Paired reads after Adapter Trimming | Uniquely mapped reads % | Multimapped reads % | Reads unaligned % | Exonic % | Intronic % | Intergenic % | Mean mapping quality | Mean coverage | Rseqc contamination check | 5'-3'bias | Known splicing junction | Partly known splicing junction | Novel splicing junction |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| HRR568844 | HT | 23553623 | 87.9 | 5.4 | 7.17 | 70.1 | 26.98 | 2.91 | 30.98 | 5.69 | 3.8 | 1.48 | 76 | 3.2 | 20.1 |
| HRR568887 | Non-HT | 7689624 | 83.9 | 9.3 | 7.28 | 64.8 | 32.3 | 2.8 | 33.33 | 2.29 | 11.9 | 1.36 | 72 | 11.61 | 15.95 |
| HRR568888 | Non-HT | 6890927 | 88.4 | 7.7 | 3.3 | 64 | 33.2 | 2.8 | 30.99 | 2.30 | 11.3 | 1.38 | 71 | 12.32 | 16.64 |
| HRR568894 | HT | 7497321 | 89.3 | 7.8 | 4.26 | 73.2 | 24.7 | 2.1 | 26.08 | 3.09 | 11.2 | 1.37 | 71 | 11.77 | 16.24 |

**3. Principal Component Analysis (PCA):** Principal Component Analysis (PCA) is a statistical technique used to identify global patterns in high-dimensional datasets. It is commonly used to explore the similarity of biological samples in RNA-seq datasets. To achieve this, gene expression values are transformed into Principal Components (PCs), a set of linearly uncorrelated features which represent the most relevant sources of variance in the data, and subsequently visualized using a scatter plot. The scatter plot of the first two principal components (pcs) of the data for given samples are shown in [Figure 2. Each point represents the sample](https://github.com/abdulaziz-khaled/mRNA-Sequence-Analysis-of-Hypothyroidism/blob/main/3.Normalization%20and%20Sample%20Clustering/Protein%20Coding%20Genes/PCA.pdf). Samples with similar gene expression profiles should come closer in the two-dimensional space.
