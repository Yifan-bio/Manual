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

> Last Update: 07 May 2024

# Introduction to ATAC-seq

## Background

Assay for Transposase-Accessible Chromatin using sequencing (ATAC-Seq) is a method to investigate the accessibility of chromatin stucture using Tn5 transposase. Accessible chromatin are highly coupled with regulatory mechanisms of gene expression thus this method can contribute to the identification of regulatory elements such as promoter, enhancer and silencer regions. There is several different techniques developed for the same purpose of ATAC-seq but ATAC-seq shows to be a best option as it easier, faster and require less cells then other techniques. In the same time, ATAC-seq are able to provide short accessible region which is limited in other techniques. 

In eukaryotic organisms, genomes are packed and organised into nuclelosomes which forms the chromatin structure. A nucleosome is a complex formed by eight histone proteins that is wrapped with ~147bp of DNA. When the DNA is being actively transcribed into RNA, the DNA will be opened and loosened from the nucleosome complex. Many factors, such as the chromatin structure, the position of the nucleosomes, and histone modifications, play an important role in the organization and accessibility of the DNA. Consequently, these factors are also important for the activation and inactivation of genes. In the same time, the change in structure also impacts on the transcription factors binding activity which may increase or decrease based on each TF nature.

---

## Principle

With ATAC-Seq, to find accessible (open) chromatin regions, the genome is treated with a hyperactive derivative of the Tn5 transposase. A transposase can bind to a transposable element, which is a DNA sequence that can change its position (jump) within a genome. During ATAC-Seq, the modified Tn5 inserts DNA sequences corresponding to truncated Nextera adapters into open regions of the genome and concurrently, the DNA is sheared by the transposase activity. The read library is then prepared for sequencing, including PCR amplification with full Nextera adapters and purification steps. Paired-end reads are recommended for ATAC-Seq for the reasons described here.

---

## Experimental Design

### 1. Replicates

Same as all techniques, ATAC-seq requires biological replicates to provide accurate results. This ensures that any signals observed are due to biological effects rather then a single replicates handling or processing. To begin (but more is better) with, two replicates per experimental group are sufficient in the case of differential analysis as evidence suggested two replicates to recall 77% of events found in six replicates.

### 2. Controls

ATAC-seq may be provided with different type of controls. These includes the untreat control, the time zero control, or a simple reference control.
However, due to the expense of ATAC-seq, many research choose to simply sequence a normal genomic DNA extraction using other fragmentation protocols to collect regions that are problematic for sequencing or aligning. However, this has been covered by the blacklist project done by ENCODE providing a list of regions that may introduce these biases. 

### 3. PCR amplification

In preparing libraries for sequencing, the samples should be amplified using as few PCR cycles as possible. This helps to reduce PCR duplicates, which are exact copies of DNA fragments that can interfere with the biological signal of interest. Techniques such as ATAC-seq which uses an enzyme is well known to be suffering from duplication because of (a) PCR cycles or (b) the same region were extracted from many different cells.

### 4. Sequencing depth

The optimal sequencing depth varies based on the size of the reference genome and the degree of open chromatin expected. For studies of human samples, Buenrostro et al. (2015) recommend more than 50 million mapped reads per sample. You will need atleast this depth for use footprint analysis, the detection of TF interaction pattern in the sample.

### 5. Sequencing mode

For ATAC-seq, use paired-end sequencing, for several reasons.

 - More sequence data leads to better alignment results. Many genomes contain numerous repetitive elements, and failing to align reads to certain genomic regions unambiguously renders those regions less accessible to the assay. Additional sequence data, such as with paired-end sequencing, helps to reduce these alignment ambiguities.

 - With ATAC-seq, we are interested in knowing both ends of the DNA fragments generated by the assay, since the ends indicate where the transposase inserted. This can be done only with paired-end reads. A single-end read defines only one end of the fragment, and, although computational methods to infer fragment lengths from single-end reads exist, they tend to be inaccurate.

 - PCR duplicates are identified more accurately. As noted above, PCR duplicates are artifacts of the procedure, and they should be removed as part of the analysis pipeline (see below for more details). However, computational programs that remove PCR duplicates (e.g. Genrich) typically identify duplicates based on comparing ends of aligned reads. With single-end reads, there is only one position to compare, and so any reads whose 5' ends match are considered duplicates. Thus, many false positives may result, and perfectly good reads are removed from further analysis. On the other hand, after paired-end sequencing, both ends of the original DNA fragments are defined. To be declared a duplicate, both ends of one fragment need to match both ends of another fragment, which is far less likely to occur by chance. Therefore, paired-end sequencing leads to fewer false positives.

