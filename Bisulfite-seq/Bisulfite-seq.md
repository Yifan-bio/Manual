# Bisulfite manual

* [Introduction to Bisulfite sequencing](#introduction-to-bisulfite-sequencing)
    + [Background](#background)
    + [Principle](#principle)
    + [Experimental Design](#experimental-design)
* [Pipelines for Bisulfite sequencing](#pipelines-for-bisulfite-sequencing)
    + [Trimming](#trimming)
    + [Alignment](#alignment)
    + [Post-alignment Filtering](#post-alignment-filtering)
    + [DNA methylation calling](#dna-methylation-calling)
    + [Differential methylation](#differential-methylation)
    + [Methylation Segmentation](#methylation-segmentation)
* [Reference](#reference)

> Last Update: 04 June 2024

# Introduction to Bisulfite sequencing

## Background

DNA methylation is a chemical modification where a methyl group is added to cytosine, one of the DNA bases. This usually happens at CpG sites, where a cytosine is followed by a guanine. Methylation can turn off gene expression by changing the structure of chromatin (the complex of DNA and proteins) or by blocking the binding of transcription factors that turn genes on. Studying methylation patterns helps researchers understand how genes are regulated, how cells differentiate, and how diseases like cancer develop.

Bisulfite sequencing (BS-seq) is a technique used to map DNA methylation. It involves treating DNA with bisulfite, which changes unmethylated cytosines to uracil, while leaving methylated cytosines unchanged. After sequencing, uracil is read as thymine, allowing researchers to distinguish between methylated and unmethylated cytosines.

Next-generation sequencing allows for a detailed, genome-wide analysis of methylation at a single-nucleotide level. This makes it possible to choose between different methods based on cost and coverage needs. BS-seq provides single-base resolution of methylated cytosines by treating genomic DNA with sodium bisulfite and then sequencing it. After bisulfite treatment, unmethylated cytosines become uracils (read as thymines during sequencing), while methylated cytosines remain as cytosines. By comparing treated and untreated sequences, the locations of methylated cytosines can be identified. However, bisulfite treatment reduces sequence complexity because unmethylated cytosines are converted to thymines.

---

## Principle

Bisulfite sequencing includes various methods tailored to different research needs. The most commonly applied methods are:

1. **Whole Genome Bisulfite Sequencing (WGBS)**: This method sequences the entire genome, providing comprehensive methylation data at single-base resolution.
2. **Reduced Representation Bisulfite Sequencing (RRBS)**: This method sequences a fraction of the genome, focusing on CpG-rich regions. It reduces cost and data complexity while still providing high-resolution methylation information.
3. **Targeted Bisulfite Sequencing**: This method focuses on specific genomic regions of interest, offering a cost-effective approach for detailed methylation analysis in those regions.

Key Steps in BS-seq

1. **Bisulfite Treatment**
   - DNA is treated with sodium bisulfite. This treatment converts unmethylated cytosines to uracil, while methylated cytosines remain unchanged. This conversion allows for the differentiation between methylated and unmethylated cytosines during sequencing.

2. **PCR Amplification**
   - The bisulfite-treated DNA is amplified using PCR. During this process, uracil residues are converted to thymine, preparing the DNA for sequencing. Care is taken to minimize PCR cycles to reduce amplification bias.

3. **Sequencing**
   - The amplified DNA is then sequenced using high-throughput sequencing technologies. Paired-end sequencing is often preferred for better alignment and comprehensive coverage.

4. **Data Analysis**
   - The sequencing reads are aligned to a bisulfite-converted reference genome. Bioinformatics tools such as Bismark or BSmap are used to map the reads and extract methylation information.
   - The methylation status of each cytosine is determined by comparing the treated and untreated sequences. Unmethylated cytosines, converted to thymine during the bisulfite treatment, are distinguished from methylated cytosines, which remain as cytosine.
   - Additional analysis can include identifying differentially methylated regions (DMRs) and understanding the biological significance of methylation patterns in gene regulation and disease.

By following these steps, researchers can generate detailed methylation maps, providing insights into epigenetic regulation and its implications in health and disease.

---

## Experimental Design

Before you begin with the study, please make sure the use of BS seq is the appropriate approach for your study. Please go through these papers to see if the choose of BS-seq is suitable for the study. In cases, BS conversion isnt the only protocol that has been devleoped in nowaday allowing the differentioation to take place.

---

# Pipelines for Bisulfite sequencing

> 

## Trimming


## Alignment



## Post-alignment Filtering



## DNA methylation calling


## Differential methylation


## Methylation Segmentation


# Reference
