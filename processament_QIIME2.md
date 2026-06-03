---
title: "Estudi de la resiliència a llarg termini de la microbiota nasofaríngia i el seu paper en la transició de la colonització a la malaltia invasiva per bacteris patobionts"
author: "Gerard González Comino"
date: "03-06-2026"
---

# Activació de l'entorn QIIME 2

```
conda activate rachis-qiime2-2026.4
```

# Importació dels *runs* a QIIME 2

## Generació dels manifests

Generació dels manifests, que associen cada mostra amb les seves lectures *forward* (R1) i *reverse* (R2).

### *run* 1 prepandèmic (run1_pre)

```
cd 01_raw_data/fastq/run1_pre
echo -e "sample-id\tforward-absolute-filepath\treverse-absolute-filepath" > ../../manifests/manifest_run1_pre.tsv
for R1 in *_R1.fastq.gz; do
    SAMPLE="${R1%_R1.fastq.gz}"
    echo -e "${SAMPLE}\t$(pwd)/${R1}\t$(pwd)/${SAMPLE}_R2.fastq.gz" >> ../../manifests/manifest_run1_pre.tsv
done
cd -
```

### *run* 2 prepandèmic (run2_pre)

```
cd 01_raw_data/fastq/run2_pre
echo -e "sample-id\tforward-absolute-filepath\treverse-absolute-filepath" > ../../manifests/manifest_run2_pre.tsv
for R1 in *_R1.fastq.gz; do
    SAMPLE="${R1%_R1.fastq.gz}"
    echo -e "${SAMPLE}\t$(pwd)/${R1}\t$(pwd)/${SAMPLE}_R2.fastq.gz" >> ../../manifests/manifest_run2_pre.tsv
done
cd -
```

### *run* 3 prepandèmic (run3_pre)

```
cd 01_raw_data/fastq/run3_pre
echo -e "sample-id\tforward-absolute-filepath\treverse-absolute-filepath" > ../../manifests/manifest_run3_pre.tsv
for R1 in *_R1.fastq.gz; do
    SAMPLE="${R1%_R1.fastq.gz}"
    echo -e "${SAMPLE}\t$(pwd)/${R1}\t$(pwd)/${SAMPLE}_R2.fastq.gz" >> ../../manifests/manifest_run3_pre.tsv
done
cd -
```

### *run* 4 prepandèmic (run4_pre)

```
cd 01_raw_data/fastq/run4_pre
echo -e "sample-id\tforward-absolute-filepath\treverse-absolute-filepath" > ../../manifests/manifest_run4_pre.tsv
for R1 in *_R1.fastq.gz; do
    SAMPLE="${R1%_R1.fastq.gz}"
    echo -e "${SAMPLE}\t$(pwd)/${R1}\t$(pwd)/${SAMPLE}_R2.fastq.gz" >> ../../manifests/manifest_run4_pre.tsv
done
cd -
```

### *run* 1 postpandèmic (run1_post)

```
cd 01_raw_data/fastq/run1_post
echo -e "sample-id\tforward-absolute-filepath\treverse-absolute-filepath" > ../../manifests/manifest_run1_post.tsv
for R1 in *_R1_001.fastq.gz; do
    SAMPLE=$(echo "$R1" | sed -E 's/_S[0-9]+_R1_001\.fastq\.gz$//')
    R2="${R1/_R1_001/_R2_001}"
    echo -e "${SAMPLE}\t$(pwd)/${R1}\t$(pwd)/${R2}" >> ../../manifests/manifest_run1_post.tsv
done
cd -
```

## Importació i resum de qualitat

Importació de cada *run* com a artefacte de QIIME 2 i visualització del perfil de qualitat de les lectures.

### run1_pre

```
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path 01_raw_data/manifests/manifest_run1_pre.tsv \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path 02_qiime2/01_import/demux_run1_pre.qza ;
    
qiime demux summarize \
  --i-data 02_qiime2/01_import/demux_run1_pre.qza \
  --o-visualization 02_qiime2/01_import/demux_run1_pre.qzv ;
```

### run2_pre

```
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path 01_raw_data/manifests/manifest_run2_pre.tsv \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path 02_qiime2/01_import/demux_run2_pre.qza ;
    
qiime demux summarize \
  --i-data 02_qiime2/01_import/demux_run2_pre.qza \
  --o-visualization 02_qiime2/01_import/demux_run2_pre.qzv ;
```