### 6. Mitochondria

It is a well-known problem that ATAC-seq datasets usually contain a large percentage of reads (30~80%) that are derived from mitochondrial DNA because it is nucleosome-free and thus very accessible to Tn5 insertion. Since the whole mitochondrial genome is accessible throughout cells, these reads are discarded in the computational analysis and thus represent a waste of sequencing resources. The Omni-ATAC method uses detergents to remove mitochondria from the samples prior to sequencing and is likely to be accessible for most researchers.

### 7. Tn5 bias

Hyperactive Tn5 transposase are used for the development of ATAC-seq library. The Tn5 transposase are found to biasly prefer the interaction to a certain pattern which can be observed in the fastqc result. Additionally, Tn5 intorduces a 9bp bias in which the Tn5 transposes cuts 4/5bp further then the 5' end of Read1 and Read2 so we need to furhter extend the reads for the region to collect the accurate interaction site of Tn5 with the accessible region. This means in order to have the read start site reflecting the centre of where Tn5 bound, the reads on the positive strand should be shifted 4 bp to the right and reads on the negative strands should be shifted 5 bp to the left as in Buenrostro et al. 2013. 

> For anyone with interest, ATAC-seq introduces modification bias at the 5' of reads which if we would use this technique for DNA methylation detection then we need to remove 9bp from the 5' and activate dovetail.

### 8. Read Size

Different from other technique, ATAC-seq does not perform additional step between enzyme treatment and PCR amplificaiton. Therefore, in this case, all sequence of all size will be included into the sequencing step. Due to the principle of ATAC-seq, it allows us to islate accessible regions that are as low as to 40bp which is lower the the recommended sequence length of above 75bp. Therefore, analysis will need to consider the case when a sequence read length is way lower then the actual read length we have. And also due to this case, we expect that many of the reads will contain adapter sequence within the sequences reads so trimming will therefor be vital. 

In the original study of ATAC-seq, they hypothsized that ATAC-seq isolate regions based on the different number of nucleosome been removed. Listed below
- <100bp (nucleosome-free regions): These reads are expected to contain no nucleosomes whithin these regions
- 100bp : Contains one nucleosome
- 200bp : Contain two nucleosomes
- 300bp: Contain three nucleosomes

>2000bp: ATAC-seq protocol does not have a size filtering step, however, due to the limitation of illumina sequencing, read with too many protein interaction will not be attached to the illumina platform and if they do, reads with a length of above 2000bp will not be sufficiently sequenced.

---

# Pipeline for ATAC-seq

> At the current time, many different pipeline has been developed for ATAC-seq with a very limited variety of packages. Some example pipeline including snakepipe; nextflow; AIAP; PEPATAC

## Quality Control

> Section update: 06 July 2023

Undoubtedly, assessing the quality of sequence data is the first step. Researchers commonly employ the tool **[fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)**. ATAC-seq is known to introduce various biases, as mentioned earlier. Therefore, in the fastqc results, certain indicators of poor quality are generally expected. These include a bias in the 3' due to the sequence bias of the Tn5 transposase, a high duplication rate, and uneven sequence length distribution. The reasons for each are explained below:

- Checkpoint One: Regarding sequence bias, we have mentioned that the Tn5 transposase exhibits a preference for interacting with specific sequence combinations. As a result, these sequences are more commonly found at the cutting points of the Tn5, leading to a sequence bias at the start of sequence reads of ATAC-seq.

- Checkpoint Two: Regarding the high duplication rate, since the Tn5 transposase exhibits bias, it tends to target similar regions across the sample cells. Consequently, it is highly likely to find two different identical regions targeted in the same sample, resulting in a high duplication rate. However, some researchers consider this a normal outcome of ATAC-seq bias, while others see it as a potential bias that could introduce errors due to potential PCR duplication mixtures.

- Checkpoint Three: The last bias is the sequence length distribution. ATAC-seq does not perform length selection, meaning sequences of all lengths are included in the PCR step. In these experiments, it is possible that regions below read length are extracted, leading to high number of adapter content after certain length. Thus, this is also why trimming is a crucial step in ATAC-seq.

## Trimming

> Section update: 07 May 2024

