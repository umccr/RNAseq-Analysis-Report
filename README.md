# RNA-seq report

This reporting tool is designed to post-process, summarise and visualise an output from *[bcbio-nextgen](https://github.com/bcbio/bcbio-nextgen)* *[RNA-seq pipeline](https://bcbio-nextgen.readthedocs.io/en/latest/contents/pipelines.html#rna-seq)*. Its main application is to complement genome-based findings and to provide additional evidence for detected alterations.

NOTE, currently the pipeline is limited to report on samples from **pancreatic tissue** since only [pancreatic cancer reference cohort](https://github.com/umccr/Pancreatic-data-harmonization/tree/master/expression/in-house) have been assembled to date (see [Reference data](#reference-data) section). An exception is the *Fusion genes* report section, which relies solely on the whole-transcriptome sequencing (WTS) data from analysed sample.

## Table of contents

<!-- vim-markdown-toc GFM -->
* [Installation](#installation)
* [Workflow](#workflow)
* [Reference data](#reference-data)
  * [Internal reference cohort](#internal-reference-cohort)
  * [External reference cohort](#external-reference-cohort)
* [Testing](#testing)
* [UMCCR HPC](#umccr-hpc)
* [Usage](#usage)

<!-- vim-markdown-toc -->


## Installation

To be done...


## Workflow

The pipeline consist of four main components illustreted and breifly described below. See the [workflow.md](workflow.md) for the complete description of the workflow.

<img src="img/RNAseq_report_workflow.pdf" width="100%"> 

<br/>

1. Process the RNA-seq data from *[bcbio-nextgen](https://github.com/bcbio/bcbio-nextgen)* *[RNA-seq pipeline](https://bcbio-nextgen.readthedocs.io/en/latest/contents/pipelines.html#rna-seq)* including **gene fusions** and per-gene **read counts**. The [gene fusion](./fusions)  candidates are re-quantified and the read counts are [normalised, transformed](img/counts_post-processing_scheme.pdf) and [converted](img/Z-score_transformation_sample_wise.pdf) into a scale that allows to present the sample's expression measurements in the context of the [reference cohorts](#reference-data).

2. Feed in **genome-based findings** from whole-genome sequencing (WGS) data to focus on genes of interest and provide additional evidence for dysregulation of mutated genes, or genes located within detected structural variants (SVs) or copy-number (CN) altered regions. The RNA-seq report pipeline is designed to be compatible with WGS patient report based on [umccrise](https://github.com/umccr/umccrise) pipeline output.


3. Collate results with knowledge derived from public **databases** and **reference cohorts** to provide additional source of evidence, e.g. about variants clinical significance or potential druggable targets, for the genes of interest and to get an idea about their expression levels in other cancer [patient cohorts](#reference-data).

4. The final product is a html-based **interactive report** with searchable tables and plots presenting expression levels of the genes of interest genes. The report consist of several sections described in [report_structure.md](report_structure.md).


## Reference data

Currently, the reference data is availbale only for **pancreatic** cancer samples.


### Internal reference cohort

In order to explore expression changes in queried sample we have built a high-quality pancreatic cancer reference cohort. This set of **40 samples** is based on WTS data generated at **UMCCR** and processed with **[bcbio-nextgen](https://github.com/bcbio/bcbio-nextgen)** *[RNA-seq pipeline](https://bcbio-nextgen.readthedocs.io/en/latest/contents/pipelines.html#rna-seq)* to minimise potential batch effects between queried sample and the reference cohort and to make sure the data are comparable. The internal reference cohort assembly is summarised in [Pancreatic-data-harmonization](https://github.com/umccr/Pancreatic-data-harmonization/tree/master/expression/in-house) repository.

### External reference cohort

Additionally, pancreas adenocarcinoma (PAAD) expression data (**150 samples**) from [TCGA](https://tcga-data.nci.nih.gov/) have been processed and used as an external reference cohort (see [Pancreatic-data-harmonization](https://github.com/umccr/Pancreatic-data-harmonization/blob/master/expression/public/README.md#tcga-paad) repository for more details). This dataset, however, is expected to demonstrate prominent batch effects when compared to UMCCR WTS data due to differences in applied experimental procedures and analytical pipelines. Moreover, TCGA data may include samples from tissue material of lower quality and cellularity compared to samples processed at UMCCR.


## Testing

To be done...


## UMCCR HPC

Currently, we the pipeline is run on local machines but the aim is to set it up on [NCI Raijin](https://github.com/umccr/wiki/blob/master/computing/clusters/raijin-intro.md) and eventually on [Amazon Web Services](https://github.com/umccr/wiki/blob/master/computing/cloud/aws.md) (AWS) cloud computing.

To be done...


## Usage

To run the pipeline execure the *[summariseMAFs.R](./rmd_files/RNAseq_report.R)* script. This script catches the arguments from the command line and passes them to the *[summariseMAFs.Rmd](./rmd_files/RNAseq_report.Rmd)* script to produce the interactive HTML report.

Argument | Description | Required
------------ | ------------ | ------------
--sample_name | Desired sample name to be presented in the report | **Yes**
--tissue | Tissue from which the sample is derived | **Yes**
--count_file | Location and name of the read count file from *[bcbio-nextgen](https://github.com/bcbio/bcbio-nextgen)* *[RNA-seq pipeline](https://bcbio-nextgen.readthedocs.io/en/latest/contents/pipelines.html#rna-seq)* | **Yes**
--report_dir | Desired location for the report | **Yes**
--transform | Transformation method for converting read counts. Available options are: `CPM` (defualt) and `TPM` | No
--norm | Normalisation method. Available options are: `TMM` (for *CPM-transformed* data) and `quantile` (for *TPM-transformed* data) | No
--filter | Filtering out low expressed genes. Available options are: `TRUE` (defualt) and `FALSE` | No
--log | Log (base 2) transform data before normalisation. Available options are: `TRUE` (defualt) and `FALSE` | No
--scaling | Apply row-wise (across samples) or column-wise (across genes in a sample) data scaling. Available options are: `sample-wise` (across samples, default) or `gene-wise` (across genes) | No
--sample_id | Sample ID required to match sample with clinical information (if available) | No
--batch | Location of the corresponding WGS-related data | No
--clinical_info | Location of *xslx* file with clinical information | No
--plots_mode | Plotting mode. Available options: `Static` (default), `interactive` and `semi-interactive` | No
--hide_code_btn | Hide the *Code* button allowing to show/hide code chunks in the final HTML report. Available options are: `TRUE` (defualt) and `FALSE` | No

<br />




