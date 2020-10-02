# Enrichment-Analysis
R codes I am using for getting from RNA-seq raw count to Pathways



Assume we performed an RNA-seq (or microarray gene expression) experiment and now want to know what pathway/biological process shows enrichment for our [differentially expressed] genes. Indeed, there are several terminologies someone should be familiar with before diving into the topic thoroughly: 

**Gene set**

A gene set is an unordered collection of genes that are functional related. 

**Pathway**

A pathway can be interpreted as a gene set by ignoring functional relationships among genes.

**Gene Ontology (GO)**

GO describe gene function. A gene role/function could be attributed to the three main classes: 

*•	Molecular Function (MF)* : which define molecular activities of gene products.

*•	Cellular Component (CC)* : which describe where gene products are active/localized.

*•	Biological Process (BP)* : which describe pathways and processes that the gene product is active in.

**Kyoto Encyclopedia of Genes and Genomes (KEGG)**

KEGG is a collection of manually curated pathway maps representing molecular interaction and reaction networks. 



There are different methods widely used for functional enrichment analysis: 

**1-	Over Representation Analysis (ORA):**

This is the simplest version of enrichment analysis and at the same time the most widely used approach. The concept in this approach is based on a Fisher exact test p value in a contingency table. For example, supposed we come up with 160 differentially expressed genes from a microarray expression experiment which was able to investigate 17,000 gene expression levels. We found that 30 genes from our findings are somehow member of a pathway which has 50 gene members (call it pathway X). To find enrichment we can perform a Fisher exact test on the following contingency table:

|                 | not.intrested.genes   |intrested.genes |
| :-------------- |:---------------------:|---------------:|
| in pathwayX     | 20                    | 30             |
| not.in pathwayX | 16820                 | 130            |

There are relatively large number of web-tools R package for ORA. Personally I am a fan of [DAVID](https://david.ncifcrf.gov/home.jsp) webtools however its lat update was in 2016 (DAVID 6.8 Oct. 2016).  

**2-	Gene Set Enrichment Analysis (GSEA):**

It was developed by Broad Institute. This is the preferred method when genes are coming from an expression experiment like microarray and RNA-seq. However, the original methodology was designed to work on microarray but later modification made it suitable for RNA-seq also. In this approach, you need to rank your genes based on a statistic (like what DESeq2 provide), and then perform enrichment analysis against different pathways (= gene set). You have to download the gene set files into you local system. The point is that here the algorithm will use all genes in the ranked list for enrichment analysis. [in contrast to ORA where only genes passed a specific threshold (like DE ones) would be used for enrichment analysis]. You can find more details about the methodology on the original [PNAS paper](https://www.pnas.org/content/102/43/15545.abstract), here is a summary of why one should use this approach instead of ORA:

1- After correcting for multiple hypotheses testing, no individual gene may meet the threshold for statistical significance.

2- On the other hand, one may be left with a long list of statistically significant genes without any unifying biological theme.

3- Cellular processes often affect sets of genes acting in concert, using ORA may lead to miss important effects on pathways.

GSEA software maybe finds on its [homepage](https://www.gsea-msigdb.org/gsea/index.jsp). However, there are some Bioconductor packages which use a similar approach to do GSEA, I like to use this one : [fgsea](https://bioconductor.org/packages/release/bioc/html/fgsea.html). Also there are some R package which can do ROA and GSEA for you like [clusterProfiler](https://bioconductor.org/packages/release/bioc/html/clusterProfiler.html)

