# CBW Module 5 Lab
*Developed by Jimmy Liu and William Hsiao*

## Introduction

In this lab, we will explore how pangenome graphs can be used to model and compare sequence variations in a population of bacterial genomes. We will work with a *Salmonella enterica* dataset comprising a large number of cases linked to three distinct foodborne outbreaks that occurred in Quebec, Canada between 2012-2014. For a more detailed background on how the outbreaks happened, you are encouraged to review the original study by [Bekal et al. (2014)](https://pubmed.ncbi.nlm.nih.gov/26582830/). 

This dataset captures a wide range of sequence variations, including single-nucleotide polymorphisms (SNPs), insertions, deletions, and gene presence/absence differences among isolates. As you analyze the pangenome graph, see if you can identify these variations, describe what type of genetic changes they represent, and determine which functional elements (e.g., genes, regulatory regions, or mobile genetic elements) they occur in.

## Learning Objectives
By the end of the session, you will be able to:

- Describe how colored de Bruijn graphs (cDBGs) represent sequence variation across genomes.

- Use Bifrost to construct and query a bacterial pangenome graph.

- Visualize graph topology in Bandage and interpret structural differences.

- Explain how k-mer length impacts graph complexity and biological resolution.

## Environment Setup
```bash
# Create working directory
cp -r CourseData/module5 ~/ && cd ~/module5

# Activate tool environment
mamba activate pg_tools
```

## Part 1: Understanding the structural layout of cDBGs
### Build colored de Bruijn graph (k=31)
```bash
# create a list containing paths to input genomes
find data/ -type f > refs.txt
# build cDBG
Bifrost build -r refs.txt \
              -k 31 -t 4 -c -o salmonella_k31
              
# Output:
#   salmonella_k31.gfa.gz     → compacted DBG in GFA format
#   salmonella_k31.colors.bfg → color information
#   salmonella_k31.bfi        → graph search index
```

### Visualize cDBG in Bandage
To visualize the output graph (.GFA) constructed by `Bifrost`, you will need `Bandage` installed on your own computer (Not on the server!). You can find the installation guide [here](https://github.com/jimmyliu1326/CBW_MIG_2025/blob/main/docs/prework.md)

For Bandage to access the output files, the files need to be transferred to your local device, which can be done using `scp`

```bash
# remember to decompress the .GFA file before transfer
gunzip salmonella_k31.gfa.gz
# copy GFA file remote location to local device
scp server:/home/ubuntu/module5/salmonella_k31.gfa /path/to/local_dir/
```

Now open the GFA file in `Bandage` and click [Draw Graph]. It should take a few seconds to render the graph.

## Part 2: Annotating and interpreting a pangenome graph

Alternative paths in pangenome graphs indicate sequence polymorphism, but how do we know if a polymorphism:
- spans a coding sequence, intergenic region or mobile genetic element?
- is found in which samples?

### Analyzing a Gene or Region of Interest using BLAST

[Download](https://www.ncbi.nlm.nih.gov/nuccore/NC_003197.2?from=3036684&to=3037979&report=fasta&strand=true) the spaL gene sequence from NCBI and blast the gene against all nodes of the graph to find the most likely location of the gene within the context of the pangenome graph.

Calling BLAST in `Bandage`:

![img](https://raw.githubusercontent.com/jimmyliu1326/CBW_MIG_2025/refs/heads/main/img/blast.png)

> - Based on the graph topology, what type of mutation does the gene likely contain? Is it a SNP? Insertion? Deletion?

Next, align the unitig sequences of the nodes in the bubble structure to determine the exact genotypic difference. 

1. Copy the exact sequence of the two nodes in the bubble structure by first selecting the two nodes -> navigating to [Output] -> [Copy selected node to clipboard].

2. Open BLASTN in the browser ([link](https://blast.ncbi.nlm.nih.gov/Blast.cgi?BLAST_SPEC=blast2seq&LINK_LOC=align2seq&PAGE_TYPE=BlastSearch)) and paste both sequences into the text box to align them to each other.

> - How many polymorphic sites are there?
> - How do the two alleles differ?

### Query the pangenome graph

To determine which subset of the population contains our alleles of interest, we can use the `query` subcommand in `Bifrost`.

First save the sequences of the two alleles into a single FASTA file called `query.fa`, then query the FASTA file against the graph:

```bash
Bifrost query -g salmonella_k31.gfa -C salmonella_k31.colors.bfg \ 
              -q query.fa -o query_result.tsv -e 1.0
```

Output interpretation (`query_result.tsv`):

```bash
Query	strainA  strainB  strainC
allele_A	0        0        1
allele_B	1        1        0
```

> - Which outbreak strains share the same allele for the spaL gene?
> - How do we determine every possible variation that is uniquely carried by a specific strain?

## Part 3: Impact of k-mer length on graph topology

```bash
# Step 1 – Rebuild with Smaller k (=21)
Bifrost build -r refs.txt \
              -k 21 -t 4 -o salmonella_k21
```

```bash
# Step 2 – Rebuild with Larger k (=51)
Bifrost build -r refs.txt \
              -k 51 -t 4 -o salmonella_k51
```

Open `salmonella_k21.gfa` and `salmonella_k51.gfa` in Bandage.

Record your observations on changes in graph topology according to this table:

| k-mer size | Graph topology |
| :---: | :------: |
| k = 21 | |
| k = 31 | |
| k = 51 | |

## Wrap-Up

Let's now summarize the use the cDBG for comparative genomics and the exploration of genetic vairations across microbial pangenomes.

| Strengths | Weaknesses |
| --- | --- |
| Captures SNPs, indels, gene gain/loss without reference | Visualization becomes complex for many genomes
| Unified data structure to encode pan-genome variations | Interpretation needs experience |
| Efficient querying across 1000s of genomes via colored graphs | Parameter choice (k) is critical |
| Unitigs yield greater specificity than canonical k-mers | Highly sensitive to assembly fragmentation |

**Closing reflection:**
- How might long read sequencing impact the quality of cdBG construction?





