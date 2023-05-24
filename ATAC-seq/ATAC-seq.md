# ATAC-seq manual

* [Introduction to ATAC-seq](#introduction-to-atac-seq)
    + [Background](#background)
    + [Principle](#principle)
    + [Experimental Design](#experimental-design)
* [Pipelines for ATAC-seq](#pipeline-for-atac-seq)
    + [Quality Control](#quality-control)
    + [Trimming](#trimming)
    + [Alignment](#alignment)
    + [Post-alignment Filtering](#post-alignment-filtering)
    + [Accessible Peak calling](#accessible-peak-calling)
    + [Differential accessibility](#differential-accessibility)
    + [Footprint analysis](#footprint-analysis)
* [Reference](#reference)

> Last Update: Day Month Year

# Introduction to ATAC-seq

## Background

Assay for Transposase-Accessible Chromatin using sequencing (ATAC-Seq) is a method to investigate the accessibility of chromatin stucture using Tn5 transposase. Accessible chromatin are highly coupled with regulatory mechanisms of gene expression thus this method can contribute to the identification of regulatory elements such as promoter, enhancer and silencer regions. There is several different techniques developed for the same purpose of ATAC-seq but ATAC-seq shows to be a best option as it easier, faster and require less cells then other techniques. In the same time, ATAC-seq are able to provide short accessible region which is limited in other techniques. 

In eukaryotic organisms, genomes are packed and organised into nuclelosomes which forms the chromatin structure. A  nucleosome is a complex formed by eight histone proteins that is wrapped with ~147bp of DNA. When the DNA is being actively transcribed into RNA, the DNA will be opened and loosened from the nucleosome complex. Many factors, such as the chromatin structure, the position of the nucleosomes, and histone modifications, play an important role in the organization and accessibility of the DNA. Consequently, these factors are also important for the activation and inactivation of genes. In the same time, the change in structure also impacts on the transcription factors binding activity which may increase or decrease based on each TF nature.

---

## Principle

With ATAC-Seq, to find accessible (open) chromatin regions, the genome is treated with a hyperactive derivative of the Tn5 transposase. A transposase can bind to a transposable element, which is a DNA sequence that can change its position (jump) within a genome. During ATAC-Seq, the modified Tn5 inserts DNA sequences corresponding to truncated Nextera adapters into open regions of the genome and concurrently, the DNA is sheared by the transposase activity. The read library is then prepared for sequencing, including PCR amplification with full Nextera adapters and purification steps. Paired-end reads are recommended for ATAC-Seq for the reasons described here.

> For anyone with interest, ATAC-seq introduces modification bias at the 5' of reads which if we would use this technique for DNA methylation detection then we need to remove 9bp from the 5' and activate the dovetail.

---

## Experimental Design

> Copied from [ATAC-seq Guidelines](https://informatics.fas.harvard.edu/atac-seq-guidelines.html)

### 1. Replicates

Same as all techniques, ATAC-seq requires biological replicates to provide accurate results. This ensures that any signals observed are due to biological effects rather then a single replicates handling or processing. To begin with, two replicates per experimental group are sufficient.

### 2. Controls

ATAC-seq may be provided with different type of controls. These includes the untreat control, the time zero control, or a simple reference control.
However, due to the expense of ATAC-seq, many research choose to simply sequence a normal genomic DNA extraction using other fragmentation protocols to collect regions that are problematic for sequencing or aligning. However, this has been covered by the blacklist project done by ENCODE providing a list of regions that may introduce these biases. 

### 3. PCR amplification

In preparing libraries for sequencing, the samples should be amplified using as few PCR cycles as possible. This helps to reduce PCR duplicates, which are exact copies of DNA fragments that can interfere with the biological signal of interest. Techniques such as ATAC-seq which uses an enzyme is well known to be suffering from duplication because of (a) PCR cycles or (b) the same region were extracted from many different cells.

### 4. Sequencing depth

The optimal sequencing depth varies based on the size of the reference genome and the degree of open chromatin expected. For studies of human samples, Buenrostro et al. (2015) recommend more than 50 million mapped reads per sample.

### 5. Sequencing mode

For ATAC-seq, use paired-end sequencing, for several reasons.

 - More sequence data leads to better alignment results. Many genomes contain numerous repetitive elements, and failing to align reads to certain genomic regions unambiguously renders those regions less accessible to the assay. Additional sequence data, such as with paired-end sequencing, helps to reduce these alignment ambiguities.

 - With ATAC-seq, we are interested in knowing both ends of the DNA fragments generated by the assay, since the ends indicate where the transposase inserted. This can be done only with paired-end reads. A single-end read defines only one end of the fragment, and, although computational methods to infer fragment lengths from single-end reads exist, they tend to be inaccurate.

 - PCR duplicates are identified more accurately. As noted above, PCR duplicates are artifacts of the procedure, and they should be removed as part of the analysis pipeline (see below for more details). However, computational programs that remove PCR duplicates (e.g. Genrich) typically identify duplicates based on comparing ends of aligned reads. With single-end reads, there is only one position to compare, and so any reads whose 5' ends match are considered duplicates. Thus, many false positives may result, and perfectly good reads are removed from further analysis. On the other hand, after paired-end sequencing, both ends of the original DNA fragments are defined. To be declared a duplicate, both ends of one fragment need to match both ends of another fragment, which is far less likely to occur by chance. Therefore, paired-end sequencing leads to fewer false positives.

### 6. Mitochondria

It is a well-known problem that ATAC-seq datasets usually contain a large percentage of reads (30~80%) that are derived from mitochondrial DNA. Since there are no ATAC-seq peaks of interest in the mitochondrial genome, these reads are discarded in the computational analysis and thus represent a waste of sequencing resources. The Omni-ATAC method uses detergents to remove mitochondria from the samples prior to sequencing and is likely to be accessible for most researchers.

---

# Pipeline for ATAC-seq

> Currectly there is many different pipelines for ATAC-seq but are limited with number of usable packages. 

## Quality Control

> Section update: Day Month Year

### Fastqc

ATAC-seq is based on Tn5 transposase which will introduce certain biases. In general cases, we expect the 3' of reads to contain a bias in the sequence context as Tn5 transposase are known to priotise the binding towards a specific regions. The following is example of the region.

## Trimming

> Section update: Day Month Year

For reads derived from short DNA fragments, the 3' ends may contain portions of the Illumina sequencing adapter. This adapter contamination may prevent the reads from aligning to the reference genome and adversely affect the downstream analysis. If you suspect that your reads may be contaminated with adapters (either from the FastQC report ["Overrepresented sequences" or "Adapter content" sections], or from the size distribution of your sequencing libraries), you should run an adapter removal tool. 

### Trim_galore & Cutadapt

ATAC-seq is commonly observed to contain contamination of adapter sequence. So it will be ideal to remove this content from the rest of the 

## Alignment

> Section update: 11 April 2023

The alignemnt of ATAC-seq remains to be the few main alignment tools for genome alignment including bowtie2 and bwa.

### Bowtie2

Here we will use Bowtie2. We will extend the maximum fragment length (distance between read pairs) from 500 to 1000 because we know some valid read pairs are from this fragment length. We will use the --very-sensitive parameter to have more chance to get the best match even if it takes a bit longer to run. We will run the end-to-end mode because we trimmed the adapters so we expect the whole read to map, no clipping of ends is needed. 

You might be surprised by the number of uniquely mapped compared to the number of multi-mapped reads (reads mapping to more than one location in the genome). One of the reasons is that we have used the parameter --very-sensitive. Bowtie2 considers a read as multi-mapped even if the second hit has a much lower quality than the first one. Another reason is that we have reads that map to the mitochondrial genome. The mitochondrial genome has a lot of regions with similar sequence.

## Post-alignment Filtering

> Section update: Day Month Year

We apply some filters to the reads after the mapping. ATAC-Seq datasets have a lot of reads that map to the mitchondrial genome because it is nucleosome-free and thus very accessible to Tn5 insertion. The mitchondrial genome is uninteresting for ATAC-Seq so we remove these reads. We also remove reads with low mapping quality and reads that are not properly paired.

High numbers of mitochondrial reads can be a problem in ATAC-Seq. Some ATAC-Seq samples have been reported to be 80% mitochondrial reads and so wet-lab methods have been developed to deal with this issue Corces et al. 2017 and Litzenburger et al. 2017. It can be a useful QC to assess the number of mitochondrial reads. However, it does not predict the quality of the rest of the data. It is just that sequencing reads have been wasted.

Because of the PCR amplification, there might be read duplicates (different reads mapping to exactly the same genomic region) from overamplification of some regions. As the Tn5 insertion is random within an accessible region, we do not expect to see fragments with the same coordinates. We consider such fragments to be PCR duplicates. We will remove them with Picard MarkDuplicates.
Once again, if you have a high number of replicates it does not mean that your data are not good, it just means that you sequenced too much compared to the diversity of the library you generated. Consequently, libraries with a high portion of duplicates should not be resequenced as this would not increase the amount of data.

## Accessible Peak calling

> Section update: Day Month Year

We have now finished the data preprocessing. Next, in order to find regions corresponding to potential open chromatin regions, we want to identify regions where reads have piled up (peaks) greater than the background read coverage. The tools which are currently used are Genrich and MACS2. MACS2 is more widely used. Genrich has a mode dedicated to ATAC-Seq but is still not published and the more reads you have, the less peaks you get (see the issue here). That’s why we will not use Genrich in this tutorial.

At this step, two approaches exists:

The first one is to select only paired whose fragment length is below 100bp corresponding to nucleosome-free regions and to use a peak calling like you would do for a ChIP-seq, joining signal between mates. The disadvantages of this approach is that you can only use it if you have paired-end data and you will miss small open regions where only one Tn5 bound.
The second one chosen here is to use all reads to be more exhaustive. In this approach, it is very important to re-center the signal of each reads on the 5’ extremity (read start site) as this is where Tn5 cuts. Indeed, you want your peaks around the nucleosomes and not directly on the nucleosome:

When Tn5 cuts an accessible chromatin locus it inserts adapters separated by 9bp (Kia et al. 2017):

Nextera Library Construction. 
Figure 16: Nextera Library Construction
This means in order to have the read start site reflecting the centre of where Tn5 bound, the reads on the positive strand should be shifted 4 bp to the right and reads on the negative strands should be shifted 5 bp to the left as in Buenrostro et al. 2013. Genrich can apply these shifts when ATAC-seq mode is selected. In most cases, we do not have 9bp resolution so we don’t take it into account but if you are interested in the footprint, this is important.

If we only assess the coverage of the 5’ extremity of the reads, the data would be too sparse and it would be impossible to call peaks. Thus, we will extend the start sites of the reads by 200bp (100bp in each direction) to assess coverage.

## Differential accessibility

> Section update: Day Month Year

Currently there is no package specially designed for ATAC-seq. Commonly people use csaw and DiffBind for ChIP-seq as a replacement.

## Footprint analysis

> Section update: Day Month Year


---

# Reference

This ATAC-seq library combined many information (or copied) from several tutorials. All involved tutorials are listed below:

https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-1929-3
https://training.galaxyproject.org/training-material/topics/epigenetics/tutorials/atac-seq/tutorial.html
https://nbis-workshop-epigenomics.readthedocs.io/en/latest/content/tutorials/atacseq_tutorials.html
https://bioinformaticsworkbook.org/dataAnalysis/ATAC-seq/ATAC_tutorial.html#gsc.tab=0
https://tobiasrausch.com/courses/atac/

Buenrostro et al. 2013, the first paper on the ATAC-Seq method
