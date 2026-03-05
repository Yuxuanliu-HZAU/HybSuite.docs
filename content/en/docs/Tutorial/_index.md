---
title: "Tutorial"
weight: 10
menu:
  main:
    identifier: tutorial   # 必须唯一
    name: "Tutorial"
    weight: 3
---

This page helps you prepare input files, configure parameters, and run HybSuite.

* [1. Prepare input files](#1-prepare-input-files)
  * [(1) The sample list file](#1-the-sample-list-file)
  * [(2) The directory containing sequence files](#2-the-directory-containing-sequence-files-specified-by--input_data)
  * [(3) The target sequence file](#3-the-target-sequence-file)
* [2. Construct command](#2-construct-command)
  * [(1) Basic command pattern](#1-basic-command-pattern)
  * [(2) Available subcommands](#2-available-subcommands)
* [3. Configure parameters and run the pipeline](#3-configure-your-parameters)
  * [HybSuite checking](#hybsuite-checking)
  * [Run `hybsuite stage1`](#run-hybsuite-stage1)
  * [Run `hybsuite stage2`](#run-hybsuite-stage2)
  * [Run `hybsuite stage3`](#run-hybsuite-stage3)
  * [Run `hybsuite stage4`](#run-hybsuite-stage4)
  * [Run `hybsuite full_pipeline`](#run-hybsuite-full_pipeline)
  * [Additional tips](#additional-tips)
* [4. Tips for rerunning HybSuite](#4-tips-for-rerunning-hybsuite)
  * [(1) Remove or add samples](#1-remove-or-add-samples)
  * [(2) Reuse existing intermediate data](#2-reuse-existing-intermediate-data)
  * [(3) Adjust sequence filtering thresholds](#3-adjust-sequence-filtering-thresholds)
  * [(4) Steps control in concatenation-based and coalescent-based analysis](#4-steps-control-in-concatenation-based-and-coalescent-based-analysis)
---
## 1. Prepare input files
> [!TIP]
> The HybSuite pipeline requires **three main inputs**, including:    
> - (1) a sample list file   
> - (2) a directory containing sequence files (raw NGS data and pre-assembled sequences)  
> - (3) a target sequence file   
### **(1) The sample list file**
This file should document sample names along with their corresponding sequence types identifiers (seperated by `\t`). HybSuite pipeline supports three input sequence types:
    
#### **Type1: public raw reads from NCBI SRA**     

To download NGS raw reads from NCBI for phylogenetic analysis, you should:

Format the sample list file as:

- Row 1: Sample names
- Row 2: Corresponding accession numbers

(Tab-delimited, one sample per column)

```
Taxon1	SRR...
Taxon2	SRR...
...
```
<br>

#### **Type2: user-provided raw reads**

To prepare existing samples' NGS raw reads as input, you should:

**1. Format the sample list file as:**

- Row 1: Sample names
- Row 2: Character `A`

(Tab-delimited, one sample per column)

```
Taxon3	A
Taxon4	A
...
```

**2. Place the raw data files (paired-end/single-end, `FASTQ` or `FASTQ.GZ` format) with corresponding names in the `-input_data` directory** (see naming rules [here](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Tutorial#2-the-directory-containing-sequence-files-specified-by--input_data) ).

<br>

#### **Type3: pre-assembled sequences**

To prepare pre-assembled sequences as input, you should:

**1. Format the sample list file as:**

- Row 1: Sample names
- Row 2: Character `B`

(Tab-delimited, one sample per column)
```
Taxon5	B
Taxon6	B
...
```
**2. Place the corresponding pre-assembled sequence files in the `-input_data` directory** (see naming rules [here](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Tutorial#2-the-directory-containing-sequence-files-specified-by--input_data)).

---

### Combine sequence types together

To include multiple sequence types showed above as pipeline inputs, just combine all entries into the sample list file:
```
Taxon1	SRR...
Taxon2	SRR...
Taxon3	A
Taxon4	A
Taxon5	B
Taxon6	B
```

### Outgroup Specification
This step is **not necessary** if you only run stage1 and stage2, but **required** for stages 3-4 (orthology inference & species tree inference):

To specify the outgroup in the sample list file, just mark outgroups with chracters `Outgroup` in row 3 (tab-separated with row 2).
Example (Outgroup = Taxon5):
```
Taxon1	SRR...
Taxon2	SRR...
Taxon3	A
Taxon4	A
Taxon5	B	Outgroup
Taxon6	B
```

> [!NOTE]
> - The order of recording the information of these three sequence types doesn't matter.    
> - The sample list file can be named with different suffix, such as `.txt`, `.tsv` ...    

> [!WARNING] 
> - At least one sequence type should be recorded in the sample list file.      
> - You have to make sure all the sample names in the first row are not dupilicated.     
> - Any mistake in the second row's characters will lead to running errors.
---
### (2) The directory containing sequence files
Required if:

Your sample list includes existing raw reads or pre-assembled sequences.     

#### 1. Naming rules for existing raw reads files in the input directory:

If `<Taxon>` is listed in the sample list file, its raw data file should be named as follows:    
- **Paired-end data:** `<Taxon>_1.<suffix>` + `<Taxon>_2.<suffix>`   
- **Single-end data:** `<Taxon>.<suffix>`
> [!TIP] 
> - **Both paired-end and single-end data are supported.**     
> - **Supported `<suffix>`:** `.fq.gz`, `.fq`, `.fastq.gz`, or `.fastq`.    

> [!NOTE] 
> Filenames must exactly match the sample names in your list. Mismatches will cause errors.                

#### 2. Naming rules for pre-assembled sequences in the input directory:
If `<Taxon>` is listed in the sample list file, its pre-assembled sequence file should be named as follows:
- `<Taxon>.fasta`

---
### (3) The target sequence file
- The required format for the target sequences file is nearly identical to that used in [HybPiper](https://github.com/mossmatters/HybPiper).
- The only difference is that in HybSuite, the gene name for a target sequence must be placed immediately after the final hyphen (`-`) in the header line (see example showed below).
- For example, see the `Reference.fasta` file in [the example dataset](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Test-dataset#1-files-in-input).
- For more details, refer to [HybPiper's documentation](https://github.com/mossmatters/HybPiper#target-sequences) to edit your target file.
      
```
>Elaeagnus-pungens-4471
AATGTCATCCAGGATAAATATCGGTTGGAAGCTGCAAATACTGACTGGATGAACAAGTAC
AAAGGCTCTAGTAAGCTTCTATTGCATCCAAGGAACACTGAGGAGGTTTCACAGATACTC
...
>Hippophae-rhamnoides-4527
GAAGAGAGGGTTGTAGTATTAGTGATTGGTGGAGGAGGAAGAGAACATGCTCTTTGCTAT
GCAATGAATCGATCACCATCCTGCGATGCAGTCTTTTGTGCTCCTGGCAATGCTGGGATT
...
>Hippophae-salicifolia-4691
CAGAGACTGCCTCCATTGTCAACTGATCCCAACAGATGCGAGCGTGCATTTGTTGGAAAC
ACGATAGGTCAAGCAAATGGTGTGTACGACAAGCCAATCGATCTCCGATTCTGTGATTAC
...
```
---

## 2. Construct command
### (1) Basic command pattern

**Conda version:**
```bash
hybsuite <subcommand> [options] ...
```

**Local version:**
```bash
bash <path to HybSuite.sh> <subcommand> [options] ...
```

### (2) Available subcommands

HybSuite provides modular subcommands for flexible workflow execution:

#### **Run individual stages:**
```bash
hybsuite stage1 [options]...    # run Stage 1: NGS dataset construction
hybsuite stage2 [options]...    # run Stage 2: Data assembly and filtering
hybsuite stage3 [options]...    # run Stage 3: Paralog handling
hybsuite stage4 [options]...    # run Stage 4: Species tree inference
```

#### **Run the full pipeline:**
```bash
hybsuite full_pipeline [options]...    # Execute stages 1-4 in one go
```

#### **Retrieve results:**
After completing the full pipeline from stage 1 to 4, retrieve key output files:
```bash
hybsuite retrieve_results -i <hybsuite_comprehensive_output_dir> -o <results_dir>
```
This subcommand collects all final trees and summary statistics from the HybSuite comprehensive output directory.

> [!TIP]
> Use `hybsuite <subcommand> --help` or refer to the [Full pipeline parameters](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Full-pipeline-parameters) to view detailed parameters for each subcommand.
---
## 3. Configure your parameters

This section guides you through running each stage sequentially and how to configure related parameters.

Suppose you have prepared all the required input files including [**the sample list file**](#1-the-sample-list-file-specified-by--input_list-or--i), [**the input sequence directory**](#2-the-directory-containing-sequence-files), and [**the target sequence file**](#3-the-target-sequence-file) in the correct formats described above, then you can keep forward and run each stage sequentially or the full pipeline in one go.

---

### HybSuite checking

Before running each stage or the full pipeline, HybSuite automatically checks all dependencies, configured parameters, and sample information. If any invalid parameters or incorrectly formatted input files are detected, the program notifies you and exits.

After the checks, the program prompts you to confirm whether to proceed with running HybSuite (`y`) or not (`n`).

To skip checking, specify `-check` as `FALSE` when running the pipeline.

---

### Run `hybsuite stage1`

**Purpose:** running `HybSuite Stage 1: "NGS dataset construction"`: download public raw reads, integrate user-provided data, and perform adapter trimming.

##### **Step 1: Configure mandatory parameters**

The following parameters must be specified when running `hybsuite stage1` (failing to do so will cause HybSuite to exit during execution):

**(1) Configure input file parameters:**
- **`-input_list <FILE>`**     
Specify the sample list file (as described in the section [(1) The sample list file](#1-the-sample-list-file))     
- **`-input_data <DIR>`**     
Specify the directory including all user-provided raw reads and pre-assembled sequences (no need to specify this option if the sample names of user-provided raw reads and pre-assembled sequences are not included in the sample list file).     

**(2) Configure output file parameters:**
- **`-output_dir <DIR>`**     
Specify the directory for storing HybSuite’s comprehensive output files. It is recommended to specify this option as the same directory for convenience in later stages to make all outputs be in the same output folder.

##### **Step 2: Configure essential parameters**

The following parameters are essential for running this stage, you'd better check whether to configure depending on your analysis.

- **`-nt <NUM>`**      
Number of threads to use for HybSuite. All integrated tools will use this value (default: `1`).      
- **`-process <NUM>`**     
Specify the number of parallel sample processing (Default: `1`). This involves the steps **"raw read downloading" and "adapter trimming"** in Stage 1.     
- **`-sra_maxsize <?GB>`**     
Specify the maximum SRA file size to download (Default: `20GB`). If the size of targeted raw read files to download from NCBI is larger than this value, downloading process will be skipped for corresponding samples. 
- **`-NGS_dir <DIR>`**     
Specify the output directory with raw and clean reads files (Default: `<output_dir>/NGS_dataset` (`<output_dir>` can be specified by `-output_dir`))

##### **Step 3: Configure other parameters** 

See the **full parameters for `running hybsuite stage1`** [here](/docs/Full_parameters/#parameters-for-running-hybsuite-stage1) for more customizable settings.

**Example command:**
```bash
# basic common pattern:
hybsuite stage1 -input_list <FILE> -input_data <DIR> -output_dir <DIR> -nt <NUM>or<AUTO> -process <NUM>

# command for our example datset (Angiosperms353) (8 threads; 5 in parallel)
cd <Path_to_"HybSuite-master/example_datasets/Angiosperms353/">
hybsuite stage1 -input_list Input_list.txt ./-input_data ./Input_data -output_dir ./Output -nt 8 -process 5
```

##### **Step 4: Check output files**
After running stage 1, you can check the output files for your analysis.  
See the **output files for `hybsuite stage1`** [here](/docs/Output_files/#stage1-output) for more details.

---

### Run `hybsuite stage2`

**Purpose:** run `HybSuite Stage 2: "Data assembly and paralog retrieval"`: assemble reads using HybPiper, retrieve paralog sequences, and filter paralog sequences by length, sample or locus coverage.

##### **Step 1: Configure mandatory parameters**    
**(1) Input file parameters**    
- **`-input_list <FILE>`**      
Sample list file (same as stage 1) [here to check](#1-the-sample-list-file).     
- **`-NGS_dir <DIR>`**       
NGS dataset outputted in stage 1.  
**(default: `<output_dir>/NGS_dataset`)**      
- **`-input_data <DIR>`**     
The same parameter as specified in Stage 1.    
This option is required only when pre-assembled sequences are provided as input.      
- **`-t <FILE>`**     
Target sequence file in HybPiper format.     

> [!TIP] 
> If you have existing cleaned read files, you can place them in the directory `<NGS_dir>/03-My_clean_data` (`<NGS_dir>` is the directory specified by `-NGS_dir`) for saving time because HybSuite will skip processing these samples to save running time!      

For example, if you have existing clean data (pair-ended) of **`<taxon1>`** and clean data (single-ended) of **`<taxon2>`**, you can place them with corresponding file names showed below to let HybSuite skip processing these two samples:   
```
<NGS_dir>/
├──01-Downloaded_raw_data
├──02-Downloaded_clean_data
└──03-My_clean_data
    ├──<taxon1>_1_clean.paired.fq.gz
    ├──<taxon1>_2_clean.paired.fq.gz
    └──<taxon2>_clean.paired.fq.gz
```

**(2) Output file parameters**
- **`-output_dir <DIR>`**     
Directory to store output files. Using the same directory across stages is recommended for convenience, though different directories are allowed.    

- **`-eas_dir <DIR>`**   
Specify the output directory for storing assembled results (one sample per subdirectory) generated by `hybpiper assemble` (see [HybPiper manual](https://github.com/mossmatters/HybPiper/wiki/Results-and-output-files#10-hybpiper-assemble)).  
**(default: `<output_dir>/NGS_dataset`)**
> [!TIP] 
> - If you already have assembled results generated by [`hybpiper assemble`](https://github.com/mossmatters/HybPiper/wiki/Results-and-output-files#10-hybpiper-assemble) (one sample per subdirectory), you can place them in the directory specified with `-eas_dir`. HybSuite will automatically skip these samples during the data assembly stage.    

For example, if you have directories with assembled sequences outputted by [`hybpiper assemble`](https://github.com/mossmatters/HybPiper/wiki/Results-and-output-files#10-hybpiper-assemble) for `<taxon1>` and `<taxon2>`:
- Create a directory and place these two directories named `<taxon1>` and `<taxon2>` in it, and specify `-eas_dir` as the path to this new directory (showed below, `<eas_dir>` is the directory specified by `-eas_dir`).

```
<eas_dir>/
├──<taxon1>
└──<taxon2>
```
- Then, include the name of `<taxon1>` and `<taxon2>` in the sample list file (specified by `-input_list`).


##### **Step 2: Configure essential parameters**

The following parameters are optional but essential for running this stage, check whether to configure depending on your analysis.     

**(1) Thread and parallel:**
- **`-nt <NUM|AUTO>`**     
Number of threads to use for HybSuite. All integrated tools will use this value (default: `1`).    
- **`process <NUM>`**     
Specify the number of parallel sample processing (Default: `1`). This involves the steps **"data assembly"** in stage 2.     
- **`-eas_dir <DIR>`**     
Specify existing assembled data to skip redundant assembly.     

**(2) Paralog sequence filtering:**
- **`-seqs_min_sample_coverage <NUM:0-1>`**     
Specify the minimum sample coverage of recovered loci. Loci with sample coverage below this threshold are removed.  
**(default: `0`, recommended value: `0.1`)**     
- **`-seqs_min_locus_coverage <NUM:0-1>`**    
Specify the minimum locus coverage of samples. Samples with locus coverage below this threshold are removed.  
**(default: `0`)**     
- **`-seqs_min_length <NUM>`**       
Specify the minimum sequence length for filtering paralog sequences. Sequences shorter than this threshold are removed.  
**(default: `0`, recommended value: `100`)**     
- **`-seqs_mean_length_ratio <NUM:0-1>`**        
Specify the minimum length ratio relative to the mean length of target sequences. Sequences with a length ratio below this threshold are removed. **(default: `0`)**     
- **`-seqs_max_length_ratio <NUM:0-1>`**     
Specify the minimum length ratio relative to the maximum length of target sequences. Sequences with a length ratio below this threshold are removed. **(default: `0`)**     
- **`-seqs_min_length_ratio <NUM:0-1>`**       
Specify the minimum length ratio relative to the minimum length of target sequences. Sequences with a length ratio below this threshold are removed. **(default: `0`)**     

**(3) Parameters related to `hybpiper assemble`:**
- **`-hybpiper_mapping_tool <blast|diamond>`**         
Specify the read mapping tool used in data assembly via `hybpiper assemble`.  
**(default: `blast`)**    
- **`-hybpiper_mapping_tool` `<TRUE|FALSE>`**     
Specify whether to check chimeric contigs.   
**(default: `TRUE`)**    
- **`-hybpiper_cov_cutoff <INT>`**   
Coverage cutoff for SPAdes when running "hybpiper assemble" in Stage 2.   
**Increasing this value may increase the loci recovery efficiency but potentially introducing errors.**  
**(Default: `8`)**

##### **Step 3: Configure other parameters** 

See the **full parameters for `running hybsuite stage2`** [here](/docs/Full_parameters/#parameters-for-running-hybsuite-stage2) for more customizable settings.

**Example command**
```bash
# Recommended command mode
hybsuite stage2 \
  -input_list <FILE> \
  -NGS_dir <DIR> \
  -t <FILE> \
  -output_dir <DIR> \
  -seqs_min_length <NUM> \
  -seqs_min_sample_coverage <NUM> \
  -nt <NUM> -process <NUM>

# Command for the example dataset (Angiosperms353)
hybsuite stage2 \
  -input_list ./Input_list.txt \
  -NGS_dir ./NGS_dataset \
  -t ./Target_file_Angiosperm353.fasta \
  -output_dir ./Output \
  -seqs_min_length 100 \
  -seqs_min_sample_coverage 0.1 \
  -nt 8 -process 5
```

##### **Step 4: Check output files**
After running stage 2, you can check the output files for your analysis.  
See the **output files for `hybsuite stage2`** [here](/docs/Output_files/#stage2-output) for more details.

---

### Run `hybsuite stage3`

**Purpose:** run `HybSuite Stage 3: "Paralog handling"`: optionally apply seven paralog-handling methods to infer orthology groups and generate final alignments for stage 4: `species tree inference`.

##### **Step 1: Configure mandatory parameters:**

**(1) Input file parameters**
- **`-input_list <FILE>`**      
Sample list file (same as stage 1) [here to check](#1-the-sample-list-file).     
- **`-eas_dir <DIR>`**       
Directory containing assembled sequences (one sample per subdirectory) generated by `hybpiper assemble` (see [HybPiper manual](https://github.com/mossmatters/HybPiper/wiki/Results-and-output-files#10-hybpiper-assemble)) in Stage 2 or provided by users. (default: `<output_dir>/NGS_dataset`)   
- **`-paralogs_dir <DIR>`**     
Directory containing all paralog sequences generated in Stage 2 or provided by users.
If Stage 2 has been executed, set this parameter to `<output_dir>/02-All_paralogs/03-Filtered_paralogs`,
where `<output_dir>` refers to the comprehensive output directory specified by `-output_dir`.  
**(default: none)**
- **`-input_data <DIR>`**     
The same parameter as specified in Stage 1.    
This option is required only when pre-assembled sequences are provided as input and the "HRS" or "RLWP" methods are selected.      
- **`-t <FILE>`**     
Target sequence file in HybPiper format.

**(2) Output file parameters**
- **`-output_dir <DIR>`**     
Directory to store output files. Using the same directory across stages is recommended for convenience, though different directories are allowed.
- **`-prefix <STRING>`**  
Output file prefix. **(default: `HybSuite`)**

##### **Step 2: Configure essential parameters:**

The following parameters are optional but essential for running this stage, check whether to configure them based on your analysis.

**(1) Thread and parallel:**
- **`-nt <NUM|AUTO>`**     
Number of threads to use for HybSuite. All integrated tools will use this value (default: `1`).    
- **`-process <NUM>`**     
Specify the number of parallel sample processing (Default: `1`). This involves the steps **"data assembly"** in stage 2.

**(2) Paralog-handling methods applied in this stage:**
- **`-PH <1-7|a|b|all>`**  
Paralog handling methods (1=HRS, 2=RLWP, 3=LS, 4=MI, 5=MO, 6=RT, 7=1to1, a=PhyloPyPruner, b=ParaGone)  
**(One or several methods can be specified; default: `1a`)**

**(3) Sequence filtering:**
- **`-seqs_min_length <INT>`**  
Minimum HRS/RLWP sequence length **(Default: `0`)**  
Only HRS and RLWP methods filter sequences in this satge, other paralog-handling methods filter sequences in stage 2.
- **`-aln_min_length <INT>`**  
Minimum alignment length **(Default: `4`)**
- **`-aln_min_sample <INT>`**  
Minimum sample number per alignment **(Default: `0`)**

**(4) Gene tree construction:**
- **`-gene_tree <1/2>`**  
Gene tree builder: 1=IQ-TREE, 2=FastTree **(Default: `1`)**
- **`-gene_tree_bb <INT>`**  
Bootstrap value **(Default: `1000`)**
- **`-trim_tool <1/2>`**  
Trimming tool: 1=trimAl, 2=HMMCleaner **(Default: `1`)**

##### **Step 3: Configure other parameters** 

See the **full parameters for `running hybsuite stage3`** [here](/docs/Full_parameters/#parameters-for-running-hybsuite-stage3) for more customizable settings.

**Example command:**
```bash
# Recommended command
hybsuite stage3 \
  -input_list <FILE> \
  -eas_dir ./01-Assembled_data \
  -paralogs_dir ./02-All_paralogs/03-Filtered_paralogs \
  -t <FILE> \
  -output_dir <DIR> \
  -PH 1234567a \  # this option can be determined by your data size, time schedule, but still suggested to try all methods, including "4567b" with ParaGone
  -nt 8 -process 5

# Command for the example dataset (Angiosperms353)
hybsuite stage3 \
  -input_list ./Input_list.txt \
  -eas_dir ./01-Assembled_data \
  -paralogs_dir ./02-All_paralogs/03-Filtered_paralogs \
  -t Target_file_Angiosperm353.fasta \
  -output_dir ./Output \
  -PH 1234567a \
  -mafft_algorithm linsi \
  -nt 8 -process 5
```

##### **Step 4: Check output files**
After running stage 3, you can check the output files for your analysis.  
See the **output files for `hybsuite stage3`** [here](/docs/Output_files/#stage3-output) for more details.

---

### Run `hybsuite stage4`

**Purpose:** run `HybSuite Stage4: "Species tree inference"`, infer species trees using concatenation-based and/or coalescent-based approaches.

##### **Step 1: Configure mandatory parameters:**

**(1) Configure input file parameters**  
- **`-input_list <FILE>`**      
Sample list file (same as stage 1-3) [here to check](#1-the-sample-list-file).
- **`-aln_dir <DIR>`**  
The path to directory `06-Final_alignments` with final alignments generated in stage 3.

**(2) Configure output file parameters**
- **`-output_dir <DIR>`**     
Directory to store output files. Using the same directory across stages is recommended for convenience, though different directories are allowed.

##### **Step 2: Configure essential parameters:**

The following parameters are optional but essential for running this stage, check whether to configure them based on your analysis.  
- **`-PH <1-7|a|b|all>`**  
Select alignments from one or more specific paralog-handling methods. Make it consistent with the one used in stage 3. Default: `1`.  
- **`-sp_tree <1-6|all>`**  
Species tree inference methods. Default: `1`.  
1=IQ-TREE, 2=RAxML, 3=RAxML-NG, 4=ASTRAL-IV, 5=wASTRAL, 6=ASTRAL-Pro
- **`-nt <INT|AUTO>`**  
Thread setting.

**Gene tree construction (for coalescent methods):**
- **`-gene_tree <1/2>`**  
Gene tree builder. Default: `1`.
- **`-gene_tree_bb <INT>`**  
Bootstrap value. Default: `1000`.
- **`-collapse_threshold <VALUE>`**  
Collapse weakly supported branches. Default: `0`.

**Concatenation-based methods:**
- **`-run_modeltest_ng <TRUE/FALSE>`**  
Run ModelTest-NG. Default: `TRUE`.
- **`-iqtree_bb <INT>`**  
IQ-TREE bootstrap replicates. Default: `1000`.
- **`-iqtree_partition <TRUE/FALSE>`**  
Use partition models. Default: `TRUE`.

**Coalescent-based methods:**
- **`-astral_r <INT>`**  
ASTRAL-IV search rounds. Default: `4`.
- **`-wastral_mode <1-4>`**  
wASTRAL weighting mode. Default: `1`.
- **`-run_phyparts <TRUE/FALSE>`**  
Run PhyParts analysis. Default: `TRUE`.

**Example command:**
```bash
# Concatenation-based method (IQ-TREE)
hybsuite stage4 \
  -input_list <FILE> \
  -aln_dir <output_dir>/06-Final_alignments \
  -output_dir <DIR> \
  -PH 1234567a \
  -sp_tree 1 \
  -nt 8

# Coalescent-based method (ASTRAL-IV)
hybsuite stage4 \
  -input_list sample_list.txt \
  -aln_dir <output_dir>/06-Final_alignments\
  -output_dir <DIR> \
  -PH 1a \
  -sp_tree 4 \
  -run_phyparts TRUE \
  -nt 8 -process 5
```

---

### Run `hybsuite full_pipeline`

**Purpose:** run stages 1-4 sequentially in a single command.

> [!NOTE]
> Running the full pipeline with a single command is convenient, but it can also be risky if intermediate outputs aren’t carefully checked.
> Therefore, We recommend running each stage separately first to review the results and understand the workflow before using this "one-to-end" execution.

##### **Step 1: Configure mandatory parameters:**

- **`-input_list <FILE>`**  
Sample list file.
- **`-t <FILE>`**  
Target file.
- **`-output_dir <DIR>`**  
Output directory.
> `-input_data` is required when including user-provided data.

##### **Step 2: Configure essential parameters:**

**Methods control:**
- **`-PH <1-7|a|b|all>`**  
Paralog handling methods. Default: `1a`.
- **`-sp_tree <1-6|all>`**  
Species tree inference methods. Default: `1`.

**Threading:**
- **`-nt <INT|AUTO>`**  
Global thread setting. Default: `1`.
- **`-process <INT|all>`**  
Parallel processing. Default: `1`.

**Workflow control:**
- **`-skip_stage <1|12|123>`**  
Skip completed stages. Default: `none`.
- **`-run_to_stage <1|2|3|4>`**  
Stop at specific stage. Default: `4`.

**Logging control:**
- **`-log_mode <simple|cmd|full>`**  
Logging verbosity. Default: cmd.
  - `simple`: Only log key information
  - `cmd`: Log key information + command history
  - `full`: Log detailed information + command history

**Example command:**
```bash
# Complete pipeline with all paralog-handling methods
hybsuite full_pipeline \
  -input_list sample_list.txt \
  -input_data Input_data \
  -t Angiosperms353.fasta \
  -output_dir ./ \
  -PH 1234567ab \
  -sp_tree 12345 \
  -seqs_min_length 100 \
  -aln_min_sample 4 \
  -nt AUTO -process 10

# Pipeline with existing NGS data, skip stage 1
hybsuite full_pipeline \
  -input_list sample_list.txt \
  -NGS_dir ./NGS_dataset \
  -t Angiosperms353.fasta \
  -output_dir ./ \
  -skip_stage 1 \
  -PH 1a \
  -sp_tree 14 \
  -nt 8 -process 5
```
##### **Step 3: Configure other parameters** 

See the **full parameters for `run hybsuite full_pipeline`** [here](/docs/Full_parameters/#parameters-for-running-hybsuite-full_pipeline) for more customizable settings.


---

## 4. Tips for rerunning the pipeline

If the initial results of HybSuite are not satisfactory, you can rerun the pipeline with modified parameters. The following strategies can help improve results or reduce runtime. These methods can be used individually or combined.

### (1) Remove or add samples

You can remove or add samples by editing the `sample_list.txt` file and rerunning HybSuite.

If the same `output_dir` is used, HybSuite will automatically detect completed samples and skip the following steps:  

- **Public data downloading (stage 1)**
- **Raw reads trimming (stage 1)**
- **Data assembly (stage 2)**
    
This allows you to update the dataset without repeating previously completed computations.

---

### (2) Reuse existing intermediate data

HybSuite allows reuse of intermediate results from previous runs.

Use the following options:

- **`-NGS_dir`**  
    Use the `NGS_dataset` directory generated by a previous run to skip **data downloading and adapter trimming**.

- **`-eas_dir`**  
    Use the `02-All_paralogs` directory generated by a previous run to skip the **data assembly step**.

Reusing intermediate data can significantly reduce runtime when rerunning analyses.

---

### (3) Adjust sequence filtering thresholds

You can improve dataset quality by adjusting filtering thresholds when rerunning the pipeline.

Common parameters include:

- `-seqs_min_length`  
    Minimum sequence length retained in stage 2.
- `-seqs_min_sample_coverage`  
    Minimum proportion of samples containing a sequence.
- `-aln_min_sample`  
    Minimum number of samples required for an alignment in stage 3.
    
Increasing these thresholds can improve alignment quality and downstream analyses.

### (4) Steps control in concatenation-based and coalescent-based analysis
HybSuite allows selective execution of steps in both concatenation-based and coalescent-based analyses.

#### Concatenation-based analysis

Use `-run_concatenated_step` to specify which steps to run.

For example, if a concatenated alignment has already been generated in a previous run, you can skip the concatenation step and directly infer the species tree by setting:

```
-run_concatenated_step 2
```
---

#### Coalescent-based analysis

Use `-run_coalescent_step` to control the execution of coalescent-based analysis.

For example, if gene trees are already available from a previous run, you can skip earlier step `1` and directly infer the species tree by setting:

```
-run_coalescent_step 234
```
