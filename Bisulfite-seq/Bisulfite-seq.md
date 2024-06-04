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

Methylation of DNA at cytosine nucleotides forms 5-methylcytosine  (5mC), which impacts various cellular processes involving gene expression and chromatin remodeling. These processes can influence health and development through methylation of promoter regions, cell differentiation, remodeling of chromatin for selective X-chromosome 
inactivation, and suppression of transposable elements. 

Next-generation sequencing technology has enabled genome-wide analysis of 5mC nucleotides at single-nucleotide resolution. In order to allow the decision of balance between cost and coverage to be decided, vairous level of method has been developed for bisufltie sequencing (discussed in next session). In general, bisufite seuqecing functions through the genomic DNA is treated with sodium bisulfite and then sequenced, providing single-base resolution of methylated cytosines in the genome. Upon bisulfite treatment, unmethylated cytosines are deaminated to uracils which, upon sequencing, are converted to thymidines. Simultaneously, methylated cytosines resist deamination and are read as cytosines. The location of the methylated cytosines can then be determined by comparing treated and untreated sequences. Bisulfite treatment of DNA converts unmethylated cytosines to thymidines, leading to reduced sequence complexity.


---

## Principle

Bisulftei seuqencing contains a list of different methods for the differenti requirements. In general, the most common applied method includes whole genome bisulftie seuqneicng, reduced representiation bisulfite seuqneicng and 

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
