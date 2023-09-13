---
title: Genome analysis using Parabricks
author: Ghanshyam Chandra, Chirag Jain
date: 2021-10-15 02:00:00 +0530
tags: [WGS,Sequence Alignment]
math: true
image: /assets/img/blogs/ParaBricks-WGS/DNA.png
---
<head>
  <link
    href="https://fonts.googleapis.com/css?family=Montserrat" rel="stylesheet"/>
  <link rel="stylesheet" href="../../assets/css/main.css" />
  <link rel="stylesheet" href="../../assets/css/project.css" />
</head>

Image source: Design Cells/iStock/Getty Images
### **Benchmarking human whole genome and RNA sequencing using Nvidia Ampere A100 GPUs**
Nvidia Ampere A100 GPUs exploit wide SIMT architecture to accelerate world's most demanding HPC and AI workloads, and one of the key application in HPC is sequencing of human genome and transcriptome. This blog aims to measure runtime required for genome and transcriptome sequencing using GPUs to help understand how quickly sequence analysis can be done.
## PARABRICKS DNA FQ2BAM pipeline

### Dataset:
- **Reference genome:** [GRCh38](https://github.com/broadinstitute/gatk/blob/master/src/test/resources/large/Homo_sapiens_assembly38.fasta.gz?raw=true)
- **Whole-genome sequencing run:** [ERR194147](https://www.ebi.ac.uk/ena/browser/view/ERR194147?show=reads) (Illumina HiSeq 2000 paired end sequencing, ~50x coverage)

### Results:

#### Table
> Execution Time

| Number of GPUs     | Execution Time(s) (alignment-phase BWA-mem) | Execution Time(s) (Overall) |
| ----------- | ----------- | ----------- |
| 1      | 9657      |             11053      |
| 2   | 4924         |             5903       |
| 4   | 2720         |             3389       |
| 8   | 1695         |             2204       |

>                     Table 1: Execution Time

#### Plot (Sequence alignment)

> Execution Time

![Execution Time](/assets/img/blogs/ParaBricks-WGS/BWA_Exec.png)
>                     Figure 1: Execution time for sequence alignment (BWA).

> Speed Up

![Speed Up](/assets/img/blogs/ParaBricks-WGS/BWA_SpeedUp.png)

>                     Figure 2: Speedup relative to single A100 GPU 

#### Plot (Overall)

> Execution Time

![Execution Time](/assets/img/blogs/ParaBricks-WGS/BWA_Exec_over.png)
>                     Figure 3: Execution time for DNA FQ2BAM pipeline.

> Speed Up

![Speed Up](/assets/img/blogs/ParaBricks-WGS/BWA_SpeedUp_over.png)

>                     Figure 4: Speedup relative to single A100 GPU 


**Scaling:** From Figure 4, we can clearly observe that with increase in number of GPUs, we are getting almost linear speedup, hence we can conclude that PARABRICKS DNA pipeline is strong scalable upto 8 GPUs.

## PARABRICKS RNA FQ2BAM pipeline

### Dataset:
- **Reference genome:** [GRCh38](https://www.ncbi.nlm.nih.gov/assembly/GCF_000001405.38/)
- **RNA-seq sample:** [SRR534301](https://www.ncbi.nlm.nih.gov/sra/?term=SRR534301) (Illumina HiSeq 2000 paired end sequencing)


### Results
#### Table

> Execution Time

| Number of GPUs     | Execution Time(s) (alignment-phase STAR) | Execution Time(s) (Overall) |
| ----------- | ----------- | ----------- |
| 1      | 1488      |             1529     |
| 2   | 987         |             1037       |
| 4   | 498         |             549       |
| 8   | 453         |             493       |

>                     Table 2: Execution Time

#### Plot (Sequence alignment)

> Execution Time

![Execution Time](/assets/img/blogs/ParaBricks-WGS/STAR_Exec.png)

>                     Figure 5: Execution time for sequence alignment using STAR algorithm.

> Speed Up

![Speed Up](/assets/img/blogs/ParaBricks-WGS/STAR_SpeedUp.png)

>                     Figure 6: Speedup relative to single A100 GPU 


#### Plot (Overall)

> Execution Time

![Execution Time](/assets/img/blogs/ParaBricks-WGS/STAR_Exec_over.png)

>                     Figure 7: Execution time for  RNA FQ2BAM pipeline.

> Speed Up

![Speed Up](/assets/img/blogs/ParaBricks-WGS/STAR_SpeedUp_over.png)

>                     Figure 8: Speedup relative to single A100 GPU 


**Scaling:** In Figure 8, we observe that while keeping data set fixed and with increase in number of GPUs, we are getting almost linear scaling up to 4 GPUs and then the speedup growth reduces. Hence, PARABRICKS RNA pipeline is showing good strong scaling up to 4 GPUs.

## Experimental Configuration

- **Compute Cluster:** PARAM-SIDDHI AI
- **Compute Node:** Single compute node (Nvidia DGX A100 320GB) \
 GPU: 8X A100-SXM4-40GB \
 CPU: Dual AMD EPYC 7742 64-Core Processor \
 RAM: 1 TB \
Storage: 8 PB (lustre fs)
- **PARABRICKS:** Nvidia Clara Parabricks: 3.6.0-1

## References
<b id="my_anchor">[1].</b> Nvidia Clara Parabricks 3.6.0-1 
[[Documentation]](https://docs.nvidia.com/clara/parabricks/v3.6/text/software_overview.html)

<b id="my_anchor">[2].</b> Param Siddhi supercomputer 
[[DST]](https://dst.gov.in/indias-ai-supercomputer-param-siddhi-63rd-among-top-500-most-powerful-non-distributed-computer)

<b id="my_anchor">[3].</b> Nvidia Ampere A100 GPU
[[Nvidia]](https://www.nvidia.com/en-in/data-center/a100/)
