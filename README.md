# sex_diff_microbiota

Analysis code for investigating the effect of fetal sex on maternal gut microbiota composition and predicted metabolic function.

This repository contains R Markdown workflows used to process 16S rRNA sequencing data, generate ASV tables, build phyloseq objects, test associations between fetal sex and maternal gut microbial features, analyze predicted functional pathways, and prepare data for downstream visualization, network analysis, and machine learning.

## Project overview

The goal of this project is to evaluate whether fetal sex is associated with differences in the maternal gut microbiome during pregnancy. The analyses include:

* Processing paired-end 16S rRNA sequencing reads with DADA2
* Assigning taxonomy and generating ASV-level count tables
* Creating and filtering phyloseq objects
* Comparing alpha and beta diversity by fetal sex
* Testing microbial taxa and predicted metabolic features for associations with fetal sex, gestational age, and fetal sex-by-gestational age interactions
* Annotating significant KEGG orthologs
* Performing ASV-level graph/network analyses
* Preparing feature tables for downstream machine learning analyses

## Repository structure

```text
sex_diff_microbiota/
├── README.md
└── R_markdown/
    ├── dada2.Rmd
    ├── phyloseq_final.Rmd
    ├── metagenomeSeq_final.Rmd
    ├── maaslin2_pathways.Rmd
    ├── maaslin2_KEGGs_final.Rmd
    ├── KEGGREST_final.Rmd
    ├── ML_data_prep_final.Rmd
    ├── se_asv_graph_analysis_final.Rmd
    └── spiec_easi_bootstrap_asv.Rmd
```

## Workflow summary

### 1. Read processing and ASV inference

`R_markdown/dada2.Rmd`

This workflow processes raw paired-end FASTQ files using DADA2. Main steps include:

1. Loading raw FASTQ files
2. Inspecting read quality profiles
3. Filtering and trimming reads
4. Learning error models
5. Inferring ASVs
6. Merging paired reads
7. Removing chimeras
8. Assigning taxonomy using SILVA
9. Creating and saving phyloseq objects

Expected input files include raw sequencing reads in a local `raw_data/fastq/` directory and a SILVA taxonomy training set.

### 2. Phyloseq processing and diversity analyses

`R_markdown/phyloseq_final.Rmd`

This workflow creates filtered phyloseq objects and performs diversity analyses. Main steps include:

1. Loading ASV-level phyloseq objects
2. Filtering metadata to retain selected healthy pregnancies
3. Filtering low-abundance and low-prevalence taxa
4. Normalizing counts using cumulative sum scaling
5. Comparing alpha diversity metrics, including Shannon, Simpson, and Chao1 diversity
6. Performing beta diversity analyses using Bray-Curtis distances and PCoA
7. Testing associations with fetal sex while adjusting for covariates such as age, BMI before pregnancy, parity, residency status, and gestational week

### 3. Differential abundance analyses

`R_markdown/metagenomeSeq_final.Rmd`
`R_markdown/maaslin2_pathways.Rmd`

These workflows test microbial features for associations with fetal sex and pregnancy-related covariates. Depending on the script, analyses include taxonomic features, pathway-level features, and covariate-adjusted models.

### 4. Predicted functional analysis and KEGG annotation

`R_markdown/maaslin2_KEGGs_final.Rmd`
`R_markdown/KEGGREST_final.Rmd`

These workflows analyze predicted microbial metabolic function. Main steps include:

1. Loading KEGG ortholog or pathway abundance tables
2. Transforming feature matrices for association testing
3. Running MaAsLin2 models
4. Testing fetal sex, gestational week, and fetal sex-by-gestational week interactions
5. Extracting significant KEGG features
6. Annotating KEGG IDs using KEGGREST
7. Generating summary tables and plots for significant functional associations

### 5. ASV network and graph analyses

`R_markdown/se_asv_graph_analysis_final.Rmd`
`R_markdown/spiec_easi_bootstrap_asv.Rmd`

These workflows are used for ASV-level network or graph-based analyses, including co-occurrence-style analyses and bootstrapping approaches.

### 6. Machine learning data preparation

`R_markdown/ML_data_prep_final.Rmd`

This workflow prepares microbiome feature matrices and metadata for downstream machine learning analyses.

## Expected local directory structure

Some intermediate and raw data files are not included in this repository. The scripts expect local directories such as:

```text
raw_data/
├── fastq/
└── filereport_read_run_PRJEB31743_tsv.txt

data/
├── dada2/
├── phyloseq/
├── maaslin2/
└── microbiomeProfiler/

supp_files/
```

You may need to create these directories before running the analyses.

## Installation

The analyses are written in R/R Markdown. Required packages vary by workflow but include:

```r
install.packages(c(
  "tidyverse",
  "ggplot2",
  "vegan",
  "ggpubr",
  "ggcorrplot",
  "reshape2",
  "compositions",
  "BiocManager"
))

BiocManager::install(c(
  "dada2",
  "phyloseq",
  "Biostrings",
  "ShortRead",
  "metagenomeSeq",
  "biomformat",
  "KEGGREST"
))
```

Additional packages used in some workflows include:

```r
install.packages(c(
  "here",
  "Maaslin2",
  "spiec.easi"
))
```

Depending on your R version and operating system, some packages may need to be installed from Bioconductor, GitHub, or other package-specific repositories.

## Running the workflows

The workflows are written as R Markdown documents and can be run interactively in RStudio or rendered from the command line.

Example:

```r
rmarkdown::render("R_markdown/dada2.Rmd")
```

A typical analysis order is:

1. `dada2.Rmd`
2. `phyloseq_final.Rmd`
3. `metagenomeSeq_final.Rmd`
4. `maaslin2_pathways.Rmd`
5. `maaslin2_KEGGs_final.Rmd`
6. `KEGGREST_final.Rmd`
7. `se_asv_graph_analysis_final.Rmd`
8. `spiec_easi_bootstrap_asv.Rmd`
9. `ML_data_prep_final.Rmd`

Some scripts depend on intermediate files generated by earlier scripts. File paths may need to be updated depending on where the raw data and processed outputs are stored.

## Reproducibility notes

* Raw sequencing data and large intermediate files are not stored in this repository.
* Several scripts assume a project-relative directory structure using the `here` package.
* Some output files are written to local `data/` subdirectories.
* File paths, metadata column names, and filtering criteria should be checked before rerunning the workflows on a new system.
* The current scripts are organized as analysis notebooks rather than a fully automated pipeline.

## Citation

If you use this code, please cite the associated manuscript or preprint.

**Manuscript citation:**
Blake M. Williams, Jessica Liu, Beatriz Peñalver Bernabé. bioRxiv 2025.06.15.659758; doi: https://doi.org/10.1101/2025.06.15.659758

## Contact

For questions about this repository, please contact the repository maintainer or open an issue on GitHub.


