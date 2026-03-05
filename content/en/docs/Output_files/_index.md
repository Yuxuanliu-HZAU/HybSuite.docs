---
title: "Output files"
linkTitle: "Output files"
menu:
  main:
    identifier: outputfiles   # 必须唯一
    name: "Output files"
    weight: 4
  # 设置显示左侧 sidebar
sidebar:
  enabled: true
  section: "Output files"
---
* [**Stage1 output**](#stage1-output)
  * [NGS_dataset](#ngs_dataset-specified-by--d)
* [**Stage2 output**](#stage2-output)
  * [01-Assembled_data](#01-assembled_data)
  * [02-All_paralogs](#02-all_paralogs)
* [**Stage3 output**](#stage3-output)
  * [03-Paralog_handling](#03-paralog_handling)
  * [04-Alignments](#04-alignments)
  * [05-Trimmed_alignments](#05-trimmed-alignments)
  * [06-Final_alignments](#06-final_alignments)
* [**Stage4 output**](#stage4-output)
  * [07-Concatenated_analysis](#07-concatenated_analysis)
  * [08-Coalescent_analysis](#08-coalescent_analysis)
* [**Comprehensive output**](#comprehensive-output)
  * [hybsuite_logs](#hybsuite_logs)
  * [hybsuite_reports](#hybsuite_reports)
  * [hybsuite_checklists](#hybsuite_checklists)

---
## Output File Naming Conventions

| Placeholder    | Represents                                                                |
|----------------|---------------------------------------------------------------------------|
| `<PH>`         | Any of the 7 orthology inference methods: HRS, RLWP, LS, MI, MO, RT, 1to1 |
| `<taxon>`      | Taxon name (e.g., `<taxon1>`, `<taxon2>`, etc.) from your sample list file |
| `<prefix>`     | User-specified output prefix (via `-prefix` option)                       |
| `<locus_name>` | Target sequence locus (e.g., `<locus_name1>`, `<locus_name2>`, etc.)      |
    
**The specific explanation of every output file is illustrated as follows.**    
    
---

## Stage1 output

### `<NGS_dataset>` (specified by `-d`)
A directory containing next-generation raw sequencing data downloaded from public databases, existing raw reads provided by the user, and clean data produced by Trimmomatic-0.39. 
```
<NGS_dataset>/
├── 01-Downloaded_raw_data/
├── 02-Downloaded_clean_data/
└── 03-My_clean_data/
```
#### `<NGS_dataset>` -> `01-Downloaded_raw_data`
A directory containing next-generation raw sequencing data downloaded from public databases.    
```
<NGS_dataset>/
└── 01-Downloaded_raw_data/
    ├── 01-Raw-reads_sra/
    └── 02-Raw-reads_fastq_gz/
```

#### `<NGS_dataset>` -> `01-Downloaded_raw_data` -> `01-Raw-reads_sra`
A directory containing raw sequencing data downloaded from NCBI in `.sra` format.
```
<NGS_dataset>/
└── 01-Downloaded_raw_data/
    └── 01-Raw-reads_sra/
        ├── <taxon>.sra
        ...
```
- `<taxon>.sra`: File with raw sequencing data in SRA format.
> By default, all `*.sra` files in this directory will be removed after converting them into `fastq` format to save space, unless you specify the option **`-rm_sra`** as **`FALSE`** to keep them.

#### `<NGS_dataset>` -> `01-Downloaded_raw_data` -> `02-Raw-reads_fastq_gz`
A directory containing raw sequencing data in `.fastq` or `.fastq.gz` format.
```
<NGS_dataset>/
└── 01-Downloaded_raw_data/
    └── 02-Raw-reads_fastq_gz/
        ├── <taxon>.fastq.gz or <taxon>.fastq
        ...
```
- `<taxon>.fastq.gz` or `<taxon>.fastq`:
> If the user specifies the option **`-download_format`** as **`fastq`**, pigz will not be used to compress the original **`.fastq`** files to **`.fastq.gz`** files, which will produce `<taxon>.fastq` in this folder.    
> If the user specifies the option **`-download_format`** as **`fastq.gz`**, pigz will be used to compress the original **`.fastq`** files to **`.fastq.gz`** files, which will produce `<taxon>.fastq.gz` in this folder.  
> **Default: `-download_format` is specified as `fastq.gz`.**

#### `<NGS_dataset>` -> `02-Downloaded_clean_data`
A directory containing sequencing data cleaned from downloaded public raw reads.
```
<NGS_dataset>/
└── 02-Downloaded_clean_data/
    ├── <taxon>_1_clean.paired.fq.gz
    ├── <taxon>_2_clean.paired.fq.gz
    ├── <taxon>_1_clean.unpaired.fq.gz
    ├── <taxon>_2_clean.unpaired.fq.gz
    ├── <taxon>_clean.single.fq.gz
    ├── <taxon>_clean.single.fq.gz
    ...    
```
- **`<taxon>_1_clean.paired.fq.gz` & `<taxon>_2_clean.paired.fq.gz`**   
Files with compressed cleaned and paired sequencing data (paired-end type) in `fq.gz` format (these files will be used for downstream analysis).
- **`<taxon>_1_clean.unpaired.fq.gz` & `<taxon>_2_clean.unpaired.fq.gz`**  
Files with compressed cleaned and unpaired sequencing data (paired-end type) in `fq.gz` format.
- **`<taxon>_clean.single.fq.gz`**  
File with compressed cleaned sequencing data (single-end type) in `fq.gz` format (these files will be used for downstream analysis).

#### `<NGS_dataset>` -> `03-My_clean_data`
A directory containing user-provided cleaned sequencing data or sequencing data cleaned from user-provided raw data.
```
<NGS_dataset>/
└── 03-My_clean_data/
    ├── <taxon>_1_clean.paired.fq.gz
    ├── <taxon>_2_clean.paired.fq.gz
    ├── <taxon>_1_clean.unpaired.fq.gz
    ├── <taxon>_2_clean.unpaired.fq.gz
    ├── <taxon>_clean.single.fq.gz
    ...   
```
- **`<taxon>_1_clean.paired.fq.gz` & `<taxon>_2_clean.paired.fq.gz`**  
Files with user-provided compressed cleaned and paired sequencing data (paired-end type) in `fq.gz` format (these files will be used for downstream analysis).
- **`<taxon>_1_clean.unpaired.fq.gz` & `<taxon>_2_clean.unpaired.fq.gz`**  
Files with user-provided compressed cleaned and unpaired sequencing data (paired-end type) in `fq.gz` format.
- **`<taxon>_clean.single.fq.gz`**  
File with user-provided compressed cleaned sequencing data (single-end type) in `fq.gz` format (these files will be used for downstream analysis).
---

## Stage2 output

### `01-Assembled_data`
A directory containing assembled sequence data produced by [`hybpiper assemble` command in HybPiper](https://github.com/mossmatters/HybPiper/wiki/Tutorial#running-multiple-samples).

```
01-Assembled_data/
├── Assembled_data_namelist.txt    
├── Old_assembled_data_namelist_<current_time>.log
├── <taxon>/
    ...
```
- **`Assembled_data_namelist.txt`**
A file containing sample names used as input to run the [`hybpiper assemble`](https://github.com/mossmatters/HybPiper/wiki/Tutorial#running-multiple-samples) command.
- **`Old_assembled_data_namelist_<current_time>.log`**
A file containing previous sample names used as input to run the [`hybpiper assemble`](https://github.com/mossmatters/HybPiper/wiki/Tutorial#running-multiple-samples) command.
- **`<taxon>`**
More details can be found [here](https://github.com/mossmatters/HybPiper/wiki/Results-and-output-files#10-hybpiper-assemble).

### `02-All_paralogs`
A directory containing all original putative paralogs retrieved by the [`hybpiper paralog_retriever` command in HybPiper](https://github.com/mossmatters/HybPiper/wiki/Paralogs), filtered paralogs, along with their paralog heatmaps and related statistical results.
```
02-All_paralogs/
├── 01-Original_paralogs
├── 02-Original_paralog_reports_and_heatmap
├── 03-Filtered_paralogs
└── 04-Filtered_paralog_reports_and_heatmap

```
#### `02-All_paralogs` -> `01-Original_paralogs`
A directory containing all original putative paralogs retrieved by the [`hybpiper paralog_retriever` command in HybPiper](https://github.com/mossmatters/HybPiper/wiki/Paralogs).
```
02-All_paralogs/
└── 01-Original_paralogs/
    └── <locus_name>_paralogs_all.fasta
```
- `<locus_name>_paralogs_all.fasta`: FASTA files for each sample/locus, containing all putative paralogs recovered by the HybPiper [`hybpiper paralog_retriever`](https://github.com/mossmatters/HybPiper/wiki/Results-and-output-files#50-hybpiper-paralog_retriever) command.

#### `02-All_paralogs` -> `02-Original_paralog_reports_and_heatmap`
A directory containing all original reports and heatmaps.
```
02-All_paralogs/
└── 02-Original_paralog_reports_and_heatmap/
    ├── Original_paralog_heatmap.png
    ├── Original_paralog_report.tsv
    ├──Original_recovered_seqs_length.tsv
    └── Original_recovery_heatmap.html
```
- **`Original_paralog_heatmap.png`**  
A heatmap image file in PNG format, depicting the number of original putative paralog sequences for each locus/sample.
- **`Original_paralog_report.tsv`**  
A TSV file recording the number of original putative paralog sequences for each locus/sample.
- **`Original_recovered_seqs_length.tsv`**  
A TSV file recording the length of original recovered sequences for each locus/sample.
- **`Original_recovery_heatmap.html`**  
An interactive HTML file for visualizing target locus recovery across all original paralogs (including both single-copy and multi-copy genes).

**Here is a recovery heatmap example file you can play with:** it shows a recovery result of the Angiosperms353 [(Johnson et al., 2019)](https://doi.org/10.1093/sysbio/syy086) loci from 10 Elaeagnaceae species in [our example dataset](/docs/Example_dataset/#example-dataset). 
- The blue bars along with x- and y-axes indicate how many loci are recovered in each sample and how many samples each locus are recovered in, respectively.  
- The color intensity of each cell indicates the proportion of gene length recovered for a given sample (y-axis) at a specific target locus (x-axis). When multiple sequences are recovered for a locus within a sample (putative paralogs), only the longest sequence is retained for visualization in the heatmap.

{{< plotly json="/plotly/Original_recovery_heatmap.json" height="400px" width=50% >}}

Now, let's play with this interactive html file for fun and better effect!
> - Choose the button **"Sort by"** as **"Descending"** to sort samples and loci on the heatmap from high to low recovery.
> - Click on the **"Plus" (+)** and **"Minus" (-)** icons in the upper right corner to zoom in and out of the heatmap.
> - Click on the **"AutoScale"** icon in the upper right corner to auto-scale the heatmap.
> - Click the **"Camera" (📷)** icon in the upper right corner to download the current heatmap view as a PNG file.
 
> [!TIP] 
> - If some samples recover very few or no loci, we recommend replacing their data sources or increasing the value of `-seqs_min_loci_coverage` to exclude these low-quality samples from downstream analyses.

#### `02-All_paralogs` -> `03-Filtered_paralogs`
A directory containing all filtered putative paralogs retrieved by the [`hybpiper paralog_retriever` command in HybPiper](https://github.com/mossmatters/HybPiper/wiki/Paralogs).
```
02-All_paralogs/
└── 03-Filtered_paralogs/
    └── <locus_name>_paralogs_all.fasta
```

#### `02-All_paralogs` -> `04-Filtered_paralog_reports_and_heatmap`
A directory containing all filtered reports and heatmaps.
```
02-All_paralogs/
└── 04-Filtered_paralog_reports_and_heatmap/
    ├── Filtered_paralog_heatmap.png
    └── Filtered_paralog_report.tsv
```
- **`Filtered_paralog_heatmap.png`**  
A heatmap image file in PNG format, depicting the number of filtered putative paralog sequences for each locus/sample.
- **`Filtered_paralog_report.tsv`**  
A TSV file recording the number of filtered putative paralog sequences for each locus/sample.
- **`Filtered_recovered_seqs_length.tsv`**   
A TSV file recording the length of original recovered sequences for each locus/sample.
- **`Filtered_recovery_heatmap.html`**  
An interactive HTML file for visualizing target locus recovery across all filtered paralogs (including both single-copy and multi-copy genes).  
The layout is identical to that of `Original_recovery_heatmap.png`, but it reflects the occupancy of filtered sequences rather than the original ones.

---

## Stage3 output

### `03-Paralog_handling`
```
03-Paralog_handling/
├── HRS/ (optional)
├── RLWP/ (optional)
├── ParaGone/ (optional)
└── PhyloPyPruner/ (optional)
```

- **Different arguments for the option `-PH` will lead to different subdirectories in this output folder:**    
  - `HRS/`: created when `-PH` includes number 1 (the user applies the HRS orthology inference method)
  - `RLWP/`: created when `-PH` includes number 2 (the user applies the RLWP orthology inference method)
  - `PhyloPyPruner/`: created when `-PH` includes one or several numbers of `4, 5, 6, and 7` (using MI/MO/RT/1to1 orthology inference methods) and "a" (default), or includes number "3" (directly running PhyloPyPruner to carry out LS method). More details about the interpretation of these orthology inference methods can be found [here](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Methods#orthology-inference)  
  - `ParaGone/`: created when `-PH` includes one or several numbers of `4, 5, 6, and 7` (the user applies MI/MO/RT/1to1 orthology inference methods) and "b" (running ParaGone rather than PhyloPyPruner). More details about the interpretation of these orthology inference methods can be found [here](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Methods#orthology-inference) 
    
- **For example:**
  - Using `-PH 12` will create `HRS` and `RLWP` directories.   
  - Using `-PH 1234b` will create `HRS`, `RLWP`, and `ParaGone` directories.

#### `03-Paralog_handling` -> `HRS`
A directory containing original and filtered HRS sequences, including the recovery heatmap and filtering reports.
```
03-Paralog_handling/
└── HRS/
    ├── 01-Original_HRS_sequences
        └── <locus_name>.FNA
    ├── 02-Original_HRS_sequences_reports_and_heatmap
        ├── Original_HRS_heatmap.png
        └── Original_HRS_seq_lengths.tsv
    ├── 03-Filtered_HRS_sequences
        └── <locus_name>.FNA
    └── 04-Filtered_HRS_sequences_reports_and_heatmap
        ├── Filtered_HRS_heatmap.png
        ├── Filtered_HRS_seq_lengths.tsv
        ├── Removed_HRS_seqs_with_low_length_info.tsv
        ├── Removed_samples_with_low_locus_coverage_info.tsv
        └── Removed_loci_with_low_sample_coverage_info.tsv
```

##### `01-Original_HRS_sequences`
  - **`<locus_name>.FNA`**  
  Files with retrieved sequences in FASTA format, produced by [`hybpiper retrieve_sequences`](https://github.com/mossmatters/HybPiper/wiki/Tutorial#retrieving-sequences) (referred to as HRS sequences in the following).  
   
> **Notes:**     
> - In the HybSuite pipeline, supercontigs are automatically retrieved, including introns and exons, for downstream analysis. HybSuite doesn't support retrieving only introns or exons.
> - Since the downstream analysis requires DNA sequences, only DNA sequences can be retrieved; protein sequences are not supported for the next stage.

##### `02-Original_HRS_sequences_reports_and_heatmap`
  - **`Original_HRS_heatmap.png`**  
  A heatmap image file in PNG format, depicting the length of the original HRS sequences for each gene and sample, relative to the mean length (default setting; users can customize by running `plot_recovery_heatmap.py`) of the sequences in the target file, produced by `plot_recovery_heatmap.py` in HybSuite.  
  - **`Original_HRS_seq_lengths.tsv`**  
  A TSV file recording all original HRS sequences' bp length, length ratio relative to the maximum, and mean length of each locus' sequences in the target file.  

##### `03-Filtered_HRS_sequences`
  - **`<locus_name>.FNA`**  
  Files with filtered HRS sequences in FASTA format, produced by [`hybpiper retrieve_sequences`](https://github.com/mossmatters/HybPiper/wiki/Tutorial#retrieving-sequences).  
  
##### `04-Filtered_HRS_sequences_reports_and_heatmap`
  - **`Filtered_HRS_heatmap.png`**  
  A heatmap image file in PNG format, depicting the length of the filtered HRS sequences for each gene and sample, relative to the mean length (default setting; users can customize by running `plot_recovery_heatmap.py`) of the sequences in the target file, produced by `plot_recovery_heatmap.py` in HybSuite.
  - **`Filtered_HRS_seq_lengths.tsv`**  
  A TSV file recording all filtered HRS sequences' bp length, length ratio relative to the maximum, and mean length of each locus' sequences in the target file.
  - **`Removed_HRS_seqs_with_low_length_info.tsv`**  
  A TSV file recording the information of the HRS sequences with low bp length/length ratio that have been filtered out from the dataset.
  - **`Removed_samples_with_low_locus_coverage_info.tsv`**  
  A TSV file recording the information of the samples with low locus coverage that have been filtered out from the dataset.
  - **`Removed_loci_with_low_sample_coverage_info.tsv`**  
  A TSV file recording the information of the loci with low sample coverage that have been filtered out from the dataset.

#### `03-Paralog_handling` -> `RLWP`
A directory containing original and filtered RLWP sequences, including the recovery heatmap and filtering reports.
```
03-Paralog_handling/
└── RLWP/
    ├── 01-Original_RLWP_sequences
        └── <locus_name>.FNA
    ├── 02-Original_RLWP_sequences_reports_and_heatmap
        ├── Original_RLWP_heatmap.png
        └── Original_RLWP_seq_lengths.tsv
    ├── 03-Filtered_RLWP_sequences
        └── <locus_name>.FNA
    └── 04-Filtered_RLWP_sequences_reports_and_heatmap
        ├── Filtered_RLWP_heatmap.png
        ├── Filtered_RLWP_seq_lengths.tsv
        ├── Removed_RLWP_seqs_with_low_length_info.tsv
        ├── Removed_samples_with_low_locus_coverage_info.tsv
        └── Removed_loci_with_low_sample_coverage_info.tsv
```

##### `01-Original_RLWP_sequences`

- **`<locus_name>.FNA`**  
Files with retrieved sequences in FASTA format, produced by [`hybpiper retrieve_sequences`](https://github.com/mossmatters/HybPiper/wiki/Tutorial#retrieving-sequences) (referred to as RLWP sequences in the following). 
 
> **Notes:**     
> - In the HybSuite pipeline, supercontigs are automatically retrieved, including introns and exons, for downstream analysis. HybSuite doesn't support retrieving only introns or exons.
> - Since the downstream analysis requires DNA sequences, only DNA sequences can be retrieved; protein sequences are not supported for the next stage.

##### `02-Original_RLWP_sequences_reports_and_heatmap`

  - **`Original_RLWP_heatmap.png`**  
  A heatmap image file in PNG format, depicting the length of the original RLWP sequences for each gene and sample, relative to the mean length (default setting; users can customize by running `plot_recovery_heatmap.py`) of the sequences in the target file, produced by `plot_recovery_heatmap.py` in HybSuite.
  - **`Original_RLWP_seq_lengths.tsv`**  
  A TSV file recording all original RLWP sequences' bp length, length ratio relative to the maximum, and mean length of each locus' sequences in the target file. 

##### `03-Filtered_RLWP_sequences`  
  - **`<locus_name>.FNA`**  
  Files with filtered RLWP sequences in FASTA format, produced by [`hybpiper retrieve_sequences`](https://github.com/mossmatters/HybPiper/wiki/Tutorial#retrieving-sequences).  
   
##### `04-Filtered_RLWP_sequences_reports_and_heatmap`
  - **`Filtered_RLWP_heatmap.png`**  
  A heatmap image file in PNG format, depicting the length of the filtered RLWP sequences for each gene and sample, relative to the mean length (default setting; users can customize by running `plot_recovery_heatmap.py`) of the sequences in the target file, produced by `plot_recovery_heatmap.py` in HybSuite. 
  - **`Filtered_RLWP_seq_lengths.tsv`**  
  A TSV file recording all filtered RLWP sequences' bp length, length ratio relative to the maximum, and mean length of each locus' sequences in the target file. 
  - **`Removed_RLWP_seqs_with_low_length_info.tsv`**  
  A TSV file recording the information of the RLWP sequences with low bp length/length ratio that have been filtered out from the dataset. 
  - **`Removed_samples_with_low_locus_coverage_info.tsv`**  
  A TSV file recording the information of the samples with low locus coverage that have been filtered out from the dataset. 
  - **`Removed_loci_with_low_sample_coverage_info.tsv`**  
  A TSV file recording the information of the loci with low sample coverage that have been filtered out from the dataset.

#### `03-Paralog_handling` -> `ParaGone`
```
03-Paralog_handling/
└── ParaGone/
    ├── 00_logs_and_reports
    ...
    ├── 28_RT_final_alignments_trimmed
    └── HybSuite_1to1_final_alignments
```
- From `00_logs_and_reports` to `28_RT_final_alignments_trimmed`: More details about these output folders can be found on [this wiki page](https://github.com/chrisjackson-pellicle/ParaGone/wiki/Results-and-output-files) of ParaGone.
- If the user specifies the `-paragone_keep_files` option in HybSuite as `TRUE`, the intermediate folders from `01_input_paralog_fasta` to `22_RT_stripped_names` will be kept. If the user specifies the `-paragone_keep_files` option as `FALSE`, intermediate folders will be removed.
- `HybSuite_1to1_final_alignments`: A directory containing orthology group alignments produced via the 1to1 algorithm, which were retrieved from results produced by ParaGone.

#### `03-Paralog_handling` -> `PhyloPyPruner`
```
03-Paralog_handling/
└── PhyloPyPruner/
    ├── Input
    ├── Output_LS
    ├── Output_MI
    ├── Output_MO
    ├── Output_RT
    └── Output_1to1
```
- **`Input`**  
A directory containing trimmed alignments of each locus and their gene trees (input files for running PhyloPyPruner).
- **`<locus_name>_paralogs_all.aln.trimmed.fasta`**  
The trimmed alignment of locus `<locus_name>` from `02-All_paralogs/03-Filtered_paralogs/<locus_name>_paralogs_all.fasta`, generated by MAFFT and TrimAl.
- **`<locus_name>_paralogs_all.aln.trimmed.fasta.tre`**  
The gene tree of locus `<locus_name>`, constructed by FastTree.
- **`Output_<PH>`**  
A directory containing PhyloPyPruner output files for the `<PH>` algorithm (`<PH>` includes `LS`, `MI`, `MO`, `RT`, `1to1`; more details can be found [here](https://gitlab.com/fethalen/phylopypruner/-/wikis/output-files)).

---

### `04-Alignments`
A directory containing alignments produced by different paralog-handling methods specified by the user. These alignments are then trimmed and filtered in stage 3.   

```
04-Alignments/
└── <PH>/
    └──<ortholog_group_name>.*.aln.fasta
```

- **`<PH>/<ortholog_group_name>.*.aln.fasta`**  
The alignments which are inferred via the `<PH>` paralog-handling method and multiple sequence alignment by MAFFT.

> **NOTE:**  
> `<ortholog_group_name>` is the name of the ortholog group inferred by the `<PH>` algorithm. For example, `4757_1` and `4757_2` are inferred ortholog group names from locus `4757`.

---

### `05-Trimmed_alignments`
A directory containing trimmed alignments inferred via different `<PH>` paralog-handling methods.  
```
05-Trimmed_alignments/
└── <PH>/
    └── <ortholog_group_name>.*.aln.trimmed.fasta
```

- **`<PH>/<ortholog_group_name>.*.aln.trimmed.fasta`**  
The alignments which are inferred via the `<PH>` paralog-handling method, aligned using MAFFT, and trimmed via TrimAl or cleaned via HMMCleaner.  

> **NOTE:**  
> `<ortholog_group_name>` is the name of the ortholog group inferred by the `<PH>` algorithm. For example, `4757_1` and `4757_2` are inferred ortholog group names from locus `4757`.

---

### `06-Final_alignments`
A directory containing final `<PH>` orthogroup alignments ready for downstream species tree inference. 
```
06-Final_alignments/
└── <PH>/
    └── <ortholog_group_name>.*.aln.trimmed.fasta
```
- **`<PH>/<ortholog_group_name>.*.aln.trimmed.fasta`**
Final `<PH>` orthogroup alignments for downstream species tree inference (stage4).

---
## Stage4 output

### `07-Concatenated_analysis`
A directory containing concatenated analysis results.
```
07-Concatenated_analysis/
<PH>
  ├── 01-Supermatrix
      ├── partition.txt
      └── <prefix>_<PH>.fasta
  └── 02-Species_tree
      ├── IQTREE
          └── IQ-TREE*
      ├── RAxML
          └── RAxML*
      ├── RAxML-NG
          └── RAxML-NG*
      ├── <prefix>_<PH>_ModelTest_NG.txt.tree
      ├── <prefix>_<PH>_ModelTest_NG.txt.log
      ├── <prefix>_<PH>_ModelTest_NG.txt.out
      └── <prefix>_<PH>_ModelTest_NG.txt.ckp
```

#### `07-Concatenated_analysis` -> `<PH>` -> `01-Supermatrix`

A directory containing the supermatrix concatenated from `<PH>` orthogroup alignments and the partition file.

- **`<prefix>_<PH>.fasta`**  
The concatenated supermatrix file for orthology groups inferred by the `<PH>` method.
- **`partition.txt`**  
The partition file for concatenation.

#### `07-Concatenated_analysis` -> `<PH>` -> `02-Species_tree`

##### **`IQ-TREE/`**  
A directory containing IQ-TREE results and final rooted trees (created only when IQ-TREE is applied by setting `-sp_tree 1`).
  - **`IQ-TREE_<prefix>_<PH>.*`**  
  IQ-TREE intermediate output files.
  - **`IQ-TREE_<prefix>_<PH>.treefile`**  
  The tree file with branch lengths and bootstrap values, generated by IQ-TREE.
  - **`IQ-TREE_<prefix>_<PH>.rr.tre`**  
  The rerooted tree file with branch lengths and bootstrap values from IQ-TREE results.

##### **`RAxML/`**  
A directory containing RAxML results and final rooted trees (created only when RAxML is applied by setting `-sp_tree 2`).
  - **`RAxML_*.<prefix>_<PH>.*`**  
  RAxML intermediate output files.
  - **`RAxML_<prefix>_<PH>.rr.tre`**  
  The rerooted tree file with branch lengths and bootstrap values from RAxML results.

##### **`RAxML-NG/`**  
A directory containing RAxML-NG results and final rooted trees (created only when RAxML-NG is applied by setting `-sp_tree 3`).
  - **`RAxML-NG_<prefix>_<PH>.raxml.*`**  
  RAxML-NG intermediate output files.
  - **`RAxML-NG_<prefix>_<PH>.rr.tre`**  
  The rerooted tree file with branch lengths and bootstrap values from RAxML-NG results.

#### `07-Concatenated_analysis` -> `<PH>` -> `<prefix>_<PH>_ModelTest_NG.txt.*`

The output files generated by [ModelTest-NG](https://github.com/ddarriba/modeltest).

---

### `08-Coalescent_analysis`   
A directory containing coalescent analysis results.
```
08-Coalescent_analysis/
├── <PH>
    ├── 01-Gene_trees
    ├── 02-Combined_gene_trees
    ├── 03-Species_tree
    ├── 04-Rerooted_gene_trees
    └── 05-PhyParts_PieCharts
└── ASTRAL-Pro
    ├── 01-Gene_trees
    ├── 02-Combined_gene_trees
    ├── 03-Species_tree
    └── 04-Rerooted_gene_trees
```
#### `08-Coalescent_analysis` -> `<PH>`
A directory containing coalescent-based phylogenetic tree results for a specific dataset generated by the `<PH>` paralog-handling method.  

##### `08-Coalescent_analysis` -> `<PH>` -> `01-Gene_trees`    
A directory containing gene trees inferred from final `<PH>` alignments.
- **`<ortholog_group_name>.tre`**: The gene tree for locus/orthogroup `<ortholog_group_name>`.
> **NOTE:** 
> `<ortholog_group_name>` is the name of the ortholog group inferred by the `<PH>` algorithm. For example, ortholog group names `4757_1` and `4757_2` are inferred from locus `4757`.

##### `08-Coalescent_analysis` -> `<PH>` -> `02-Combined_gene_trees`    
A directory containing combined gene trees generated from `<PH>` alignments.
- **`Combined_gene_trees.tre`**: File containing all gene trees combined into a single file.
- **`Combined_gene_trees.tre.collapsed`**: File containing all gene trees with low-support branches collapsed.

##### `08-Coalescent_analysis` -> `<PH>` -> `03-Species_tree`    
A directory containing species trees inferred from `<PH>` alignments.
###### **`ASTRAL-IV/`**
A directory containing the final species tree for the `<PH>` dataset, generated by ASTRAL-IV.
  - **`ASTRAL-IV_<prefix>_<PH>.log`**  
  The log file generated by ASTRAL-IV.
  - **`ASTRAL-IV_<prefix>_<PH>.tre`**  
  The species tree inferred by ASTRAL-IV from the combined gene trees.  
  - **`ASTRAL-IV_<prefix>_<PH>.bootstrap.tre`**  
  The species tree generated by ASTRAL-IV and bootstrapped using ASTRAL-III, following the [ASTER](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral4.md) protocol.
  - **`ASTRAL-IV_<prefix>_<PH>.bootstrap.rr.tre`**  
  The rerooted species tree generated by ASTRAL-IV and bootstrapped using ASTRAL-III.
  - **`ASTRAL-III_LPP.log`**  
  The ASTRAL-III log file which documents the bootstrapping process performed by ASTRAL-III.

###### **`wASTRAL/`**
A directory containing the final species tree for the `<PH>` dataset, generated by wASTRAL.
  - **`wASTRAL_<prefix>_<PH>.tre`**  
  The species tree inferred by wASTRAL from the combined gene trees.
  - **`wASTRAL_<prefix>_<PH>.log`**  
  The log file generated by wASTRAL.
  - **`wASTRAL_<prefix>_<PH>.rr.tre`**  
  The rerooted species tree generated by wASTRAL.

##### `08-Coalescent_analysis` -> `<PH>` -> `04-Rerooted_gene_trees` 
A directory containing rerooted gene trees from the `<PH>` dataset.
- **`<ortholog_group_name>.rr.tre`**  
File containing the rerooted gene tree for `<ortholog_group_name>` alignments in the `<PH>` dataset, generated using Phyx or the [MAD method](https://www.sciencedirect.com/science/article/abs/pii/S1055790322000471).

##### `08-Coalescent_analysis` -> `<PH>` -> `05-PhyParts_PieCharts`    
A directory containing phylogenetic concordance analysis results using rerooted gene trees and species trees.  
###### **`ASTRAL-IV/`**  
A directory containing ASTRAL-IV species tree conflict assessment using rerooted gene trees from directory `04-Rerooted_gene_trees` (created only when users choose to run ASTRAL-IV by setting `-sp_tree 4`).  
  - **`ASTRAL_PhyParts.*`**  
  Files containing the PhyParts output (more details can be found [here](https://bitbucket.org/blackrim/phyparts/src/master/)).
  - **`ASTRAL_PhyPartsPieCharts_<prefix>_<PH>.svg`**  
  Visualization of concordance and conflict between gene trees and the species tree, generated by our newly developed [modified_phypartspiecharts.py](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Extension-Tools#6-modified_phypartspiechartspy) script.

###### **`wASTRAL/`**  
A directory containing wASTRAL species tree conflict assessment using rerooted gene trees from directory `04-Rerooted_gene_trees` (created only when users choose to run wASTRAL by setting `-sp_tree 5`).  
  - **`wASTRAL_<prefix>_<PH>.tre`**
  The species tree inferred by wASTRAL from rerooted `<PH>` gene trees.   
  - **`wASTRAL_<prefix>_<PH>_sorted_rr.tre`**  
  The final rerooted species tree, rerooted by Phyx and sorted by Newick_Utilities.

---
## Comprehensive output
### `hybsuite_logs`
A directory containing the comprehensive log file generated by HybSuite.
```
hybsuite_logs/
└── hybsuite_<current_time>.log   
```
- **`hybsuite_<current_time>.log`**  
The log file produced when running the HybSuite pipeline (running the extension tools will not produce this logfile).

### `hybsuite_checklists`
A directory containing checklist files, including species checklists and locus checklists.
```
hybsuite_checklists/
├── All_Spname_list.txt
├── My_Spname.txt
├── Outgroup.txt
├── Pre-assembled_Spname.txt
├── Public_Spname.txt
├── Public_Spname_SRR.txt
├── Recovered_locus_num_for_samples.tsv
├── Recovered_sample_num_for_loci.tsv
└── Ref_gene_name_list.txt

```

- **`All_Spname_list.txt`**  
A file containing all sample names from your research.
- **`My_Spname.txt`**  
A file containing all sample names for user-provided raw data in your research.
- **`Outgroup.txt`**  
A file containing all outgroup taxa specified by the user.
- **`Pre-assembled_Spname.txt`**  
A file containing the names of all pre-assembled samples specified by the user.
- **`Public_Spname.txt`**  
A file containing all sample names whose Next Generation Sequencing (NGS) raw data was downloaded from NCBI.
- **`Public_Spname_SRR.txt`**  
A file containing all Sequence Read Archive (SRA) IDs used to download NGS raw data from NCBI. These SRA IDs correspond with the sample names listed in the `Public_Spname.txt` file.
- **`Recovered_locus_num_for_samples.tsv`**  
A file containing the number of recovered loci by HybPiper for each sample.
- **`Recovered_sample_num_for_loci.tsv`**  
A file containing the number of recovered samples by HybPiper for each locus.
- **`Ref_gene_name_list.txt`**  
A file containing the names of all genes in the target sequences (specified by the `-t` option).

### `hybsuite_reports`
A directory containing comprehensive statistical summaries of the results generated by the pipeline.
```
hybsuite_results/
├── Alignments_stats
    ├── <PH>-01_Alignments_stats_AMAS.tsv
    ├── <PH>-02_Trimmed_alignments_stats_AMAS.tsv
    ├── <PH>-03_Removed_alignments_without_parsimony_informative_sites.txt
    ├── <PH>-04_Removed_alignments_with_length_less_than_4.txt
    ├── <PH>-05_Removed_alignments_with_sample_number_less_than_5.txt
    ├── <PH>-06_Final_alignments_list.txt
    └── <PH>-07_Final_alignments_stats_AMAS.tsv
└── Supermatrix_stats
    └── <PH>-Supermatrix_stats_AMAS.tsv
```

#### `hybsuite_reports` -> `Alignments_stats`

- **`<PH>-01_Alignments_stats_AMAS.tsv`**  
Summary table of orthogroup alignments inferred via the `<PH>` paralog-handling method (generated by `AMAS.py`).

- **`<PH>-02_Trimmed_alignments_stats_AMAS.tsv`**  
Summary table of trimmed orthogroup alignments inferred via the `<PH>` paralog-handling method (generated by `AMAS.py`).

- **`<PH>-03_Removed_alignments_without_parsimony_informative_sites.txt`**  
List of alignments without parsimony informative sites. These alignments are removed for downstream species tree inference.

- **`<PH>-04_Removed_alignments_with_length_less_than_4.txt`**  
List of alignments with base pair length less than 4. These alignments are removed for downstream species tree inference.

- **`<PH>-05_Removed_alignments_with_sample_number_less_than_5.txt`**  
List of alignments with fewer than 5 samples. These alignments are removed for downstream species tree inference.

- **`<PH>-06_Final_alignments_list.txt`**  
List of final `<PH>` alignments selected for downstream species tree inference.

- **`<PH>-07_Final_alignments_stats_AMAS.tsv`**  
Summary table of final `<PH>` alignments for downstream species tree inference (generated by `AMAS.py`).
> **Filtering process:** Alignments without parsimony-informative sites, low bp length, and with low sample number are removed.

#### `hybsuite_reports` -> `Supermatrix_stats`

- **`<PH>-Supermatrix_stats_AMAS.tsv`**  
Summary table of final `<PH>` supermatrix for downstream concatenation-based species tree inference (generated by `AMAS.py`).
