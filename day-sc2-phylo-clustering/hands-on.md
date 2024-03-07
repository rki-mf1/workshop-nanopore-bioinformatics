# Workshop: SARS-CoV-2 phylogeny & outbreak investigation

## Alignment and tree

In this part, we will calculate a multiple sequence alignment (MSA) and a phylogenetic tree. We will do this using the Linux system and command line interfance and online tools for tree visualization. 

The tools we will use are listed here and will be discussed in detail below:

* `MAFFT`
* `IQTree`
* `president`

__Install all tools__
```bash
# config some channels, this might be already done.
# basically helps to not explicitly type the channels
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge

# now we create a new environment and install all tools
mamba create -n tree president mafft iqtree jalview

# activate the env
conda activate tree
```

### Example data
```bash
# get example data, a collection of different SARS-CoV-2 lineages, full genomes
wget --no-check-certificate https://osf.io/wpk75/download -O sc2-genomes-diff-lineages.tar.gz
# extract the archive
tar zxvf sc2-genomes-diff-lineages.tar.gz
```

### Multiple sequence alignment

* `MAFFT`

```bash
# first we need a multiple FASTA file, which we can for example generate
# by 'cat'ing together single FASTA files, like the ones in the example-data folder
cat sc2-genomes-diff-lineages/*.fasta > all.fasta

# now we can calculate the alignment
mafft --thread 4 all.fasta > alignment.fasta
```

__Task:__ Check the `alignment.fasta` - do you see mismatches? Gaps? You can for example use `jalview` to look at the alignment! The tool is also installed in your conda env.

### Phylogenetic reconstruction

* `IQTree`

```bash
# simple usage (there are many parameters though!)
iqtree -nt 4 -s alignment.fasta --prefix phylo

# first look at the output, scroll a bit to see a tree in ASCII format
cat phylo.iqtree

# the actual tree is stored in the so-called newick format:
cat phylo.treefile
```

### Tree visualization

* `IROKI`

Go to [https://www.iroki.net/](https://www.iroki.net/) and upload a tree file in `newick` format, e.g. `phylo.treefile`. 


## Clustering 

Now we want to do a clustering of SARS-CoV-2 sequences based on their mutation profile using [https://github.com/rki-mf1/breakfast](https://github.com/rki-mf1/breakfast).

### Example data
```bash
# get example data, a collection of different SARS-CoV-2 lineages, full genomes
wget --no-check-certificate https://osf.io/kxasc/download -O breakfast-clustering-data.tar.gz
# extract the archive
tar zxvf breakfast-clustering-data.tar.gz
```

**TODO Denis/Matt**

* install breakfast
* prepare input data (does probably involve covsonar? Or does the example data already have a cs DB?)
* run the tool 

