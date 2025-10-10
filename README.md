# Executive Data Analysis Summary
I analyzed a total of 4 Human samples [Study id- HRA001684](https://www.nature.com/articles/s41467-022-28120-2#data-
availability). Samples and Grouping information are provided in Table 1 and
Table 2. The raw reads were filtered using Cutadapt for quality scores and
adapters. Filtered reads were aligned to the Human genome (GRCh38.
primary_assembly.genome.) using splice aware aligner STAR to quantify
reads mapped to each gene. Percentage of uniquely aligned reads ranged
between 83% – 89% across all the samples. The aligned data quality check
was performed using Qualimap. rRNA contamination was screened using
RSeqQC package. Total number of uniquely mapped reads were counted
using FeatureCounts. The uniquely mapped reads were then subjected to
differential gene expression using DESeq2 in R. Total number of
differentially expressed genes are given in the Table 5 for the respective
comparisons.