For reads derived from short DNA fragments, the 3' ends may contain portions of the Illumina sequencing adapter. This adapter contamination may prevent the reads from aligning to the reference genome and adversely affect the downstream analysis. If you suspect that your reads may be contaminated with adapters (either from the FastQC report or from the size distribution of your sequencing libraries), you should run an adapter removal tool. In this case, ATAC-seq must be trimmed as it does not have any experimental filter.

Few things to take into consideration for this section.

* Need to remove low quality bases
  - The phred quality can be vary, but need to be above 20 for precaution.
* Need to remove reads that are too short as it may introduce too many false alignment
  - Too short will lead to wrong alignment.
* Need to remove reads with bad pair as ATAC-seq need to be paired.
  - Single end alignment not recommended.

There is many different packges has been developed to perform this procedure. Common packages include cutadapt and trimmotatic are quite widely applied. Here is an example code of trim-galore, a further application of cutadapt, and few detail explained:

```sh
# ATAC-seq library trimming
trim_galore --paired \ # The library we are inputting are paired end sequencing
            --quality 20 \ # Phred cutoff of 20
            --length 20 \ # Read with length below 20 is insufficient as they will either be thrown out by the mapping or may interfere with our results at the end
            --output_dir $trim_dir \ # The output file of all info
            $R1 $R2 # Paired end so two fastq file for Read 1 and 2
```

## Alignment

> Section update: 11 April 2023

The alignemnt of ATAC-seq remains to be the few main alignment tools for genome alignment including bowtie2 and bwa. 
Alignment is alignment, so skip details here.

### Bowtie2

Here we will use Bowtie2. We will extend the maximum fragment length (distance between read pairs) from 500 to 2000 because we know some valid read pairs are from this fragment length. 

You might be surprised by the number of uniquely mapped compared to the number of multi-mapped reads (reads mapping to more than one location in the genome). One of the reasons is that we have used the parameter --very-sensitive. Bowtie2 considers a read as multi-mapped even if the second hit has a much lower quality than the first one. Another reason is that we have reads that map to the mitochondrial genome. The mitochondrial genome has a lot of regions with similar sequence.

```sh
# Bowtie2 alignment
bowtie2 --very-sensitive \ # more chance to get the best match even if it takes a bit longer to run
        --end-to-end \     # trimmed the adapters so we expect the whole read to map
        -p 4               # Number of threads
        --no-mixed \       # Only allow paired end reads, ATAC-seq without Paired will be problem
        -X 2000 \          # Valid read lengths are below 2000bp, as illumina sequencing over 2000bp will fail
        -x $Index \
        -1 $Read1 \
        -2 $Read2 \
        -S ${prefix}.sam \
        > ${prefix}.bowtie2.log
```

## Post-alignment Filtering

> Section update: 11 April 2023

### Step 1 Mitochondrial removal