### run3_pre

```
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path 01_raw_data/manifests/manifest_run3_pre.tsv \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path 02_qiime2/01_import/demux_run3_pre.qza ;
    
qiime demux summarize \
  --i-data 02_qiime2/01_import/demux_run3_pre.qza \
  --o-visualization 02_qiime2/01_import/demux_run3_pre.qzv ;
```

### run4_pre

```
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path 01_raw_data/manifests/manifest_run4_pre.tsv \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path 02_qiime2/01_import/demux_run4_pre.qza ;
    
qiime demux summarize \
  --i-data 02_qiime2/01_import/demux_run4_pre.qza \
  --o-visualization 02_qiime2/01_import/demux_run4_pre.qzv ;
```

### run1_post

```
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path 01_raw_data/manifests/manifest_run1_post.tsv \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path 02_qiime2/01_import/demux_run1_post.qza ;
    
qiime demux summarize \
  --i-data 02_qiime2/01_import/demux_run1_post.qza \
  --o-visualization 02_qiime2/01_import/demux_run1_post.qzv ;
```

# Eliminació dels encebadors V3-V4 i resum de qualitat

Eliminació dels encebadors de la regió V3-V4 i visualització del perfil de qualitat de les lectures resultants.

## run1_pre

```
qiime cutadapt trim-paired \
  --i-demultiplexed-sequences 02_qiime2/01_import/demux_run1_pre.qza \
  --p-front-f CCTACGGGNGGCWGCAG \
  --p-front-r GACTACHVGGGTATCTAATCC \
  --p-discard-untrimmed \
  --p-cores 4 \
  --o-trimmed-sequences 02_qiime2/02_cutadapt/demux_run1_pre_trimmed.qza ;

qiime demux summarize \
  --i-data 02_qiime2/02_cutadapt/demux_run1_pre_trimmed.qza \
  --o-visualization 02_qiime2/02_cutadapt/demux_run1_pre_trimmed.qzv ;
```

## run2_pre

```
qiime cutadapt trim-paired \
  --i-demultiplexed-sequences 02_qiime2/01_import/demux_run2_pre.qza \
  --p-front-f CCTACGGGNGGCWGCAG \
  --p-front-r GACTACHVGGGTATCTAATCC \
  --p-discard-untrimmed \
  --p-cores 4 \
  --o-trimmed-sequences 02_qiime2/02_cutadapt/demux_run2_pre_trimmed.qza ;

qiime demux summarize \
  --i-data 02_qiime2/02_cutadapt/demux_run2_pre_trimmed.qza \
  --o-visualization 02_qiime2/02_cutadapt/demux_run2_pre_trimmed.qzv ;
```

## run3_pre

```
qiime cutadapt trim-paired \
  --i-demultiplexed-sequences 02_qiime2/01_import/demux_run3_pre.qza \
  --p-front-f CCTACGGGNGGCWGCAG \
  --p-front-r GACTACHVGGGTATCTAATCC \
  --p-discard-untrimmed \
  --p-cores 4 \
  --o-trimmed-sequences 02_qiime2/02_cutadapt/demux_run3_pre_trimmed.qza ;

qiime demux summarize \
  --i-data 02_qiime2/02_cutadapt/demux_run3_post_trimmed.qza \
  --o-visualization 02_qiime2/02_cutadapt/demux_run3_pre_trimmed.qzv ;
```

## run4_pre

```
qiime cutadapt trim-paired \
  --i-demultiplexed-sequences 02_qiime2/01_import/demux_run4_pre.qza \
  --p-front-f CCTACGGGNGGCWGCAG \
  --p-front-r GACTACHVGGGTATCTAATCC \
  --p-discard-untrimmed \
  --p-cores 4 \
  --o-trimmed-sequences 02_qiime2/02_cutadapt/demux_run4_pre_trimmed.qza ;

qiime demux summarize \
  --i-data 02_qiime2/02_cutadapt/demux_run4_pre_trimmed.qza \
  --o-visualization 02_qiime2/02_cutadapt/demux_run4_pre_trimmed.qzv ;
```

## run1_post

