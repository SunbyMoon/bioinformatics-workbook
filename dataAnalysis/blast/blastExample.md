
# Blast Example

In this tutorial, we will perform translational genomics.  We will take  a gene from one genome and identify its location in another using Blast.

The background reading for this tutorial is this paper [A novel C-type lectin gene is a strong candidate gene for Benedenia disease resistance in Japanese yellowtail, Seriola quinqueradiata](https://www.sciencedirect.com/science/article/pii/S0145305X17301787?via%3Dihub) by Masatoshi Nakamoto.

 The authors identified a gene (C-lectin) in a fish (Seriola quinqueradiata) that is associated with resistance to a parasite (Benedenia) .


### Download the Seriola rivoliana genome from NCBI
Here, I went to [NCBI.nlm.nih.gov](https://www.ncbi.nlm.nih.gov/) and searched for Seriola rivoliana then clicked on the Assembly link under Genomes. The download link can be found on the right hand side under ```Download the GenBank assembly```.

```
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/002/994/505/GCA_002994505.1_ASM299450v1/GCA_002994505.1_ASM299450v1_genomic.fna.gz

#unzip
gunzip GCA_002994505.1_ASM299450v1_genomic.fna.gz
```

### Obtain the gene sequence

Manually type out the C-lectin gene in the Paper and add a fasta header.  This is just the first part of the gene starting from the start site.

```
>C-lectin
actgctgttgaggacactgctgttgaggaaactgctgttgaagacactgctgttggaggaactgctgttgaggaaactgctgttgaagacactgctgttgaggaaactgctgttgaggaactgccgttgaggaaactgctgttgaggacactgctgttgaggacacggctgttgaggaca
```
Put this into a file using nano or vim

```
vim benediniaGene.fasta
```


### Make a dna blast database of the genomes

```
makeblastdb -in GCA_002994505.1_ASM299450v1_genomic.fna -dbtype nucl -input_type fasta -out SerRivdb
```

output will look something like this.

```
Building a new DB, current time: 08/03/2018 15:32:36
New DB name:   /pylon5/mc48o5p/severin/Seriola/Rivoliana/SerRivdb
New DB title:  GCA_002994505.1_ASM299450v1_genomic.fna
Sequence type: Nucleotide
Keep MBits: T
Maximum file size: 1000000000B
Adding sequences from FASTA; added 1343 sequences in 5.96571 seconds.
```

### Blast gene to genome

There are four programs in blast that you can choose from.


|Query Type	|Database Type	|BLAST Program|
|:------------- |:-------------|:-----|
|Nucleotide	|Nucleotide	|**blastn:** Search a nucleotide database using a nucleotide query.|
|Nucleotide |Protein	|**blastx:** Search protein database using a translated nucleotide query.|
|Protein	|Nucleotide	|**tblastn:** Search translated nucleotide database using a protein query.|
|Protein | Protein	|**blastp:** Search protein database using a protein query.|

In this case, we are blasting a protein query against a nucleotide database so we need the third option or tblastn.

The most basic use of blast is as follows
```
tblastn -db SerRivdb -query benedeniaGene.fasta  -out blastout.txt

```
Parameters
* -db = genome reference database generated by makeblastdb and the reference fasta file.
* -query = fasta file containing the sequences you want to blast (map) to the reference
* -out = file that you want the results to be written to.

**OUTPUT Snippet**

```
> PVUN01001342.1 Seriola rivoliana isolate HWSR04 Scaffold_1308,
whole genome shotgun sequence
Length=25273714

 Score = 72.4 bits (176),  Expect(2) = 3e-21, Method: Compositional matrix adjust.
 Identities = 32/36 (89%), Positives = 33/36 (92%), Gaps = 0/36 (0%)
 Frame = -3

Query  214      YFQGEWRWEDGSRFDYSNWDTPRSTAYYQCLLLNSQ  249
                + Q EWRWEDGSRFDYSNWDTP STAYYQCLLLNSQ
Sbjct  2542610  FLQDEWRWEDGSRFDYSNWDTPSSTAYYQCLLLNSQ  2542503


 Score = 51.2 bits (121),  Expect(2) = 3e-21, Method: Compositional matrix adjust.
 Identities = 22/25 (88%), Positives = 23/25 (92%), Gaps = 0/25 (0%)
 Frame = -2

Query  250      VSMGWSNNGCNMNFPFVCQVRQLNC  274
                VS GWSNNGCNM FPFVCQVRQL+C
Sbjct  2542419  VSKGWSNNGCNMRFPFVCQVRQLDC  2542345


 Score = 85.5 bits (210),  Expect = 3e-17, Method: Compositional matrix adjust.
 Identities = 39/54 (72%), Positives = 44/54 (81%), Gaps = 0/54 (0%)
 Frame = -3

Query  163      LANHPDSWANAERFCASYEGSLASVGSIWEYNFLQRMVKTGGHAFAWIGGYYFQ  216
                + N   S    +RFCAS++GSLASV SIWEYNFLQRMVKTGGH FAWIGGYYF+
Sbjct  2543138  IKNKSSSPVVLQRFCASFDGSLASVRSIWEYNFLQRMVKTGGHKFAWIGGYYFE  2542977

```

We can also change the output format to give more concise results

```
tblastn -db SerRivdb -query benediniaGene.fasta  -outfmt '6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore qcovs salltitles' -num_threads 16 -out blastout2  .txt
```
Parameters
* -db = genome reference database generated by makeblastdb and the reference fasta file.
* -query = fasta file containing the sequences you want to blast (map) to the reference
* -out = file that you want the results to be written to.
* -outfmt = this is where you can specify the fields you wish to output int he file. see tblastn --help for more information about this field

For this example we are outputing querySeqId SampleSeqID PercentIdentity AlignmentLength, mismatches, gapopen querystart queryend, samplestart sampleend, evalue, bitscore, querycovs subjectalltitles.

There are several important numbers to look for in a blast result but the main ones are evalue, percent identity and alignment length.  Below you can use a pipe and awk to require that the output has at least 50% identity.

```
more blastout2.txt  | awk '$3>50'
C-LECTIN        PVUN01001342.1  88.889  36      4       0       214     249     2542610 2542503 3.15e-21        72.4    49      PVUN01001342.1 Seriola rivoliana isolate HWSR04 Scaffold_1308, whole genome shotgun sequence
C-LECTIN        PVUN01001342.1  88.000  25      3       0       250     274     2542419 2542345 3.15e-21        51.2    49      PVUN01001342.1 Seriola rivoliana isolate HWSR04 Scaffold_1308, whole genome shotgun sequence
C-LECTIN        PVUN01001342.1  72.222  54      15      0       163     216     2543138 2542977 3.04e-17        85.5    49      PVUN01001342.1 Seriola rivoliana isolate HWSR04 Scaffold_1308, whole genome shotgun sequence
C-LECTIN        PVUN01001342.1    85.714  35      5       0       140     174     2543329 2543225 4.08e-11        67.0    49      PVUN01001342.1 Seriola rivoliana isolate HWSR04 Scaffold_1308, whole genome shotgun sequence
```

When we do that we see that they all fall on this one chromosome (PVUN01001342.1) at position 2,542,610.  The subject all titles is very handy when you have a lot of information in the fasta header lines of the reference genome.  If this is not included then all you get is the first word in the header ie (PVUN01001342.1) rather than (PVUN01001342.1 Seriola rivoliana isolate HWSR04 Scaffold_1308, whole genome shotgun sequence).  In this case it was rather important because there is a genome browser that uses the older scaffold names.

If we go this region in the Jbrowse on [Serioladb.org](https://www.serioladb.org/jbrowse/?data=https%3A%2F%2Fwww.serioladb.org%2Fapi%2Fjbrowse%2F14&loc=Scaffold_1308%3A2538561..2546660&tracks=refseq%2Cgenes%2Ctranscripts&highlight=)

You will find this gene Serriv.G0000274 with this sequence.

```
ATGTTTTATGGTTACCTGTGTCAGGTGACTCAGTGTTCATGTGCAGGTATAAAGAGCAGAGAGCATCGCTCACCTTCCAGACACTTGACCGGCTCTTCACACTTACCTGACTGCTGCTTCTCTCTGTAAGTCAAATTTGTGTTACTGTTGTCTGGATCTGTTACACATTGCACTGATGAACATTGTGTTGTCATGACTTAATGAAATGTGAAACTTTGCCTCCCTCCAGATCTTCATTGCTTTTCATCATGAAGACTCTTCTGATTCTGTCTGTTGTCCTCTGTACTGCTCTCTCTGTCTGGGCTGCAGCAGGTGAGACACAACACAGATAGATTATTTCAGTAACACATGTGAAACAGCTGTCATATGTTTAGGGCTGGGGATCAAACTTCCATCCTCTAATTGATCGACTGGTTTGACATCATCCAGGATAAAAAAAAATGTCCTGATTTGATACCTGAGTAAATCTGATCAGCGTCAGTGAGCCAGTCAGCATGCAGCATGCTAGGATGTAATAATGCTGGTCATTGGCTGTTTATTTACACATTGAAACTTCAAAATAAAACTACAGTAAAACTAAAATTTGCATCATGATGAGGAAAAAAGAGGTGAATATGGGTCATGGGAGGAAGGAAGGAGGGTTTAGGACATCCCAGGTTACACGTAGTCTTTGTTCCTGAACTCAGTAGAACTTGTTTAGCGCAGTGTTTAACTATATAAATCTGATCTACTTCACTGTCTTTCCCTCAGTTGTTCCAGCTGAAGCTGCTACTGTGCAACTGGGAGACAAAGCTGCACCTGAACCAGGTGATGGGCACATCTTATTGTAAATCTTTCCATCTTGAGTGAGATCACAGACCACCACAGTGTGTGTTACTATTTATGTGTCTCAAACTGCAGCCATCATGCTGACGTGTGAAGTGCTTTGTATCATCACACGGGTGTTTCTCATTGTCTTTGTGAATCATTGCTGAAGTGTCTCAGCACAGACAACAGCACTGGTTTGAGCCTGAGTCAGTGATGGGTACACCTCAGCTCTACATCACGTACAATAGAACAAGTATCAACTACTGCTGCTTTATTAGTGATCTGTACAAGCTGCACTGCAAACTGGTATAAACTGGACCATTGAAATGATTTACTGTCAGTCTTTCTATCAGCCATGTGTCTAAAGAACATAGTGATAATGAATGATTTCTGACATGATCAGTGAAGCTCACAGACCTTGTGTTGCTGGTTTTACAGAGGCAGTTAATGACACTGCTGTTGAGGAGACTGCTGTTGAGGAGACTGCTGTTGAGGAGACTGCTGTTGAGGAGACTGCTGTTGAAGACACTGCTGTTGCTGCTGGCAGACCAGCTGTACTCCGTCAAGGTGGGAAAATCACACAATGCACTGCTGTCACGGTAACACACAAGGCTGCTGCTGGACACTGGCAGCCATTTTGTTAATTACCTATTCTCATGATTTGTTCCCCTCTTTGTCTCCAGCTCGTTTCAGTTTCTGCCTCGATGGTTGGCAGAGTTTCTCTGGCAAGTGCTACTTCTTAGCCAACCATCCTGACAGCTGGGCAAATGCCGAGGTAATAACATCACATTCATAATGGTAGAACAGGAAAAGTTTGGAGCTCTTTCTCAGCTTCATTTGAGTGCAGTATATAATTTCGAAATAAAAAACAAATCCTCCTCTCCTGTCGTCCTGCAGAGATTCTGTGCTAGCTTCGATGGCAGTCTGGCCTCAGTCCGCAGCATCTGGGAGTATAACTTCCTCCAGCGCATGGTCAAGACTGGTGGCCACAAGTTTGCCTGGATCGGAGGTTACTACTTTGAGGTATGTTCAAACATTTAAAATGCCTGTAGCTTGTCAATGGATTTCAGTGACATTTGGCAGAAAGCTACGGAACATGTTCACATCACTCATGTGTGTATGAGTGGTTTCCTGTTTGAACTTCAGCTCACAACAGGAACAGAAGCAGATGAACAAAGTTTGTGTTTTTTCAGCTCATTCATAAAGAATTGGGTTAAAGTCAGTTTAAAGCTCATGTTTGTATAATTTGACATGAAGTTTGGTCAATGGTTAAATGATTCTGTGGCATTATGTTTTGTTTTTAAGGTTAGTAAAAATGGTGCAGTCTGTGTTTGTATTGAGTCATAAATGTTGAAATGCCACACAAAGGCTCTTTAAACTAAAAGTGACTTCCTCCAGGATGAATGGAGATGGGAAGATGGCTCACGGTTTGACTACTCCAACTGGGATACACCAAGCTCCACTGCATACTACCAGTGCCTGCTGCTCAACTCTCAAGGTAAGAGAGACCTGTGTCTACCATCACATTTGATGATGATGGTGATGATGATGATGATGATGATGATGATGATGCTTTTGCAGTGTCCAAGGGCTGGTCCAACAATGGCTGCAACATGCGCTTCCCATTCGTGTGTCAGGTCAGACAGCTAGACTGTTAG
```

### Verify it is the gene we want

To verify I took the sequence above and blasted it against the [nr database](https://blast.ncbi.nlm.nih.gov/Blast.cgi  ) which resulted in this.

```
Seriola quinqueradiata DNA, linkage group 2, BAC clone: 013_p20
```

The important part here is that it matches linkage group 2 in Seriola quinqueradiata which is where the QTL is reported in the paper.