ATAC-Seq datasets have been reported to contain [mitchondrial contamination](#experimental-design). Reads derived from mitochondrial DNA represent noise in ATAC-seq datasets and can substantially inflate the background level. Thus, we remove these reads. 
It can be a useful QC to assess the number of mitochondrial reads. However, it does not predict the quality of the rest of the data. It is just that sequencing reads have been wasted.

> We remove mitochondrial reads using "chrM" as it chromosome name. However, this varies across databases and this is for Gencode. ENSEMBL database uses "MT" as mitochondrial read chromosome names.
> Techniques such as Omni-ATAC-seq has removed MT contamination through wetlab protocols before sequencing.

```sh
samtools view -@ $threads -h $input.bam | grep -v chrM | samtools sort -@ $threads -O bam -o $output.bam
```

### Step 2 Low quality removal

We also remove reads with low mapping quality. Bowtie2 marks multimapped reads as below 20 so removing low quality can remove these reads. And we can also remove low quality alignment which has a lack of confident.

> The command we provide here only keeps primary alignment and proper paired reads. And also a mapping quality of above 30 (bowite2 max score is 42) providing sufficient quality

```sh
samtools view -h -b -q 30 -@ $threads -F 1804 -o $output.bam $input.bam
```

### Step 3 PCR deduplication ###

Because of the PCR amplification, there might be read duplicates (different reads mapping to exactly the same genomic region) from overamplification of some regions. As the Tn5 insertion is random within an accessible region, we do not expect to see fragments with the same coordinates. We consider such fragments to be PCR duplicates. We will remove them with Picard MarkDuplicates. There is samtools and sambamba deduplication which was previously shown to have a lack in ability for paired-end reads deduplication, there isnt recent studies on deduplication for new versions of these package.

> Once again, if you have a high number of duplicates it does not mean that your data are not good, it just means that you sequenced too much compared to the diversity of the library you generated. Consequently, libraries with a high portion of duplicates should not be resequenced as this would not increase the amount of data.

```sh
picard MarkDuplicates Input=$input.bam Output=$output.bam METRICS_FILE=$dedup.txt REMOVE_DUPLICATES=true
```

### Overall command 

```sh
# Combine step 1 and 2
samtools view -@ 4 -h -b -q 30 -@ 4 -f 2 ${prefix}.bam | grep -v chrM | samtools sort -o ${prefix}.rmChrM.bam
# Step 3
picard MarkDuplicates Input=${prefix}.rmChrM.bam Output=${prefix}.rmChrM_dedup.bam METRICS_FILE=${prefix}.dedup.txt REMOVE_DUPLICATES=true
# Indexing the file for future use
samtools index ${prefix}.final.bam
```

## Accessible Peak calling

> Section update: 11 April 2023

We have now finished the data preprocessing. Next, in order to find regions corresponding to potential open chromatin regions, we want to identify regions where reads have piled up (peaks) greater than the background read coverage. The tools which are currently used are Genrich and HMMRATAC. MACS2 was widely used but it incoporated HMMRATAC as their ATAC-seq algorithm in recent update. Both Genrich and HMMRATAC has not been updated in the past three years.

### Genrich

Genrich is still not published and there is issues on more reads you have, the less peaks you get. However, it is more computational easy to use and less computation resources to use. Additionally, it allows the combination of multiple replicates Genrich can allow the user to do the post-alignment filtering in the algorithm by just incorporating the commands in the algorithm. However, it requires MASSIVE computational power so do it before the peak calling step. This algorithm provides a narrowPeak alogrithm that provides the user with bunch of small peaks. Genrich can apply the Tn5 shifts when ATAC-seq mode is selected. In most cases, we do not have 9bp resolution so we don’t take it into account.

```sh
Genrich -t $input.bam \
        -o $output.narrowPeak \
        -k $output.log \
        -E $Blacklist.bed \     # Remove regions with a high accessibility across many techniques
        -j                      # ATAC-seq mode
```

### HMMRATAC

Another package that was developed for ATAC-seq peak calling is HMMRATAC. It provides the user with gappedPeak statergy which reports the accessible peak region and the flanking region that is hypothesized to support the accessibility. This method uses a machine learning method that divided the whole genome into center of peak, supporting region and background (non peak). Personally I prefer this method as it accomodates with the variation of ATAC-seq peak size. In other words, not all region will have a stable peak size variation, which most packages does not deal with, but HMMRATAC does. More details please refer to my method (Yi).
 
 ```sh
 # HMMRATAC peak calling
HMMRATAC -b ${prefix}.final.bam \
         -i ${prefix}.final.bam.bai \
         -g $GenomeFasta -o ${prefix}.hmmratac \ 
         -e $blacklist \  # Black list from ENCODE
         -m $readlength,200,400,600 \ # the first size is the read length
         --window 5000000 # remove RAM issue, if ram is problem then make this smaller
 ```

## Differential accessibility

> Section update: 06 May 2024

Currently there is no package specially designed for ATAC-seq. Commonly people use csaw and DiffBind for ChIP-seq as a replacement.

csaw divides the whole genomic region in fragments of a certain size. It then compares the numebr of read in each fragment. Therefore, it does not need to run peak detection. It is based off edgeR algorithm.

DiffBind recurate the peak we called in the previous section into a more standarised peak that takes into the consideration of the peak consistency across replicates and the size of peak. By which, it allows much less computation power, more specific focused region analysis and more diversity as peaks does not always fall into one fragment defined by csaw. DiffBind allows the differential analysis to be performed using edgeR or DiffBind.

## Footprint analysis

> Section update: TBU


 
---

# Reference

This ATAC-seq library combined many information (or copied) from several tutorials. All involved tutorials are listed below:

https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-1929-3
https://training.galaxyproject.org/training-material/topics/epigenetics/tutorials/atac-seq/tutorial.html
https://nbis-workshop-epigenomics.readthedocs.io/en/latest/content/tutorials/atacseq_tutorials.html
https://bioinformaticsworkbook.org/dataAnalysis/ATAC-seq/ATAC_tutorial.html#gsc.tab=0
https://tobiasrausch.com/courses/atac/

Buenrostro et al. 2013, the first paper on the ATAC-Seq method
