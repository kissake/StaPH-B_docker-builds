# MIDAS container

Main tool: [MIDAS](https://github.com/snayfach/MIDAS)

An integrated pipeline for estimating strain-level genomic variation from metagenomic data.

Additional tools:

* python x
* samtools 1.4
* bowtie2 2.3.2
* vsearch 2.14.2
* hmmer 3.1b2

## Example commands

```bash
# Test with supplied E. coli data
docker run --rm -u $(id -u):$(id -g) -v ${PWD}:/data staphb/spades:latest run_midas.py --help

# paired-end Illumina reads are in the $PWD
$ ls 
SRR7062227_1.fastq.gz  SRR7062227_2.fastq.gz

# run MIDAS with your own data (broken into two lines for readability)
docker run --rm -u $(id -u):$(id -g) -v ${PWD}:/data staphb/midas:latest \
  run_midas.py species /path/to/outdir -1 /path/to/reads_1.fq.gz -2 /path/to/reads_2.fq.gz
```

View full `spades` help options: `docker run --rm -u $(id -u):$(id -g) -v ${PWD}:/data staphb/spades:latest spades.py --help`
