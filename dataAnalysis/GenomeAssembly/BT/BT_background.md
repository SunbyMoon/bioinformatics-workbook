---
title: Organismal data
layout: single
header:
  overlay_color: "444444"
  overlay_image: /assets/images/dna.jpg
---

# Bacillus thuringiensis dataset

## Background

This particular genome is interesting because it shows insecticidal activity against Diptera.  The raw data contains 1.5G of data and the genome size is 6.4 mb.  Here is more background reading.

* [Article for the Complete genome sequence of Bacillus thuringiensis HS18-1](https://www.sciencedirect.com/science/article/pii/S0168165615300961)
* [ NCBI SRA for Bacillus thuringiensis strain: HS18-1](https://www.ncbi.nlm.nih.gov/sra/?term=SRR2093876)
* [NCBI genome assembly](https://www.ncbi.nlm.nih.gov/assembly/GCF_001182785.1).  

## Files

| SeqType   | Platform | BioProject  | Experiment  | Files ||
|-----------|----------|-------------|-------------|
| short Reads| Illumina HiSeq 2000   |  [PRJNA288953](https://www.ebi.ac.uk/ena/data/view/PRJNA288953) | [Bacillus thuringiensis strain:HS18-1 Genome sequencing and assembly](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA288953/)| [Forward1](ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/001/SRR2093871/SRR2093871_1.fastq.gz)| [Reverse2](ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/001/SRR2093871/SRR2093871_2.fastq.gz)
| short Reads| Illumina HiSeq 2000   |[PRJNA288953](https://www.ebi.ac.uk/ena/data/view/PRJNA288953) | [Bacillus thuringiensis strain:HS18-1 Genome sequencing and assembly](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA288953/)| [Forward1](ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/002/SRR2093872/SRR2093872_1.fastq.gz)| [Reverse2](ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/002/SRR2093872/SRR2093872_2.fastq.gz)
| Long Reads| PacBio RS II   | [PRJNA288953](https://www.ebi.ac.uk/ena/data/view/PRJNA288953) | [Bacillus thuringiensis strain:HS18-1 Genome sequencing and assembly](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA288953/)| [PacbioReads](ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/006/SRR2093876/SRR2093876_subreads.fastq.gz)|||

## How to download the data from ENA

The European Nucleotide Archive has the files already in fastq format and it is easy to download.
The main page with this project data is [PRJNA288953](https://www.ebi.ac.uk/ena/data/view/PRJNA288953)
We are only going to take the pacbio data which is the 3rd file link under the '''Read Files''' tab.

```
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/001/SRR2093871/SRR2093871_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/001/SRR2093871/SRR2093871_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/002/SRR2093872/SRR2093872_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/002/SRR2093872/SRR2093872_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR209/006/SRR2093876/SRR2093876_subreads.fastq.gz
```


## Assembly statistics for Bacillus thuringiensis
This assembly will be considered the gold standard that we strive for with the different assembly programs.

```
---------------- Information for assembly 'GCA_001182785.1_ASM118278v1_genomic.fna' ----------------


                                         Number of scaffolds         10
                                     Total size of scaffolds    6403459
                                            Longest scaffold    5292526
                                           Shortest scaffold       4669
                                 Number of scaffolds > 1K nt         10 100.0%
                                Number of scaffolds > 10K nt          7  70.0%
                               Number of scaffolds > 100K nt          3  30.0%
                                 Number of scaffolds > 1M nt          1  10.0%
                                Number of scaffolds > 10M nt          0   0.0%
                                          Mean scaffold size     640346
                                        Median scaffold size      92085
                                         N50 scaffold length    5292526
                                          L50 scaffold count          1
                                                 scaffold %A      32.55
                                                 scaffold %C      17.46
                                                 scaffold %G      17.54
                                                 scaffold %T      32.44
                                                 scaffold %N       0.00
                                         scaffold %non-ACGTN       0.00
                             Number of scaffold non-ACGTN nt          0
```
