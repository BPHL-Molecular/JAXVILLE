# JAXVILLE: A Rabies Analysis Pipeline

[![Nextflow](https://img.shields.io/badge/nextflow%20DSL2-%E2%89%A523.04.0-23aa62.svg)](https://www.nextflow.io/)
[![run with conda](http://img.shields.io/badge/run%20with-conda-3EB049?labelColor=000000&logo=anaconda)](https://docs.conda.io/en/latest/)
[![run with docker](https://img.shields.io/badge/run%20with-docker-0db7ed?labelColor=000000&logo=docker)](https://www.docker.com/)
[![run with singularity](https://img.shields.io/badge/run%20with-singularity-1d355c.svg?labelColor=000000)](https://sylabs.io/docs/)

## Introduction

This pipeline is designed for the analysis of rabies data using Pacbio or MinION sequencing data. It performs quality control, species identification, abundance estimation, SNP calling, and annotation.

## Prerequisites
Nextflow is needed. The details of installation can be found at https://github.com/nextflow-io/nextflow. For HiPerGator users, its installation is not needed. 

Singularity/APPTAINER is needed. The details of installation can be found at https://singularity-tutorial.github.io/01-installation/. For HiPerGator users, its installation is not needed.

SLURM is needed. For HiPerGator users, its installation is not needed.

Python3 is needed. The package "pandas" should be installed by ``` pip3 install pandas ``` if not included in your python3.

LongQC is needed. Please install it to your local computer from its GitHub repository (https://github.com/yfukasawa/LongQC). For HiPerGator users, its installation is not needed.

PacBio SMRTLINK stand-alone tools are needed. About how to install them, please see the file "How_to_install_smrtlink_tools.txt" in the pipeline.

The Kraken2/Bracken Refseq index--PlusPF is needed. Please download PlusPF index (over 77 GB) from the link (https://benlangmead.github.io/aws-indexes/k2) to the "PlusPF" folder in your local computer. And then extract the tar.gz archive. For HiPerGator users, downloading is not needed. It has been downloaded and configured in the pipeline.

## Recommended conda environment installation
   ```bash
   conda create -n JAXVILLE -c conda-forge python=3.10 pandas
   ```
   ```bash
   conda activate JAXVILLE
   ```
## Pipeline summary

1. Quality control ([`LongQC`](https://github.com/yfukasawa/LongQC))
2. Species identification ([`Kraken2`](https://ccb.jhu.edu/software/kraken2/))
3. Species abundance estimation ([`Bracken`](https://ccb.jhu.edu/software/bracken/))
4. Read alignment ([`pbmm2`](https://www.pacb.com/support/software-downloads/))
5. SNP calling ([`BCFtools`](https://samtools.github.io/bcftools/bcftools.html))
6. Variant annotation ([`SnpEff`](https://pcingola.github.io/SnpEff/))

## Pipeline Overview

![lyssa_chart](https://github.com/user-attachments/assets/78406c8e-e87a-4df5-b9b0-5eee6bf25ae0)


## How to run

1. Rename your data files and make them looks like "bc2024bc2024.bam.pbi" and "bc2024bc2024.bam". You can use to the script "rename.sh" in the pipeline to rename your data files.
2. Put the renamed data files (*.bam and *.bam.pbi) into the directory /pbbams.
3. Open file "params.yaml", set the full paths of the parameters.   
   **input** : the full path to pbbams dir of the pipeline in your computer. It looks like "/\<the full path to the pipeline dir in your computer\>/pbbams".    
   **output** : the full path to output dir of the pipeline in your computer. It looks like "/\<the full path to the pipeline dir in your computer\>/output".            
   **reference** : the full path to reference dir of the pipeline in your computer. It looks like "/\<the full path to the pipeline dir in your computer\>/reference".    
   **snpeffconfig** : the full path to configs dir of the pipeline in your computer. It looks like "/\<the full path to the pipeline dir in your computer\>/configs".      
          
   **db** : the full path to kraken/bracken-database (PlusPF) in your computer. It looks like "/\<the full path to the parent dir of PlusPF foler in your computer\>/PlusPF".    
   **qc** : the full path to LongQC dir in your computer. It looks like "/\<the full path to the parent dir of LongQC foler in your computer\>/LongQC".     
           
   **Note: For HiperGator users, the parameters "db" and "qc" do not need to be changed. Just keep default settings.**     

4. Get to the top directory of the pipeline, run 
```bash
sbatch ./lyssa.sh
```
#### Note:      
If you want to get email notification when the pipeline running ends, please input your email address in the line "#SBATCH --mail-user=<EMAIL>" of the file lyssa.sh.  
