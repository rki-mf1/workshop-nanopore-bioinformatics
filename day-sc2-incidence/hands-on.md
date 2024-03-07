# Workshop: SARS-CoV-2 incidence estimation

We will do genome-based incidence estimation using a small example SARS-CoV-2 genome set from Australia and [GInPipe](https://github.com/KleistLab/GInPipe).

See also the corresponding [Publication](https://www.nature.com/articles/s41467-021-26267-y).

First, download the example data:

```bash
wget --no-check-certificate https://osf.io/d7r49/download -O incidence-estimation-australia.zip
unzip incidence-estimation-australia.zip
# check the content of the folder, you will see that the FASTA file is compressed with xz - uncompress it!
# This might take a bit, xz is strong in compressing similar strings
cd incidence-estimation-australia
xzcat gisaid_VIC_2020.fasta.xz > gisaid_VIC_2020.fasta
```

**TODO for P5**

* install GInPipe & dependencies
* prepare input data
* run it
