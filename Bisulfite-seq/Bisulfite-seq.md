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

Before you begin with the study, please ensure that the use of BS-seq is the appropriate approach for your research. Review the relevant literature to determine if BS-seq is the best method for your study. Other protocols and methods for detecting DNA methylation may be more suitable depending on your specific research goals.

1. **Biological Replicates**
Biological replicates are crucial for distinguishing true biological variations from experimental noise and technical variability. It is recommended to have at least two biological replicates per condition. More replicates can increase the robustness of the results.

2. **Controls**
Including appropriate controls in your BS-seq experiment is essential for accurate interpretation of the data:

- **Untreated Control**: Use DNA that has not undergone bisulfite treatment to assess the efficiency of the treatment process and to identify any potential biases introduced by the treatment.
- **Spike-in Control**: Add known quantities of methylated and unmethylated control DNA to your sample. This allows for monitoring the bisulfite conversion efficiency and sequencing accuracy.
- **Negative Control**: Include a DNA sample known to be unmethylated. This helps to check for false positives and ensure that any detected methylation is genuine.

3. **PCR Amplification**
Minimize the number of PCR cycles to reduce PCR bias and the generation of duplicate reads. Over-amplification can lead to the over-representation of certain sequences, distorting the methylation profile. Use high-fidelity polymerases and optimize PCR conditions to ensure accurate amplification.

4. **Sequencing Depth**
The required sequencing depth depends on the size of the genome and the resolution needed. For human samples, a minimum of 30x coverage is often recommended. Higher coverage may be necessary for more detailed analyses or for samples with low DNA input. Consider the following guidelines:

- **Whole Genome Bisulfite Sequencing (WGBS)**: Aim for high coverage (30x or more) to ensure comprehensive methylation profiling.
- **Reduced Representation Bisulfite Sequencing (RRBS)**: Lower coverage (10-20x) may be sufficient due to the focus on CpG-rich regions.
- **Targeted Bisulfite Sequencing**: Coverage requirements will depend on the size and number of targeted regions.

5. **Sequencing Mode**
Paired-end sequencing is preferred for BS-seq because it provides more comprehensive coverage and helps in the alignment of reads, especially in repetitive regions of the genome. Paired-end reads also facilitate the detection of methylation patterns across longer genomic regions.

6. **Conversion Efficiency**
High conversion efficiency is critical for accurate methylation analysis. Assess the efficiency by including known quantities of unmethylated control DNA and calculating the percentage of cytosines converted to thymines. Aim for a conversion efficiency above 98%.

7. **Library Preparation**
Library preparation for BS-seq involves several steps, each of which must be optimized to ensure efficient and unbiased preparation of sequencing libraries:

- **DNA Fragmentation**: Fragment the DNA to the desired size (typically 200-500 bp) using a sonicator or a restriction enzyme.
- **End-Repair and Adapter Ligation**: Perform end-repair to generate blunt-ended DNA fragments. Ligate methylated adapters to the ends of the DNA fragments to prevent their conversion during the bisulfite treatment.
- **Bisulfite Treatment**: Treat the DNA with sodium bisulfite following the manufacturer's protocol. This step involves denaturation, conversion, and desulphonation.
- **PCR Amplification**: Amplify the bisulfite-treated DNA using PCR with primers specific to the methylated adapters. Minimize the number of PCR cycles to reduce biases.
- **Library Quality Control**: Assess the quality and quantity of the prepared libraries using a Bioanalyzer or similar instrument.

8. **Data Analysis**
Data analysis involves several steps to accurately determine the methylation status of cytosines across the genome:

- **Read Alignment**: Align the sequencing reads to a bisulfite-converted reference genome using tools like Bismark or BSmap.
- **Methylation Calling**: Determine the methylation status of each cytosine by comparing treated and untreated sequences. Unmethylated cytosines, converted to thymine during bisulfite treatment, are distinguished from methylated cytosines, which remain as cytosine.
- **Quality Control**: Perform quality control checks to ensure data accuracy, including assessing conversion efficiency and checking for biases.
- **Differential Methylation Analysis**: Identify differentially methylated regions (DMRs) and analyze their biological significance. Tools like MethPipe and DSS can be used for this purpose.

Additional Considerations

- **Choice of BS-seq Method**: Different BS-seq methods such as WGBS, RRBS, or targeted BS-seq should be chosen based on the specific research goals, budget, and required resolution. Each method has its own advantages and limitations.
- **Alternative Methods**: Before finalizing your experimental design, consider other available methods for methylation analysis. Techniques like oxidative bisulfite sequencing (oxBS-seq) and methylated DNA immunoprecipitation sequencing (MeDIP-seq) may be suitable for specific applications.

By following these guidelines and ensuring a thorough understanding of the available methodologies, researchers can design and execute a BS-seq experiment that provides accurate and reproducible insights into DNA methylation and its role in gene regulation and disease.


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
