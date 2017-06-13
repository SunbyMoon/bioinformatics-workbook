In this tutorial, we will learn about how to assemble the transcriptome from RNA-Seq reads and calculate differential expression.

Command to start Trinity


```
Trinity --seqType fq --max_memory 32G --CPU 16 --trimmomatic --normalize_by_read_set --output TrinityOut --left all_left_R1.gz --right all_right_R2.gz &
```



--seqType Specify fq for fastq files or fa for fasta files