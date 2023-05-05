README DATA
This folder contains DRIP-seq data from Sherrin, Sun, and Roy, 2021
Downloaded from BioProject accession number PRJNA721008 (1.19.2023)
Data processed and annotated by Bioinformatics team at IIHG from .bam raw 
files (received processed data back  2.12.2023)
>2.13.2023--Folder and README created and pushed to Git remote repo
>3.6.2023--Uploaded data xls and README to GitHub--received fatal errors 
when trying to push through command line; manually uploaded via Git website to main branch
>5.3.2023--Resolved Git issues by uninstalling all local and public repositories and re-authenticating. Pushed updated xls with the chr number and pos split to allow for easier interpretation in R. Column structure was previously for example "I:182364..238460." Split to include one column with the chr number as a numeric digit in applicable cases, used "mutate" to convert mitochondrial DNA and X chromosome reads which could not be given a numeric digit. Also copied total reads value to all cells in that column to make array the same size across columns after various issues in generating heatmap. 