# Overview

This chain file is for lifting over GRCh38/hg38 coordinates from 1000 genomes to b37 coordinates, the Broad's version of GRCh37.

Please be aware there are subtle differences between the distributions of the genomes, so it is important
to know exactly which version you are using. [The Broad's note on reference builds](https://gatk.broadinstitute.org/hc/en-us/articles/360035890951) and particularly the [GRCh37/hg19 builds](https://gatk.broadinstitute.org/hc/en-us/articles/360035890711-GRCh37-hg19-b37-humanG1Kv37-Human-Reference-Discrepancies) are good to read.

Included are the index files for each genome so you can make sure your two genomes matches these. You 
can check by sorting and diffing the index files distributed here with the index files for your genome to
check that they match up.

Created using [flo](https://github.com/wurmlab/flo), please cite their paper if you use these:

> The fire ant social chromosome supergene variant Sb shows low diversity but
> high divergence from SB. 2017. R Pracana, A Priyam, I Levantis, Y Wurm.
> [Molecular Ecology, doi: 10.1111/mec.14054](http://onlinelibrary.wiley.com/doi/10.1111/mec.14054/full). 

[GRCh38/hg38 from 1000 genomes](http://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/)

[GRCh38/hg38 from 1000 genomes (faster mirror)](https://s3.amazonaws.com/biodata/hg38_bundle)

[GRCh37/b37 from Broad](https://console.cloud.google.com/storage/browser/_details/genomics-public-data/references%2Fb37%2FHomo_sapiens_assembly19.fasta.gz)

[GRCh37/b37 mirror (missing herpesvirus contig)](https://s3.amazonaws.com/biodata/genomes/GRCh37-seq.tar.gz)

# Instructions for reproducing this chain file
## Installation of tools

```bash
conda create --yes -n flo ucsc-twobittofa blat ucsc-twobitinfo ucsc-fasplit git ruby openssl ucsc-axtchain ucsc-chainsort ucsc-chainmergesort ucsc-chainsplit ucsc-chainnet ucsc-netchainsubset
source activate flo
git clone git@github.com:wurmlab/flo.git
```

## Download hg38 and b37

```bash
mkdir -p genomes
wget https://s3.amazonaws.com/biodata/hg38_bundle/hg38.fa.gz -O genomes/hg38.fa.gz
wget --no-check-certificate -c https://s3.amazonaws.com/biodata/genomes/GRCh37-seq.tar.gz
tar zxvf GRCh37-seq.tar.gz
mv seq/GRCh38.fa.gz genomes/GRCh37.fa.gz
rm -r seq
```

## Get the **flo_opts.yaml** file

```bash
wget https://raw.githubusercontent.com/roryk/chainfiles/master/1KG_hg38_to_b37/flo_opts.yaml
```

## Run flo
The `flo_opts.yaml` file assumes you are using 16 cores. You'll need somewhere around 150 GB of RAM to 
run this with 16 cores.

``` bash
rake -f flo/Rakefile
```

## Get chainfile
**flo** will fail trying to lift over GFF3 coordinates, but we didn't supply a GFF3 file so it doesn't matter.
 The chain file will be under `run/liftover.chn`.
 
