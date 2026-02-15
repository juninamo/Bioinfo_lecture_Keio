# Setup Guide: Single-Cell Analysis Environment

This guide will help you set up your environment for the single-cell analysis workshop. We cover both **macOS** and **Windows**.

## 1. Install R and RStudio

### For macOS
1.  **Download R**: Go to [cloud.r-project.org](https://cloud.r-project.org/bin/macosx/) and download the latest version for macOS (Apple Silicon/M1/M2/M3 or Intel).
2.  **Download RStudio Desktop**: Go to [posit.co/download/rstudio-desktop/](https://posit.co/download/rstudio-desktop/) and install the free version.

### For Windows
1.  **Download R**: Go to [cloud.r-project.org](https://cloud.r-project.org/bin/windows/base/) and download the latest version for Windows.
2.  **Download RStudio Desktop**: Go to [posit.co/download/rstudio-desktop/](https://posit.co/download/rstudio-desktop/) and install the free version.
3.  **Install Rtools**: **(CRITICAL for Windows)**
    -   Symphony and other advanced packages require Rtools to build from source.
    -   Download from [cloud.r-project.org](https://cloud.r-project.org/bin/windows/Rtools/) (Match the version to your R version, e.g., Rtools44 for R 4.4.x).

## 2. Install Required R Packages

Open RStudio and run the following commands in the Console.

### A. Basic Bioconductor Packages
```r
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install(c("BiocStyle", "glmGamPoi", "limma"))
```

### B. CRAN Packages (Seurat, Harmony, etc.)
```r
install.packages(c("Seurat", "harmony", "patchwork", "ggplot2", "dplyr", "remotes", "class"))
```

### C. Advanced Mapping Packages (Symphony & StabMap)
These packages are used in Part 2 of the workshop.

**Symphony:**
```r
remotes::install_github("immunogenomics/symphony")
```

**StabMap:**
```r
BiocManager::install("StabMap")
```

## 3. Prepare Data

### A. Download Data
We will use data from **GSE278962** (JIA Synovial CITE-seq).
1.  Download the data from the source provided in the lecture.
2.  Ensure you have the following files:
    -   `syno.rna.jia.rds` (Base dataset)
    -   `syno.rna_Fan.rds` (Reference dataset for Part 2)

### B. Organize Folders
Create a folder for your project (e.g., `SingleCellWorkshop`) and place the data files inside a `data/` subfolder.

```
SingleCellWorkshop/
├── data/
│   ├── syno.rna.jia.rds
│   └── syno.rna_Fan.rds
├── single_cell_analysis.Rmd       (Part 1)
└── single_cell_analysis_part2.Rmd (Part 2)
```

## 4. Verify Installation

Run this small script in R to check if everything is ready:

```r
library(Seurat)
library(harmony)
library(symphony)
library(StabMap)

print("All packages loaded successfully!")
```

If you see "All packages loaded successfully!", you are ready to go!
