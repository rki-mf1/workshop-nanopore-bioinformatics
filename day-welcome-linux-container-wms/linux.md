# Workshop - Linux re-cap

## Short Linux and bash re-cap

* Linux/Bash basics + conda setup ([slides](https://docs.google.com/presentation/d/14xELo7lDbd-FYuy144ZDK1tV_ZBdBYun_COelrKYWps/edit?usp=sharing))
* Another good resource: [Introduction to the UNIX command line](https://ngs-docs.github.io/2021-august-remote-computing/introduction-to-the-unix-command-line.html)
* Cheat sheet for Bash: [github.com/RehanSaeed/Bash-Cheat-Sheet](https://github.com/RehanSaeed/Bash-Cheat-Sheet)

### Small Cheat Sheet:
```bash
# Print your user name
echo $USER
# change directory to your user home directory (all of these are the same)
cd /home/$USER
cd $HOME
cd ~  # <- the shortest version, I like this one
# show content of current directory
ls
# make a new directory called 'myfolder'
mkdir myfolder
# generate an empty file
touch some-file.txt
# show full content of a file
cat some-file.txt
# show first 10 lines of a file
head some-file.txt
# generate three emtpy files
touch file1.txt file2.txt file3.txt
# concatenate the content of files into a new file
cat file1.txt file2.txt file3.txt > file123.txt
# ... or via a wild card
cat file?.txt > file123.txt
# compress a file 
gzip file123.txt # this will generate a compressed file 'sample.fastq.gz'
# uncompress a gz file
gunzip file123.txt.gz


# make conda/mamba environment and activate it
mamba create -n nanoplot
conda activate nanoplot
mamba install nanoplot
# Attention, the above command will create a folder 'nanoplot' in your default path, e.g. ~/miniconda3/envs
# However, you can also specify any other folder, and which we will also do in the training:
mamba create -p envs/nanoplot
conda activate envs/nanoplot
mamba install nanoplot
# run a program
NanoPlot --help
```

### Practices and examples

Here are some example commands and things you can try on your own. Always remember:

* use _auto completion_ as often as possible, you can always use the _tab_/_tabulator_ key to get suggestions for a command you are typing and to auto-complete folder/file names and paths - it's much faster and less error prone! (preventing typos!)
* prevent whitespaces in all folder and file names! You can use whitespaces in general, but it will complicate your work on a Linux system! Use `-`, `_`, etc... instead, e.g. `new-file.txt`
* be always careful when you delete a folder or file! It's not as easy as on windows to get your data back!

Now, open a terminal and try the following commands.

```bash
# when opening a new terminal, you always start in your home directory

# the following command shows the current path you are located (remember the tree-like structure of folders on a linux system!)
pwd

# create a new directory
mkdir testdir

# change into that new directory
cd testdir

# check where you are located now
pwd

# generate a new empty file
touch genome.fasta

# list content of the current directory
ls

# list more details, in a human readable format
ls -lah

# write some content into that file
printf ">Sequence\nATCGTACGTACGTAC\n" > genome.fasta

# check content of the file
cat genome.fasta

# change to your home directory
#     ~ is a short version of /home/$USER
cd ~

# check again the content of the file you created
# now you have to type the full path to find the file! Use auto-complete! Here we use the so-called relative path
cat testdir/genome.fasta

# you can also use the absolute path
cat /home/$USER/testdir/genome.fasta

# Hint: $USER is a so-called variable. To see the content of a variable you can also use echo:
echo $USER

# in $USER your terminal stored the information about the current user running the session. You can also define your own variables, for example you could store the absolute path to your file in a variable for easier re-usage:
GENOME=/home/$USER/testdir/genome.fasta
cat $GENOME

# please notice that we always use a leading $ sign when we want to access the content of a variable! See the difference:
echo GENOME
echo $GENOME

# generate another file
touch genome2.fasta

# copy the file to the test folder
cp genome2.fasta testdir/

# list the content of the test folder
ls -lah testdir/

# remove the original file we just generated in your home dir 
rm genome2.fasta

# is it gone?
ls -lah

# however, remember we copied the file so a copy of the file we just deleted is still in the test folder
ls -lah testdir/
```


## (H)PC setup

If you are using your own (Linux) laptop or one provided for the workshops you are good to go. If you are using an RKI Windows laptop, you have to connect to the Linux High-Performance Cluster (HPC). You also need an account that you can request via an IT ticket. 

### HPC access

* Install `MobaXterm` via the RKI Software Kiosk
* Open `MobaXterm`
* Connect to the HPC login node, your instructors will tell you the name
    * Select "Session": "SSH"
    * "Remote host": "provided login node name" 
    * "Username": RKI account name

### HPC usage

* Detailed information on HPC infrastructure and usage can be found in the RKI Confluence, search for:
    * "HPC Aufbau"
    * "HPC Nutzung"
    * "HPC FAQ"
* Opening an interactive Shell
    * On the HPC we have login and compute nodes. We dont want to compute on login nodes.
    * An interactive shell is simply any shell process that you use to type commands, and get back output from those commands. That is, a shell with which you interact. We want to connect to a compute node for the workshop.
    * Open `MobaXterm`` and connect to one of the login nodes (ask instructors)

Opening an interactive shell on the RKI HPC:
```sh
#start an interactive bash session using the default ressources
srun --pty bash -i

#start an interactive bash session using 8 CPUs, 40GB RAM, 30GB HDD
srun --cpus-per-task=8 --mem=40GB --gres=local:30 --pty bash -i

#start an interactive bash session using 10 CPUs, 80GB RAM, 50GB HDD, 1GPU
srun --cpus-per-task=10 --mem=80GB --gres=local:50 --gpus=1 --pty bash -i

#IMPORTANT to free the blocked resources after our work is done close the interactive shell via:
exit
```
Due to competing requests it may take some time until the requested resources can be provided by the system. Therefore, wait patiently until the prompt appears. Reducing requested resources might help as well.


## Install mamba (if done already on your machine: skip)

* Mamba is a packaging manager that will help us to install bioinformatics tools and to handle their dependencies automatically
* Mamba works together with the conda package manager, and makes installing packages faster
* You will use the mamba command to create environments and install packages, and conda command for some other package management tasks like configuration and activating environments (yes it can be a bit confusing)
* __Hint:__ You can create as many environments as you want! It is often convenied to have separate environments for separate tasks, pipelines, or even tools 
* In the terminal enter:

```bash
# Switch to a directory with enough space
cd /scratch/$USER

# make a new folder called 'workshop'
mkdir workshop

# switch to this folder
cd workshop

# Download mamba installer
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-Linux-x86_64.sh"

# ATTENTION: the space in your home directory might be limited (e.g. 10 GB) and per default conda installs tools into ~/.conda/envs
# Thus, take care of your disk space!
# On the HPC you can take care of this by moving ~/.conda to /scratch and making a symlink from your home directory:
# mv ~/.conda /scratch/dot-conda
# ln -s /scratch/dot-conda ~/.conda

# Run installer
bash Mambaforge-Linux-x86_64
# Use space to scroll down the license agreement
# then type 'yes'
# accept the default install location with ENTER
# when asked whether to initialize mamba type 'yes'

# Now start a new shell or simply reload your current shell via
bash

# You should now be able to create environments, install tools and run them
```

* Set up mamba

```bash
# add repository channels for bioconda
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
```

* Create and activate a new conda environment

```bash
# -n parameter to specify the name
mamba create -p envs/workshop

# activate this environment
conda activate envs/workshop

# You should now see (workshop) at the start of each line.
# You switched from the default 'base' environment to the 'workshop' environment.
# Which is placed in a folder envs/workshop
```

Next: [Container & WMS](container-wms.md)