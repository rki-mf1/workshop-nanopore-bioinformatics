# Workshop: SARS-CoV-2 mutation profile

## Mutation profiling with covsonar

We will use [covsonar](https://github.com/rki-mf1/covsonar) to create a database of mutation profiles and do some queries. 

Check the covsonar GitHub README and install the tool in a conda/mamba environment. 

```bash
# download the repository to the current working directory using git 
git clone https://github.com/rki-mf1/covsonar.git
# build the custom software environment using conda or mamba [recommended]
conda env create -n sonar -f covsonar/sonar.env.yml
# activate the conda evironment if built 
conda activate sonar
```

Let's get some example SARS-CoV-2 genomes:

```bash
# get example data, a collection of different SARS-CoV-2 lineages, full genomes
wget --no-check-certificate https://osf.io/wpk75/download -O sc2-genomes-diff-lineages.tar.gz
# extract the archive
tar zxvf sc2-genomes-diff-lineages.tar.gz
# create a multi FASTA file
cat sc2-genomes-diff-lineages/*.fasta > sc2-genomes-diff-lineages.fasta
```

Create a new covsonar DB:

```bash
# adding all sequences from 'sc2-genomes-diff-lineages.fasta' to database 'mydb'
# using four cpus (the database file will be created if it does not exist)
covsonar/sonar.py add -f sc2-genomes-diff-lineages.fasta --db mydb --cpus 4
```

Adding metadata information:

```bash
# get metadata TSV
wget --no-check-certificate https://osf.io/fwcv2/download -O sc2-genomes-diff-lineages.tsv

# importing lineage annotations and sampling dates from a custom TSV file to database 'mydb'
covsonar/sonar.py update --tsv sc2-genomes-diff-lineages.tsv --fields accession=acc date=sampling lineage=lineage --db mydb
```

Query genome sequences based on profiles and metadata information:

```bash
covsonar/sonar.py match --db mydb --include S:N501Y
covsonar/sonar.py match --db mydb --lineage B.1.1.7
covsonar/sonar.py match --db mydb --lineage B.1.1.7 --count
covsonar/sonar.py match --db mydb --lineage B.1.1.7 --include S:del:68:3 --count
covsonar/sonar.py match --db mydb --date 2022-01-01:2023-01-01 --count
```