```
qiime cutadapt trim-paired \
  --i-demultiplexed-sequences 02_qiime2/01_import/demux_run1_post.qza \
  --p-front-f CCTACGGGNGGCWGCAG \
  --p-front-r GACTACHVGGGTATCTAATCC \
  --p-discard-untrimmed \
  --p-cores 4 \
  --o-trimmed-sequences 02_qiime2/02_cutadapt/demux_run1_post_trimmed.qza ;

qiime demux summarize \
  --i-data 02_qiime2/02_cutadapt/demux_run1_post_trimmed.qza \
  --o-visualization 02_qiime2/02_cutadapt/demux_run1_post_trimmed.qzv ;
```

# DADA2

Correcció dels errors de seqüenciació, inferència de les ASV i exportació de les taules d'ASV en format BIOM.

## run1_pre

```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs 02_qiime2/02_cutadapt/demux_run1_pre_trimmed.qza \
  --p-trunc-len-f 270 \
  --p-trunc-len-r 210 \
  --p-n-threads 8 \
  --o-table 02_qiime2/03_dada2/table_run1_pre.qza \
  --o-representative-sequences 02_qiime2/03_dada2/rep-seqs_run1_pre.qza \
  --o-denoising-stats 02_qiime2/03_dada2/denoising-stats_run1_pre.qza \
  --o-base-transition-stats 02_qiime2/03_dada2/base-transition-stats_run1_pre.qza ;

qiime metadata tabulate \
  --m-input-file 02_qiime2/03_dada2/denoising-stats_run1_pre.qza \
  --o-visualization 02_qiime2/03_dada2/denoising-stats_run1_pre.qzv ;

qiime tools export \
  --input-path 02_qiime2/03_dada2/table_run1_pre.qza \
  --output-path 02_qiime2/03_dada2/table_run1_pre ;
```

## run2_pre

```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs 02_qiime2/02_cutadapt/demux_run2_pre_trimmed.qza \
  --p-trunc-len-f 270 \
  --p-trunc-len-r 220 \
  --p-n-threads 8 \
  --o-table 02_qiime2/03_dada2/table_run2_pre.qza \
  --o-representative-sequences 02_qiime2/03_dada2/rep-seqs_run2_pre.qza \
  --o-denoising-stats 02_qiime2/03_dada2/denoising-stats_run2_pre.qza \
  --o-base-transition-stats 02_qiime2/03_dada2/base-transition-stats_run2_pre.qza ;

qiime metadata tabulate \
  --m-input-file 02_qiime2/03_dada2/denoising-stats_run2_pre.qza \
  --o-visualization 02_qiime2/03_dada2/denoising-stats_run2_pre.qzv ;

qiime tools export \
  --input-path 02_qiime2/03_dada2/table_run2_pre.qza \
  --output-path 02_qiime2/03_dada2/table_run2_pre ;
```

## run3_pre

```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs 02_qiime2/02_cutadapt/demux_run3_pre_trimmed.qza \
  --p-trunc-len-f 270 \
  --p-trunc-len-r 220 \
  --p-n-threads 8 \
  --o-table 02_qiime2/03_dada2/table_run3_pre.qza \
  --o-representative-sequences 02_qiime2/03_dada2/rep-seqs_run3_pre.qza \
  --o-denoising-stats 02_qiime2/03_dada2/denoising-stats_run3_pre.qza \
  --o-base-transition-stats 02_qiime2/03_dada2/base-transition-stats_run3_pre.qza ;

qiime metadata tabulate \
  --m-input-file 02_qiime2/03_dada2/denoising-stats_run3_pre.qza \
  --o-visualization 02_qiime2/03_dada2/denoising-stats_run3_pre.qzv ;

qiime tools export \
  --input-path 02_qiime2/03_dada2/table_run3_pre.qza \
  --output-path 02_qiime2/03_dada2/table_run3_pre ;
```

## run4_pre

```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs 02_qiime2/02_cutadapt/demux_run4_pre_trimmed.qza \
  --p-trunc-len-f 270 \
  --p-trunc-len-r 220 \
  --p-n-threads 8 \
  --o-table 02_qiime2/03_dada2/table_run4_pre.qza \
  --o-representative-sequences 02_qiime2/03_dada2/rep-seqs_run4_pre.qza \
  --o-denoising-stats 02_qiime2/03_dada2/denoising-stats_run4_pre.qza \
  --o-base-transition-stats 02_qiime2/03_dada2/base-transition-stats_run4_pre.qza ;

qiime metadata tabulate \
  --m-input-file 02_qiime2/03_dada2/denoising-stats_run4_pre.qza \
  --o-visualization 02_qiime2/03_dada2/denoising-stats_run4_pre.qzv ;

qiime tools export \
  --input-path 02_qiime2/03_dada2/table_run4_pre.qza \
  --output-path 02_qiime2/03_dada2/table_run4_pre ;
```

