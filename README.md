# LYSSA: A Rabies Analysis Pipeline

[![Nextflow](https://img.shields.io/badge/nextflow%20DSL2-%E2%89%A523.04.0-23aa62.svg)](https://www.nextflow.io/)
[![run with conda](http://img.shields.io/badge/run%20with-conda-3EB049?labelColor=000000&logo=anaconda)](https://docs.conda.io/en/latest/)
[![run with docker](https://img.shields.io/badge/run%20with-docker-0db7ed?labelColor=000000&logo=docker)](https://www.docker.com/)
[![run with singularity](https://img.shields.io/badge/run%20with-singularity-1d355c.svg?labelColor=000000)](https://sylabs.io/docs/)

## Introduction

This pipeline is designed for the analysis of rabies data using Pacbio MinION sequencing. It performs quality control, species identification, abundance estimation, SNP calling, and annotation.

## Pipeline summary

1. Quality control ([`FastQC`](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/))
2. Species identification ([`Kraken2`](https://ccb.jhu.edu/software/kraken2/))
3. Species abundance estimation ([`Bracken`](https://ccb.jhu.edu/software/bracken/))
4. Read alignment ([`Minimap2`](https://github.com/lh3/minimap2))
5. SNP calling (choice of [`Sniffles`](https://github.com/fritzsedlazeck/Sniffles) or [`BCFtools`](https://samtools.github.io/bcftools/bcftools.html))
6. Variant annotation ([`SnpEff`](https://pcingola.github.io/SnpEff/))

## Pipeline Overview

```mermaid
graph TD
    A[Input: BAM] --> B[Quality Control]
    B --> C[Bam to Fastq]
    C --> D[De novo assembly]
    D --> E[Species Identification]
    E --> F[Species Abundance Estimation]
    F --> G[Read Alignment]
    G --> H[Variant Calling]
    H --> I[Variant Annotation]
    I --> J[Final Output]
    
    style A fill:#f9d79b,stroke:#f39c12,stroke-width:2px    
    style B fill:#aed6f1,stroke:#3498db,stroke-width:2px     
    style C fill:#aed6f1,stroke:#3498db,stroke-width:2px     
    style D fill:#aed6f1,stroke:#3498db,stroke-width:2px    
    style E fill:#aed6f1,stroke:#3498db,stroke-width:2px     
    style F fill:#f5b7b1,stroke:#e74c3c,stroke-width:2px     
    style G fill:#d5f5e3,stroke:#2ecc71,stroke-width:2px    
    style H fill:#d5f5e3,stroke:#2ecc71,stroke-width:2px    
    style I fill:#aed6f1,stroke:#3498db,stroke-width:2px    
    style J fill:#f9d79b,stroke:#f39c12,stroke-width:2px     
```

## Quick Start

1. Install [`Nextflow`](https://www.nextflow.io/docs/latest/getstarted.html#installation) (>=23.04.0)

2. Install any of [`Docker`](https://docs.docker.com/engine/installation/), [`Singularity`](https://www.sylabs.io/guides/3.0/user-guide/), [`Podman`](https://podman.io/), [`Shifter`](https://nersc.gitlab.io/development/shifter/how-to-use/) or [`Charliecloud`](https://hpc.github.io/charliecloud/) for full pipeline reproducibility _(you can use [`Conda`](https://conda.io/miniconda.html) both to install Nextflow itself and also to manage software within pipelines. Please only use it within pipelines as a last resort; see [docs](https://nf-co.re/usage/configuration#basic-configuration-profiles))_

3. Download the pipeline and test it on a minimal dataset with a single command:

   ```bash
   nextflow run /path/to/pipeline -profile test,YOURPROFILE --outdir <OUTDIR>
   ```

## To run this pipeline with Bracken for multiple taxonomic levels, you would use:     

  ```bash
  nextflow run main.nf -profile conda \
    --input 'path/to/your/files/*.{fastq,bam}' \
    --input_format auto \
    --snp_caller bcftools \
    --kraken2_db /path/to/kraken2_db \
    --bracken_db /path/to/bracken_db \
    --read_length 150 \
    --bracken_levels 'S,G,F' \
    --outdir results
  ```
