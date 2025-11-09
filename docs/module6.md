# CBW Module 6 Lab
*Developed by Jimmy Liu and William Hsiao*

### Introduction

In this integrative assignment, you will be applying some of the genomic epidemiology analysis methods covered in this workshop. The focus of the assignment will be on *Salmonella enterica*, an enteric pathogen that primarily spreads by human consumption of contaminated foods in Canada and the United States. Here, you will examine isolates of *Salmonella* serovar Heidelberg from three epidemiologically distinct foodborne outbreaks that occurred in Quebec, Canada between 2012-2014. For more detailed background on how the outbreaks happened, you are encouraged to read over the original publication by [Bekal et al. (2014)](https://pubmed.ncbi.nlm.nih.gov/26582830/). You will be analyzing the whole-genome sequencing (WGS) data generated from the study to investigate these foodborne outbreaks. Briefly, you will identify core genome single nucleotide variants (SNVs) from pre-assembled genomes, construct a core genome SNV phylogenetic tree and infer the evolutionary relationships of the isolates. In addition, you will annotate the bacterial genomes to detect the presence of various genetic features from this *Salmonella* outbreak dataset.

### Learning Objectives
By the end of the session, you will be able to:

- Describe how colored de Bruijn graphs (cDBGs) represent sequence variation across genomes.

- Use Bifrost to construct and query a bacterial pangenome graph.

- Visualize graph topology in Bandage and interpret structural differences.

- Explain how k-mer length impacts graph complexity and biological resolution.

### Environment Setup
```bash
# Create working directory
mkdir pg_demo && cd pg_demo

# Activate tool environment
conda activate pg_tools
```

### Part 1: Understanding the structural layout of cDBG
#### Build colored de Bruijn graph (k=31)
```bash
bifrost build -r strain_A.fna,strain_B.fna,strain_C.fna \
              -k 31 -t 4 -o salmonella_k31

# Output:
#   salmonella_k31.gfa   → compacted DBG in GFA format
#   salmonella_k31.bfg_colors → color information
```

#### Visualize cDBG in Bandage
To visualize the output graph (.GFA) constructed by `Bifrost`, you will need `Bandage` installed on your own computer (Not on the server!). You can download pre-compiled binaries from [here](https://github.com/rrwick/Bandage/releases) (download and decompress the binary matching your computer OS). 

For Bandage to access the output files, the files need to be transferred to your local device, which can be done using `scp`

```bash
# copy GFA file remote location to local device
scp server:/pg_demo/salmonella_k31.gfa /path/to/local
```

### Part 2: Querying a pangenome graph
#### Find a Gene or Region of Interest

Suppose we have a small gene fragment from strain A:

```bash
grep -A1 "geneX" strain_A.fna | head -n 2 > query.fa
```

#### Query the Graph

```bash
bifrost query -g salmonella_k31.gfa -c salmonella_k31.bfg_colors \ 
              -r query.fa -o query_result.tsv
```

Output interpretation (`query_result.tsv`):

```bash
Query	Color	Presence
geneX	A	1	Present
geneX	B	0	Absent
geneX	C	1	Present
```

### Part 3: Impact of k-mer length on graph topology

```bash
# Step 1 – Rebuild with Smaller k (=21)
bifrost build -r strain_A.fna,strain_B.fna,strain_C.fna \
              -k 21 -t 4 -o salmonella_k21
```

```bash
# Step 2 – Rebuild with Larger k (=51)
bifrost build -r strain_A.fna,strain_B.fna,strain_C.fna \
              -k 51 -t 4 -o salmonella_k51
```

Open `salmonella_k21.gfa` and `salmonella_k51.gfa` in Bandage.

Record your observations on changes in graph topology according to this table:

| k-mer size | Graph topology |
| :---: | :------: |
| k = 21 | |
| k = 31 | |
| k = 51 | |

### Wrap-Up

Let's now summarize the use the cDBG for comparative genomics and the exploration of genetic vairations across microbial pangenomes.

| Strengths | Weaknesses |
| --- | --- |
| Captures SNPs, indels, gene gain/loss without reference | Visualization becomes complex for many genomes
| Unified data structure to encode pan-genome variations |	Parameter choice (k, abundance) critical |
| Efficient querying across 1000s of genomes via colored graphs | Interpretation needs experience |
| Unitigs yield greater specificity than canonical k-mers | Highly sensitive to assembly fragmentation |

**Closing reflection:**
- How might long read sequencing impact the quality of cdBG construction?