## run1_post

```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs 02_qiime2/02_cutadapt/demux_run1_post_trimmed.qza \
  --p-trunc-len-f 280 \
  --p-trunc-len-r 220 \
  --p-n-threads 8 \
  --o-table 02_qiime2/03_dada2/table_run1_post.qza \
  --o-representative-sequences 02_qiime2/03_dada2/rep-seqs_run1_post.qza \
  --o-denoising-stats 02_qiime2/03_dada2/denoising-stats_run1_post.qza \
  --o-base-transition-stats 02_qiime2/03_dada2/base-transition-stats_run1_post.qza ;

qiime metadata tabulate \
  --m-input-file 02_qiime2/03_dada2/denoising-stats_run1_post.qza \
  --o-visualization 02_qiime2/03_dada2/denoising-stats_run1_post.qzv ;

qiime tools export \
  --input-path 02_qiime2/03_dada2/table_run1_post.qza \
  --output-path 02_qiime2/03_dada2/table_run1_post ;
```

# Filtratge de les ASV contaminants (decontam + ràtio 1:3)

Filtratge de les ASV identificades com a contaminants, combinant el mètode de prevalença de decontam amb el criteri de ràtio 1:3 als controls negatius. No es va detectar cap ASV contaminant en el *run* 1 prepandèmic (run1_pre).

## run2_pre

```
qiime feature-table filter-features \
  --i-table 02_qiime2/03_dada2/run2_pre/table_run2_pre.qza \
  --m-metadata-file 02_qiime2/04_decontam/contaminants_run2_pre.tsv \
  --p-exclude-ids \
  --o-filtered-table 02_qiime2/04_decontam/decontam_table_run2_pre.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/03_dada2/run2_pre/rep-seqs_run2_pre.qza \
  --i-table 02_qiime2/04_decontam/decontam_table_run2_pre.qza \
  --o-filtered-data 02_qiime2/04_decontam/decontam_rep-seqs_run2_pre.qza ;
```

## run3_pre

```
qiime feature-table filter-features \
  --i-table 02_qiime2/03_dada2/run3_pre/table_run3_pre.qza \
  --m-metadata-file 02_qiime2/04_decontam/contaminants_run3_pre.tsv \
  --p-exclude-ids \
  --o-filtered-table 02_qiime2/04_decontam/decontam_table_run3_pre.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/03_dada2/run3_pre/rep-seqs_run3_pre.qza \
  --i-table 02_qiime2/04_decontam/decontam_table_run3_pre.qza \
  --o-filtered-data 02_qiime2/04_decontam/decontam_rep-seqs_run3_pre.qza ;
```

## run4_pre

```
qiime feature-table filter-features \
  --i-table 02_qiime2/03_dada2/run4_pre/table_run4_pre.qza \
  --m-metadata-file 02_qiime2/04_decontam/contaminants_run4_pre.tsv \
  --p-exclude-ids \
  --o-filtered-table 02_qiime2/04_decontam/decontam_table_run4_pre.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/03_dada2/run4_pre/rep-seqs_run4_pre.qza \
  --i-table 02_qiime2/04_decontam/decontam_table_run4_pre.qza \
  --o-filtered-data 02_qiime2/04_decontam/decontam_rep-seqs_run4_pre.qza ;
```

## run1_post

```
qiime feature-table filter-features \
  --i-table 02_qiime2/03_dada2/run1_post/table_run1_post.qza \
  --m-metadata-file 02_qiime2/04_decontam/contaminants_run1_post.tsv \
  --p-exclude-ids \
  --o-filtered-table 02_qiime2/04_decontam/decontam_table_run1_post.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/03_dada2/run1_post/rep-seqs_run1_post.qza \
  --i-table 02_qiime2/04_decontam/decontam_table_run1_post.qza \
  --o-filtered-data 02_qiime2/04_decontam/decontam_rep-seqs_run1_post.qza ;
```

