[![Build Status](https://travis-ci.org/gavinha/TitanCNA.svg?branch=master)](https://travis-ci.org/gavinha/TitanCNA)

# *TitanCNA*

TitanCNA a R/Bioconductor package for analyzing subclonal copy number alterations (CNA) and loss of heterozygosity (LOH) in whole genome and exome sequencing of tumours.  
Ha, G., et al. (2014). [TITAN: Inference of copy number architectures in clonal cell populations from tumour whole genome sequence data. Genome Research, 24: 1881-1893.](http://genome.cshlp.org/content/24/11/1881) (PMID: 25060187)

## Contact
Gavin Ha  
Dana-Farber Cancer Institute  
Broad Institute  
contact: <gavinha@gmail.com> or <gavinha@broadinstitute.org>  
Date: May 13, 2017  

## Table of Contents
* [Links](#Links)
* [Latest version notes](#news)
* [Installation](#installation)
* [Usage](#usage)
* [Vignette](#vignette)
* [License](#license)
* [Acknowledgements](#acknowledgements)

## Links
TitanCNA GitHub: https://github.com/gavinha/TitanCNA  
TitanCNA utils: https://github.com/gavinha/TitanCNA-utils  
Google Groups: https://groups.google.com/forum/#!forum/titancna   
TitanCNA website: http://compbio.bccrc.ca/software/titan/  
Publication in Genome Research: http://genome.cshlp.org/content/24/11/1881

Thank you for your interest in the TitanCNA software.

## Latest version notes 
(See NEWS for previous version notes)

### TitanCNA version 1.15.0 changes 
1) 10X Genomics analysis
  - Please see https://github.com/gavinha/TitanCNA-utils for instructions on running the 10X Genomics data

2) New function
  - plotSegmentMedians()
  - loadHaplotypeAlleleCounts(): loads input allele counts with phasing information
  - plotHaplotypeFraction(): results from 10X Genomics WGS data with phasing of haplotype blocks
  
3) Modified features (no changes for user-accessible functions)
  - updateParameters: coordinate descent estimate of ploidy update uses previously estimated normal parameter from the same corodinate descent iteration ; leads to faster convergence
  - fwd_back_clonalCN.c: returns 2-slice marginals 

## Installation
### Install TitanCNA R package from github

From within R-3.3.2 or higher,  
```
install.packages("devtools")
library(devtools)
install_github("gavinha/TitanCNA")
```

### Install TitanCNA from Bioconductor
From within R-3.3.2 or higher,  
```
source("https://bioconductor.org/biocLite.R")
biocLite("TitanCNA")
```

### Install other dependencies  
1. Install the HMMcopy suite
Please follow instructions on the HMMcopy website <http://compbio.bccrc.ca/software/hmmcopy/>.

2. [KRONOS](https://github.com/MO-BCCRC/titan_workflow) TITAN Workflow
The easiest way to generate these files is by using the downloadable pipeline from https://github.com/MO-BCCRC/titan_workflow. 

## Usage
R scripts are provided to run the R component of the TITAN analysis using the TitanCNA R/Bioconductor package.  

**Input files**  
  This script assumes that the necessary input files have been generated. These are generated by the KRONOS workflow. 
  1. GC-corrected, normalized read coverage using the HMMcopy suite  
  2. Tumour allelic read counts at heterozygous SNPs (identifed from the normal sample).

**Running the R script**  
1. Look at the usage of the R script  
  ```
  # from the command line
  > Rscript titanCNA_v1.10.1.R --help
  Usage: Rscript titanCNA_v1.10.1.R [options]


  Options:
        --id=ID
                Sample ID

        --hetFile=HETFILE
                File containing allelic read counts at HET sites. (Required)

        --cnFile=CNFILE
                File containing normalized coverage as log2 ratios. (Required)

        --outDir=OUTDIR
                Output directory to output the results. (Required)

        --numClusters=NUMCLUSTERS
                Number of clonal clusters. (Default: 1)

        --numCores=NUMCORES
                Number of cores to use. (Default: 1)

        --ploidy_0=PLOIDY_0
                Initial ploidy value; float (Default: 2)

        --estimatePloidy=ESTIMATEPLOIDY
                Estimate ploidy; TRUE or FALSE (Default: TRUE)

        --normal_0=NORMAL_0
                Initial normal contamination (1-purity); float (Default: 0.5)

        --estimateNormal=ESTIMATENORMAL
                Estimate normal contamination method; string {'map', 'fixed'} (Default: map)

        --maxCN=MAXCN
                Maximum number of copies to model; integer (Default: 8)
        ...
  ```

3. Example usage of R script
  ```
  # normalized coverage file: test.cn.txt
  # allelic read count file: test.het.txt
  Rscript titanCNA_v1.10.1.R --id test --hetFile test.het.txt --cnFile test.cn.txt \
    --numClusters 1 --numCores 1 --normal_0 0.5 --ploidy_0 2 \
    --chrs "c(1:22, \"X\")" --estimatePloidy TRUE --outDir ./
  ```
  Additional arguments to consider are the following:  
    These arguments can be used to tune the model based on variance in the read coverage data and data-type (whole-exome sequencing or whole-genome sequencing).
    ```
    --alphaK=ALPHAK
                Hyperparameter on Gaussian variance; for WES, use 1000; for WGS, use 10000; 
                float (Default: 10000)

    --alphaKHigh=ALPHAKHIGH
                Hyperparameter on Gaussian variance for extreme copy number states; 
                for WES, use 1000; for WGS, use 10000; float (Default: 10000)
    ```


## Vignette in R package
The PDF of the vignette can be accessed from R
```
library(TitanCNA)
browseVignettes(package = "TitanCNA")
```
The path of the file can also be located using
```
pathToInstall <- system.file(package = "TitanCNA")
pathToPdf <- paste0(pathToInstall, "/int/doc/TitanCNA.pdf)
```
The example provided will reproduce Figure 1 in the manuscript. However, it will be slightly different because the example is only based the analysis of chr2, not genome-wide.

## License
TitanCNA is The license for the R Bioconductor package is now open source under GPLv3.  This applies to the v1.9.0 and all subsequent versions within and obtained from Bioconductor.  
Because the Bioconductor SVN is bridged to the GitHub TitanCNA repository "master" branch, the GitHub master branch for TitanCNA will also be under GPLv3.  
Please note that any other branch in the GitHub TitanCNA repository is still under the previous academic license. Users who are using TitanCNA for the purpose of academic research are free to use all branches and versions of the software. If this does not apply to you, please contact gavinha@broadinstitute.org, sshah@bccrc.ca, and prebstein@bccancer.bc.ca.

# Acknowledgements
TitanCNA was developed by Gavin Ha while in the laboratories of Sohrab Shah (sshah@bccrc.ca) and Sam Aparicio (saparicio@bccrc.ca) at the Dept of Molecular Oncology, BC Cancer Agency, Vancouver, Canada.
The KRONOS TITAN workflow was developed by Diljot Grewal (<dgrewal@bccrc.ca>) and Jafar Taghiyar (<jtaghiyar@bccrc.ca>).

