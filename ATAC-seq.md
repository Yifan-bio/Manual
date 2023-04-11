
* [Introduction to ATAC-seq](#introduction-to-atac-seq)
* [Background for ATAC-seq](#background-for-atac-seq)
* [Principle behind ATAC-seq](#principle-behind-atac-seq)
* [Pipelines for ATAC-seq](#pipelines-for-atac-seq)
* 
* [Background for ATAC-seq](#background-for-atac-seq)
* [Reference](#reference)

# ATAC-seq manual



## Introduction to ATAC-seq

Assay for Transposase-Accessible Chromatin using sequencing (ATAC-Seq) is a method to investigate the accessibility of chromatin stucture using Tn5 transposase. Accessible chromatin are highly coupled with regulatory mechanisms of gene expression thus this method can contribute to the identification of regulatory elements such as promoter, enhancer and silencer regions. There is several different techniques developed for the same purpose of ATAC-seq but ATAC-seq shows to be a best option as it easier, faster and require less cells then other techniques. In the same time, ATAC-seq are able to provide short accessible region which is limited in other techniques. 

## Background for ATAC-seq

In eukaryotic organisms, genomes are packed and organised into nuclelosomes which forms the chromatin structure. A  nucleosome is a complex formed by eight histone proteins that is wrapped with ~147bp of DNA. When the DNA is being actively transcribed into RNA, the DNA will be opened and loosened from the nucleosome complex. Many factors, such as the chromatin structure, the position of the nucleosomes, and histone modifications, play an important role in the organization and accessibility of the DNA. Consequently, these factors are also important for the activation and inactivation of genes. In the same time, the change in structure also impacts on the transcription factors binding activity which may increase or decrease based on each TF nature.

## Principle behind ATAC-seq

With ATAC-Seq, to find accessible (open) chromatin regions, the genome is treated with a hyperactive derivative of the Tn5 transposase. A transposase can bind to a transposable element, which is a DNA sequence that can change its position (jump) within a genome. During ATAC-Seq, the modified Tn5 inserts DNA sequences corresponding to truncated Nextera adapters into open regions of the genome and concurrently, the DNA is sheared by the transposase activity. The read library is then prepared for sequencing, including PCR amplification with full Nextera adapters and purification steps. Paired-end reads are recommended for ATAC-Seq for the reasons described here.

# Pipelines for ATAC-seq

Currectly there is many different pipelines for ATAC-seq but are limited with number of usable packages.

## Alignment of ATAC-seq



# Reference

This ATAC-seq library combined many information (or copied) from several tutorials. All involved tutorials are listed below:

https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-1929-3
https://training.galaxyproject.org/training-material/topics/epigenetics/tutorials/atac-seq/tutorial.html
https://nbis-workshop-epigenomics.readthedocs.io/en/latest/content/tutorials/atacseq_tutorials.html
https://bioinformaticsworkbook.org/dataAnalysis/ATAC-seq/ATAC_tutorial.html#gsc.tab=0
https://tobiasrausch.com/courses/atac/

Buenrostro et al. 2013, the first paper on the ATAC-Seq method