# Selecció de les mostres a analitzar

Eliminació de les mostres no incloses en cap dels dos estudis.

## run1_pre

```
qiime feature-table filter-samples \
  --i-table 02_qiime2/03_dada2/run1_pre/table_run1_pre.qza \
  --m-metadata-file 02_qiime2/05_filter_samples/samples_run1_pre.tsv \
  --p-no-exclude-ids \
  --o-filtered-table 02_qiime2/05_filter_samples/filtered_table_run1_pre.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/03_dada2/run1_pre/rep-seqs_run1_pre.qza \
  --i-table 02_qiime2/05_filter_samples/filtered_table_run1_pre.qza \
  --o-filtered-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run1_pre.qza ;
```

## run2_pre

```
qiime feature-table filter-samples \
  --i-table 02_qiime2/04_decontam/decontam_table_run2_pre.qza \
  --m-metadata-file 02_qiime2/05_filter_samples/samples_run2_pre.tsv \
  --p-no-exclude-ids \
  --o-filtered-table 02_qiime2/05_filter_samples/filtered_table_run2_pre.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/04_decontam/decontam_rep-seqs_run2_pre.qza \
  --i-table 02_qiime2/05_filter_samples/filtered_table_run2_pre.qza \
  --o-filtered-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run2_pre.qza ;
```

## run3_pre

```
qiime feature-table filter-samples \
  --i-table 02_qiime2/04_decontam/decontam_table_run3_pre.qza \
  --m-metadata-file 02_qiime2/05_filter_samples/samples_run3_pre.tsv \
  --p-no-exclude-ids \
  --o-filtered-table 02_qiime2/05_filter_samples/filtered_table_run3_pre.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/04_decontam/decontam_rep-seqs_run3_pre.qza \
  --i-table 02_qiime2/05_filter_samples/filtered_table_run3_pre.qza \
  --o-filtered-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run3_pre.qza ;
```

## run4_pre

```
qiime feature-table filter-samples \
  --i-table 02_qiime2/04_decontam/decontam_table_run4_pre.qza \
  --m-metadata-file 02_qiime2/05_filter_samples/samples_run4_pre.tsv \
  --p-no-exclude-ids \
  --o-filtered-table 02_qiime2/05_filter_samples/filtered_table_run4_pre.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/04_decontam/decontam_rep-seqs_run4_pre.qza \
  --i-table 02_qiime2/05_filter_samples/filtered_table_run4_pre.qza \
  --o-filtered-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run4_pre.qza ;
```

## run1_post

```
qiime feature-table filter-samples \
  --i-table 02_qiime2/04_decontam/decontam_table_run1_post.qza \
  --m-metadata-file 02_qiime2/05_filter_samples/samples_run1_post.tsv \
  --p-no-exclude-ids \
  --o-filtered-table 02_qiime2/05_filter_samples/filtered_table_run1_post.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/04_decontam/decontam_rep-seqs_run1_post.qza \
  --i-table 02_qiime2/05_filter_samples/filtered_table_run1_post.qza \
  --o-filtered-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run1_post.qza ;
```

# Fusió dels *runs* en un únic conjunt de dades

Combinació de les taules d'ASV i dels conjunts de seqüències representatives de tots els *runs* en un únic conjunt de dades.

```
qiime feature-table merge \
  --i-tables 02_qiime2/05_filter_samples/filtered_table_run1_pre.qza \
  --i-tables 02_qiime2/05_filter_samples/filtered_table_run2_pre.qza \
  --i-tables 02_qiime2/05_filter_samples/filtered_table_run3_pre.qza \
  --i-tables 02_qiime2/05_filter_samples/filtered_table_run4_pre.qza \
  --i-tables 02_qiime2/05_filter_samples/filtered_table_run1_post.qza \
  --o-merged-table 02_qiime2/06_merge_samples/merged_table.qza ;

qiime feature-table merge-seqs \
  --i-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run1_pre.qza \
  --i-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run2_pre.qza \
  --i-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run3_pre.qza \
  --i-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run4_pre.qza \
  --i-data 02_qiime2/05_filter_samples/filtered_rep-seqs_run1_post.qza \
  --o-merged-data 02_qiime2/06_merge_samples/merged_rep-seqs.qza ;
```

