# Kraken analysis on nanopore sequences and Illumina assemblies

### Database
```
kraken-build --download-taxonomy --db db_20180202
kraken-build --download-library bacteria --db db_20180202/
kraken-build --download-library viruses --db db_20180202/
kraken-build --download-library viral --db db_20180202/
kraken-build --download-library plasmid --db db_20180202/

wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/002/897/155/GCA_002897155.1_Rir_HGAP_ii_V2/GCA_002897155.1_Rir_HGAP_ii_V2_genomic.fna.gz
gunzip GCA_002897155.1_Rir_HGAP_ii_V2_genomic.fna.gz

kraken-build --add-to-library GCA_002897155.1_Rir_HGAP_ii_V2_genomic.fna --db db_20180202/
```

Build the database, needs a big node on Uppmax:
```
#!/bin/bash -l
#SBATCH -A b2017181
#SBATCH -p node 
#SBATCH -n 16
#SBATCH -C mem256GB
#SBATCH -t 12:00:00
#SBATCH -J kraken_build_db

module load bioinfo-tools

module load Kraken

kraken-build --build --threads 16 --db db_20170202/
```

### Analysis

#### Nanopore
```
kraken --threads 8 --db /Users/merce/Documents/kraken/kraken_db/db_20180202 --fastq-input /Users/merce/Documents/merce/nanopore/Nanopore/2018-01-24/reads/20180118_1710_CC4-2-3_18-1-17/fastq/pass/fastq_Cc4_nanopore.fastq > Cc4_kraken_db20180202.kraken

kraken-report --db /Users/merce/Documents/kraken/kraken_db/db_20180202 Cc4_kraken_db20180202.kraken  > Cc4_kraken_db20180202.report

kraken-translate --db /Users/merce/Documents/kraken/kraken_db/db_20180202 Cc4_kraken_db20180202.kraken  > Cc4_kraken_db20180202.labels
```

##### Results:
29149 sequences (213.48 Mbp) processed in 9271.116s (0.2 Kseq/m, 1.38 Mbp/m).
  8902 sequences classified (30.54%)
  20247 sequences unclassified (69.46%)



#### Assembly
```
kraken --threads 8 --db /Users/merce/Documents/kraken/kraken_db/db_20180202 /Users/merce/Documents/merce/nanopore/cclaro_assembly.fasta > Cclaro_bigassembly_kraken_db20180202.kraken

kraken-translate --db /Users/merce/Documents/kraken/kraken_db/db_20180202 Cclaro_bigassembly_kraken_db20180202.kraken  > Cclaro_bigassembly_kraken_db20180202.labels

kraken-report --db /Users/merce/Documents/kraken/kraken_db/db_20180202 Cclaro_bigassembly_kraken_db20180202.kraken  > Cclaro_bigassembly_kraken_db20180202.report
```


##### Results:
44841 sequences (241.94 Mbp) processed in 13.919s (193.3 Kseq/m, 1042.91 Mbp/m).
  12787 sequences classified (28.52%)
  32054 sequences unclassified (71.48%)







