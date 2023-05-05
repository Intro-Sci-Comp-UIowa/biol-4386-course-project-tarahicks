---
output:
  pdf_document: default
  html_document: default
---
# AMPK counteracts anomalous R-loops at open promoters in starved C. elegans germlines

## Reference
Sun, B., Sherrin, M., Roy, R., 2023. Unscheduled epigenetic modifications cause 
genome instability and sterility through aberrant R-loops following starvation. 
*Nucleic Acids Research*. 51:1. 84-98. 

## Introduction

Chromatin dynamics are at the heart of successful meiosis and proper regulation of 
epigenetic modifications associated with transcriptional processes is essential to 
proper chromosome segregation during meiotic divisions. While DNA double strand 
breaks (DSBs) are abundant and, contrarily to cycling nuclei, systematically 
required in meiotic nuclei, their formation is a tightly-regulated and programmed 
process. DSB formation favors open chromatin([Price & Andrea, 
2013](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3670600/)) and unscheduled 
euchromatic regions can therefore poteniate unprogrammed DSBs in turn. In meiotic 
nuclei, failure to repair aberrant DSBs is a significant threat to gamete genome 
integrity and defects can manifest transgenerationally or lead to 
reproductive sterility as a result. Therefore, understanding the nuclear chromatin 
environment and the regulatory network surrounding chromatin dynamics is of major 
interest to meiosis research and is key to capturing molecular targets in clinical 
applications. When a system is stressed, the epigenetic environment undergoes 
significant changes in response as gene expression requirements are shifted ([Gudsnuk 
& Champagne, 2012](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4021821/)). [Sun, 
Sherrin, & Roy, 2023](https://academic.oup.com/nar/article/51/1/84/68876020) found 
that under starvation conditions, H3K4me3 marks are significantly elevated in 
*Caenorhabditis elegans* meiotic nuclei at open promoters in accompaniment with 
aberrant R-loops when the AMP kinase is absent. They propose that R-loops accumulate 
as the result of hyperactive transcription during starvation and then hijack key 
repair proteins from programmed DSB sites, leading to aberrant repair and 
transgenerational sterility. However, studies performed in yeast models ([Yang et. 
al, 2022](https://www.sciencedirect.com/science/article/pii/S2211124721015916)) 
suggest that R-loops may actually drive DSB repair in meiosis under wild-type conditions. 
While a field consensus on R-loops' full contribution to the regulation of meiosis 
has not yet been reached, it is clear that they are associated with meiotic DSBs in 
some capacity, and therefore may aid in the identification of DSB hotspots. My aim is 
thus to compile a picture of where R-loop reads map on the whole-genome scale in an 
effort to identify candidate hotspot regions using the wild-type data of [Sun, Sherrin, & Roy, 
2023](https://www.sciencedirect.com/science/article/pii/S2211124721015916), as is represented in Figure 1 below.


**Figure 1: R-loop formation correlates with ectopic deposition of H3K4me3**


![Figure 1: R-loop formation correlates with ectopic deposition of H3K4me3](https://github.com/Intro-Sci-Comp-UIowa/biol-4386-course-project-tarahicks/blob/main/Data/Fig3A-DRIP_seq_peak_SSR2023.png)


> **Legend from [Sun, Sherrin, and Roy, 2023](https://www.sciencedirect.com/science/article/pii/S2211124721015916)** (A) Genome browser snapshots of DRIP-seq signals at regions proximal to 
genes and RNH tracks. Green bars show R-loop peak calls. Track height represents read counts. 
(B) Overall comparison of DRIP-seq output (peak score and number) in WT versus 
*aak-1/2* genome. (C) DRIP-qPCR validation. WT or F2 descendants of starved *aak-1/2* 
mutants cultured with or without DRB treatment during starvation for 3 days were 
collected for DRIP. *tig-3* was selected as a negative control; n = 3, mean ± S.E.M. 
Signal values normalized with respect to input genomic DNA are plotted. *P< 0.05, 
**P< 0.01, ***P < 0.001 by Mann–Whitney U-test. (D) Metaplot of GC skew centered on 
all R-loop peaks. (E) Four enriched *de novo* motifs identified by HOMER analysis of 
AMPK mutant versus WT DRIP-seq. (F) Upper: annotation and peak location analyses for 
called R-loop peaks mapping to promoter–TSS, TTS, exon, intron and the other regions. 
Genomic peak proportion in per thousand is shown. Peaks accumulate predominantly at 
the promoter–TSS fraction in *aak-1/2* mutants. Lower: pie charts of DRIP-seq signal 
distribution for genic versus intergenic regions in WT or F2 descendants of starved 
aak-1/2 mutants. (G) Genome browser snapshots showing a representative sample of 
positively correlated R-loop signals and H3K4me3 levels in the F2 descendants of 
starved AMPK mutants.

This figure is showing the results of the authors' DNA-RNA immunprecipitation 
followed by high throughput sequencing (DRIP-Seq) experiments. This method captures 
R-loops by antibody immunoprecipitation and the immunoprecipitated products are then 
sequenced to identify regions of the genome at which R-loops were associated in the 
sample. While they only show information at loci whose gene ontology matched that of their point of interest (I.e. chromatin modulation in starvation), they provide fair quality deep sequencing data (>90% of reads map to *C. elegans* genome) which can be used to extrapolate further genomic information regarding the role of R-loops in *C. elegans* gene regulation. 


## Materials and Methods

The authors made their paired-end sequencing data publicly-available in a repository with NCBI [PRJNA721008] (https://www.ncbi.nlm.nih.gov/sra?linkname=bioproject_sra_all&from_uid=721008). These can also be found in the GitHub repository associated with this report. 
Raw sequencing data was processed by the Bioinformatics team at the IIHG core here at the University of Iowa. The data was provided in xls as the number of reads which mapped to a particular chromosome loci, with chromosome number and genomic position annotated via alignment with the *Caenorhabditis_elegans* assembly WBcel235 reference genome. 

To approach generating a heat map which represents a genomic snapshot of R-loop accumulation, this file was reorganized to match the necessary array structure for processing with R tools. An undergraduate assistant manually separated chromosome number from genomic position coordinates in the xls prior to integration into the RStudio platform. All downstream analysis was performed in RMarkdown within RStudio using code chunks to run code. .Rdata history is also available on GitHub. The xls was imported into the RStudio environment using the readxl package. Other packages that were required for this analysis include tidy, dplyr, and ggplot2. To arrange the data into a format that R could recognize and interpret, chromosome numbers were mutated such that numbers were transformed to a character variable so X chromosome and mitochondrial DNA reads could be incorporated into the representation using tidyr's data_clean function. Secondly, ranges in the original xls data file were provided as a "#..#" format. 

To parse the range into a tidier format, two separate columns for the start and end coordinates were generated using the ".." characters as a delimiter for the "separate" function. Data values in the column reflecting the raw number of reads associated with each row were converted to a numeric data type and were then calculated against the total number of reads to attain a percentage value for the number of reads at a particular location relative to the total input. 

Using ggplot2, the percentage of reads were then defined as the fill gradient with three colors for the low, midpoint, and high values respectively for the heat map. The x position was defined as the starting position and the y-axis was defined as the chromosome number (1-6, Mt for mitochondrial, and X) such that percentage of reads at particular loci would correlate based on their coordinates. The intended effect was a series of 7 rows correlating to the 7 possible chromosomes a read could be at and within those rows, colored bars which correlate to both the particular location along that chromosome and whose coloring would reflect the degree of enrichment among all genomic locations with peaks. This would theoretically provide a visual for any biases toward particular regions of the chromosomes (e.g. central or arm-biased). 

## Results and Discussion

By using the above described pipeline, figure 2 was generated. 

**Figure 2: Wild-type R-loop distribution along *C. elegans* chromosomes**


![Figure 2: Wild-type R-loop distribution along C. elegans chromosomes](https://github.com/Intro-Sci-Comp-UIowa/biol-4386-course-project-tarahicks/blob/main/Output/DRIP_Seq_chr_map-wt-5.4.2023.pdf)

While it is not precisely the anticipated product, the x and y-axes do reflect the desired outcome and reads are mapping along the chromosomes with color coding which is informative. Unsurprisingly, many of the reads map to the more central region of chromosome V. Because *C. elegans* are holocentric, their chromosomes do not exhibit the same aversion for the central region of the chromosomes that species with centromeres do. Indeed, worm chromosomes have a well-documented bias for gene density in the central region of the chromosome rather than the arms. Given R-loops' association with transcription and an increased gene density at chromosome V in general, this result aligns with expectations. What is surprising about this figure however is the enrichment of R-loops on the X-chromosome, as worms reduce transcription from the X chromosome to half. However, because the worms in this study were starved, there is a chance that this accumulation is due to a stress response or because starvation can initiate chromosome nondisjunction which results in XO males and could contribute to the signal. 

Despite the enrichment at the central region of chromosome V, there is also a stark accumulation of reads at chromosome ends which, as previously mentioned, are not generally gene-rich regions in worm genomes. Exciting new studies in R-loop biology suggest that the long non-coding RNA TERRA upregulates R-loop formation even in the absence of active transcription and may use this mechanism to model telomere ends. Therefore, the accumulation of R-loops at chromosome ends in a wild type background may suggest that R-loops are a natural regulatory or structural component to t-loop formation, making them an indispensable contributor to genome stability as well as a major health threat by way of alternative lengthening of telomeres (ALT) and telomere immortality. 

## Conclusions

R-loops are often associated with transcription-replication conflicts which ultimately can result in DNA double strand breaks. However, R-loops are a particularly transient structure, and there is overwhelming evidence across taxa that the balance between too many and too few R-loops is key to proper gene expression, Ig class switching, mitochondrial DNA stability, and homology-driven recombination pathways. Their contributions to neurological disease, cancer, and aging have been well documented in cycling systems, however the roles they may play in the germline have been severely neglected in the field. Given their essential role in crossover assurance and DSB repair fate in both fission and budding yeast meiosis, it stood to reason that the same might be true in other organisms. Because *C. elegans* do not have any known DSB hotspots, the ability to study certain meiotic processes, like crossover interference and assurance, is limited. If R-loops constitute a regulatory facet behind meiotic DSBs, this may provide a unique avenue to identify hotspots which have eluded others. Given the association of R-loops from this sample set (figure 2) to gene-rich and telomeric regions of the genome, it is clear that regardless of whether this analysis actually provides an avenue to identify DSB hotspots, there is biological importance for R-loops in genome integrity and their contributions are likely to key cell biological processes. 

## Reflections

When performing the analysis, I struggled most with the ggplot2 rendering and surprisingly, the knitr conversion to PDF in RStudio. I think with ggplot2, I needed to set bases between the percentages of reads, however most of the reads have fairly low percentages (~1-2%) and then there is one spot that represents 25% of the reads, and setting a scale became rather difficult to do. There are also numbers correlating to the read positions on the x-axis that I was unsuccessful in removing. However all that said, I was actually very excited to see the plot look somewhat like what I wanted and going through the troubleshooting process for it to get even what I got was a great learning experience. I have some notes for myself for next time, but I can finally say I did at least something in R!






 

  
 