# Assignació taxonòmica

Assignació de la taxonomia amb un classificador Naive Bayes entrenat per a la regió V3-V4 i filtratge posterior de les ASV.

## Entrenament del classificador

Extracció de la regió V3-V4 de la base de dades de referència i entrenament del classificador.

```
qiime feature-classifier extract-reads \
  --i-sequences 02_qiime2/07_taxonomy/classifier/2024.09.backbone.full-length.fna.qza \
  --p-f-primer CCTACGGGNGGCWGCAG \
  --p-r-primer GACTACHVGGGTATCTAATCC \
  --p-n-jobs 8 \
  --o-reads 02_qiime2/07_taxonomy/classifier/gg2-2024.09-seqs-V3-V4.qza ;

qiime feature-classifier fit-classifier-naive-bayes \
  --i-reference-reads 02_qiime2/07_taxonomy/classifier/gg2-2024.09-seqs-V3-V4.qza \
  --i-reference-taxonomy 02_qiime2/07_taxonomy/classifier/2024.09.backbone.tax.qza \
  --o-classifier 02_qiime2/07_taxonomy/classifier/gg2-2024.09-v3-v4-classifier.qza ;
```

## Classificació de les ASV

Aplicació del classificador al conjunt de seqüències representatives.

```
qiime feature-classifier classify-sklearn \
  --i-reads 02_qiime2/06_merge_samples/merged_rep-seqs.qza \
  --i-classifier 02_qiime2/07_taxonomy/classifier/gg2-2024.09-V3-V4-classifier.qza \
  --o-classification 02_qiime2/07_taxonomy/taxonomy.qza \
  --p-n-jobs 8 ;
```

## Filtratge taxonòmic

Eliminació de les ASV assignades a mitocondris, cloroplasts o arqueus, i de les que no tenen assignació a nivell de fílum.

```
qiime taxa filter-table \
  --i-table 02_qiime2/06_merge_samples/merged_table.qza  \
  --i-taxonomy 02_qiime2/07_taxonomy/taxonomy.qza  \
  --p-include "p__" --p-exclude "f__Mitochondria","c__Chloroplast","d__Archaea" \
  --o-filtered-table 02_qiime2/07_taxonomy/clean_table.qza ;
```

# Separació en infants sans i infants amb MPI

Divisió de la taula d'ASV i del conjunt de seqüències representatives en els dos estudis (infants sans i infants amb MPI), i exportació de la taula d'ASV de cada estudi en format BIOM.

## Infants sans

```
qiime feature-table filter-samples \
  --i-table 02_qiime2/07_taxonomy/clean_table.qza \
  --m-metadata-file 02_qiime2/08_split_samples/healthy_samples.tsv \
  --p-no-exclude-ids \
  --o-filtered-table 02_qiime2/08_split_samples/healthy_table.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/06_merge_samples/merged_rep-seqs.qza \
  --i-table 02_qiime2/08_split_samples/healthy_table.qza \
  --o-filtered-data 02_qiime2/08_split_samples/healthy_rep-seqs.qza ;

qiime tools export \
  --input-path 02_qiime2/08_split_samples/healthy_table.qza \
  --output-path 02_qiime2/08_split_samples/healthy_table ;
```

## Infants amb MPI

```
qiime feature-table filter-samples \
  --i-table 02_qiime2/07_taxonomy/clean_table.qza \
  --m-metadata-file 02_qiime2/07_split_samples/IPD_samples.tsv \
  --p-no-exclude-ids \
  --o-filtered-table 02_qiime2/07_split_samples/IPD_table.qza ;

qiime feature-table filter-seqs \
  --i-data 02_qiime2/06_merge_samples/merged_rep-seqs.qza \
  --i-table 02_qiime2/08_split_samples/IPD_table.qza \
  --o-filtered-data 02_qiime2/08_split_samples/IPD_rep-seqs.qza ;

qiime tools export \
  --input-path 02_qiime2/08_split_samples/IPD_table.qza \
  --output-path 02_qiime2/08_split_samples/IPD_table ;
```

# Col·lapse de les taules d'ASV a nivell de gènere

Col·lapse de les taules d'ASV al nivell taxonòmic de gènere per a cada estudi i exportació de les taules resultants en format BIOM.

## Infants sans

```
qiime taxa collapse \
  --i-table 02_qiime2/08_split_samples/healthy_table.qza \
  --i-taxonomy 02_qiime2/07_taxonomy/taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table 02_qiime2/08_split_samples/healthy_table_l6.qza ;

qiime tools export \
  --input-path 02_qiime2/08_split_samples/healthy_table_l6.qza \
  --output-path 02_qiime2/08_split_samples/healthy_table_l6 ;
```

## Infants amb MPI

```
qiime taxa collapse \
  --i-table 02_qiime2/08_split_samples/IPD_table.qza \
  --i-taxonomy 02_qiime2/07_taxonomy/taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table 02_qiime2/08_split_samples/IPD_table_l6.qza ;

qiime tools export \
  --input-path 02_qiime2/08_split_samples/IPD_table_l6.qza \
  --output-path 02_qiime2/08_split_samples/IPD_table_l6 ;
```

# Construcció de l'arbre filogenètic

Construcció de l'arbre filogenètic, necessari per calcular les mètriques de diversitat filogenètica.

## Infants sans

```
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences 02_qiime2/08_split_samples/healthy_rep-seqs.qza \
  --o-alignment 02_qiime2/09_phylogeny/healthy_aligned-rep-seqs.qza \
  --o-masked-alignment 02_qiime2/09_phylogeny/healthy_masked-aligned-rep-seqs.qza \
  --o-tree 02_qiime2/09_phylogeny/healthy_unrooted-tree.qza \
  --o-rooted-tree 02_qiime2/09_phylogeny/healthy_rooted-tree.qza ;
```

## Infants amb MPI

```
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences 02_qiime2/08_split_samples/IPD_rep-seqs.qza \
  --o-alignment 02_qiime2/09_phylogeny/IPD_aligned-rep-seqs.qza \
  --o-masked-alignment 02_qiime2/09_phylogeny/IPD_masked-aligned-rep-seqs.qza \
  --o-tree 02_qiime2/09_phylogeny/IPD_unrooted-tree.qza \
  --o-rooted-tree 02_qiime2/09_phylogeny/IPD_rooted-tree.qza ;
```

# Anàlisi de diversitat

Càlcul de les mètriques de diversitat alfa i beta a partir de les taules d'ASV normalitzades per rarefacció.

## Corbes de rarefacció

Generació de les corbes de rarefacció per avaluar la cobertura de seqüenciació.

### Infants sans

```
qiime diversity alpha-rarefaction \
  --i-table 02_qiime2/08_split_samples/healthy_table.qza \
  --i-phylogeny 02_qiime2/09_phylogeny/healthy_rooted-tree.qza \ 
  --p-max-depth 60000 \
  --p-steps 12 \ 
  --m-metadata-file 00_bbdd/bbdd.tsv \ 
  --o-visualization 02_qiime2/10_diversity/healthy_core-metrics/alpha-rarefaction.qzv ;
```

### Infants amb MPI

```
qiime diversity alpha-rarefaction \
  --i-table 02_qiime2/08_split_samples/IPD_table.qza \
  --i-phylogeny 02_qiime2/09_phylogeny/IPD_rooted-tree.qza \
  --p-max-depth 60000 \
  --p-steps 12 \
  --m-metadata-file 00_bbdd/bbdd.tsv \
  --o-visualization 02_qiime2/10_diversity/IPD_core-metrics/alpha-rarefaction.qzv ;
```

## Mètriques de diversitat

Càlcul de les principals mètriques de diversitat alfa i beta.

### Infants sans

```
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny 02_qiime2/09_phylogeny/healthy_rooted-tree.qza \
  --i-table 02_qiime2/08_split_samples/healthy_table.qza \
  --p-sampling-depth 10891 \
  --m-metadata-file 00_bbdd/bbdd.tsv \
  --p-random-seed 12 \
  --output-dir 02_qiime2/10_diversity/healthy_core-metrics ;
```

### Infants amb MPI

```
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny 02_qiime2/09_phylogeny/IPD_rooted-tree.qza \
  --i-table 02_qiime2/08_split_samples/IPD_table.qza \
  --p-sampling-depth 15482 \
  --m-metadata-file 00_bbdd/bbdd.tsv \
  --p-random-seed 12 \
  --output-dir 02_qiime2/10_diversity/IPD_core-metrics ;
```