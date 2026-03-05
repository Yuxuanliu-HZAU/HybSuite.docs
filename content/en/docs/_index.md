---
title: "Introduction"
linkTitle: "Introduction"
menu:
  main:
    identifier: home   # must be unique
    name: "Introduction"
    weight: 1
---

This page offers detailed introduction of HybSuite. Feel free to explore!    
* [Pipeline overview](#pipeline-overview)
* [Features](#features)
* [Advantages](#advantages)
---

## 🧬 Pipeline overview {#pipeline-overview}

**HybSuite** performs end-to-end hybrid capture (Hyb-Seq) phylogenomic analysis from raw reads (Hyb-Seq preferred; compatible with RNA-seq, WGS, and genome skimming data) to phylogenetic trees.       
<br>
**The full pipeline is composed of 4 stages:**

![HybSuite workflow](/HybSuite.docs/assets/static/images/HybSuite-workflow.png)

- **Stage 1: NGS dataset construction**    
  - (1) Optionally download public raw reads from NCBI (via [SRA Toolkit](https://github.com/ncbi/sra-tools) );
  - (2) Optionally integrate user-provided raw reads (if provided);
  - (3) Raw reads trimming (via [Trimmomatic](https://github.com/usadellab/Trimmomatic)); <br> 
<br>

- **Stage 2: Data assembly and paralog retrieval**    
  - (1) Target loci assembly and putative paralogs retrieval (via [HybPiper](https://github.com/mossmatters/HybPiper))
  - (2) Integrate pre-assembled sequences (if provided);
  - (3) Filter putative paralogs;
  - (4) Plot recovery heatmap and paralog heatmap of original and filtered sequences;<br>
<br>

- **Stage 3: Paralog handling**    
  - Optionally execute seven paralogs-handling methods (HRS, RLWP, LS, MO, MI, RT, 1to1; see our [Tutorial]({{< relref "/docs/Tutorial/" >}}) and generate filtered alignments for downstream analysis:    
    - **HRS**:    
    (1) Retrieve seqeunces via command `hybpiper retrieve_sequences` in [HybPiper](https://github.com/mossmatters/HybPiper);     
    (2) Integrate pre-assembled sequences (if provided);    
    (3) Filter sequences by length to remove potential mis-assembled seqeunces;    
    (4) Mutiple sequences aligning (via [MAFFT](https://github.com/GSLBiotech/mafft)) and trimming (via [trimAl](https://github.com/inab/trimal) or [HMMCleaner](https://metacpan.org/dist/Bio-MUST-Apps-HmmCleaner/view/bin/HmmCleaner.pl));    
    (5) Filter trimmed alignments to generate final alignments.    
    - **RLWP**:     
    (1) Retrieve seqeunces via `hybpiper retrieve_sequences` via [HybPiper](https://github.com/mossmatters/HybPiper);    
    (2) Integrate pre-assembled sequences (if provided);    
    (3) Filter sequences by length to remove potential mis-assembled seqeunces;   
    (4) Remove loci with putative paralogs masked in more than <threshold> samples;   
    (5) Mutiple sequences aligning (via [MAFFT](https://github.com/GSLBiotech/mafft)) and trimming (via [trimAl](https://github.com/inab/trimal) or [HMMCleaner](https://metacpan.org/dist/Bio-MUST-Apps-HmmCleaner/view/bin/HmmCleaner.pl));    
    (6) Filter trimmed alignments to generate final alignments.    
    - **[PhyloPypruner](https://github.com/fethalen/phylopypruner) pipeline ([LS](https://sourceforge.net/projects/phylotreepruner/), [MI, MO, RT, 1to1](https://bitbucket.org/yangya/phylogenomic_dataset_construction/src/master/))**:    
    (1) Mutiple sequences aligning (via [MAFFT](https://github.com/GSLBiotech/mafft)) and trimming (via [trimAl](https://github.com/inab/trimal) or [HMMCleaner](https://metacpan.org/dist/Bio-MUST-Apps-HmmCleaner/view/bin/HmmCleaner.pl)) for all putative paralogs;    
    (2) Gene trees inference of all putative paralogs;    
    (3) Obtain orthogroup alignments using tree-based orthology inference algorithms (via [PhyloPypruner](https://github.com/fethalen/phylopypruner));    
    (4) Realign (via [MAFFT](https://github.com/GSLBiotech/mafft)) and trim (via [trimAl](https://github.com/inab/trimal) or [HMMCleaner](https://metacpan.org/dist/Bio-MUST-Apps-HmmCleaner/view/bin/HmmCleaner.pl)) the orthogroup alignments;    
    (5) Filter trimmed orthogroup alignments to generate final alignments.    
    - **[ParaGone](https://github.com/chrisjackson-pellicle/ParaGone) pipeline ([MI, MO, RT, 1to1](https://bitbucket.org/yangya/phylogenomic_dataset_construction/src/master/))**:    
    (1) Use the directory cantaining all putative paralogs generated in stage 2 as input;    
    (2) Obtain orthogroup alignments using tree-based orthology inference algorithms via [ParaGone](https://github.com/chrisjackson-pellicle/ParaGone);    
    (3) Filter trimmed orthogroup alignments to generate final alignments. <br>
<br>  
  
- **Stage 4: Species tree inference**
  - Multiple species tree inference methods available:        
    - **Concatenation-based approach:** [IQ-TREE](https://github.com/iqtree/iqtree2), [RAxML](https://github.com/stamatak/standard-RAxML), or [RAxML-NG](https://github.com/amkozlov/raxml-ng);    
    - **Coalescent-based approach:** [ASTRAL-IV](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral4.md) or [wASTRAL](https://github.com/chaoszhang/ASTER/blob/master/tutorial/wastral.md);    
    - **Multi-copy genes aware coalescent-based approach**: [ASTRAL-pro3](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral-pro3.md).    

---
## ✨ Features {#features}

🔄 **Transparent**: Full workflow visibility with real-time progress logging at each step    
📝 **Reproducible**: Automatically archives exact software commands & parameters for every run      
🧩 **Modular**: Execute individual stages or complete pipeline in one command    
⚡ **Flexible**: 7 paralog handling methods & 5+ species tree inference options    
🚀 **Scalable**: Built-in parallelization for large-scale phylogenomic datasets

---

## 🏆 Advantages {#advantages}

### 1. End-to-end pipeline from reads to trees

- Processes data from raw reads to phylogenetic trees with single-command workflows
- Supports both full pipeline execution and modular stage-specific operations
- Minimizes manual intervention while maintaining flexibility

### 2. Unique functionality of integrating pre-assembled sequences

- Allows for integrating pre-assembled loci sequences into the working dataset. (click [here](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Tips-for-running-HybSuite#1-integrating-pre-assembled-sequences) to grasp skills)

### 3. Customizable sequences filtering strategies

- Dual filtering strategies for both loci and samples
- Configurable thresholds for read depth, missing data, and sequence quality
- Enables dataset optimization for different study goals

### 4. Advanced paralog-handling methods

- Implements 7 distinct methods for paralog detection and processing
- Includes both similarity-based and topology-based approaches
- Improves orthology assessment accuracy

### 5. Multi-method Phylogenetic tree inference

- Integrated softwares for concatenation-based methods: [IQ-TREE](https://github.com/iqtree/iqtree2), [RAxML](https://github.com/stamatak/standard-RAxML), and [RAxML-NG](https://github.com/amkozlov/raxml-ng)
- Integrated softwares for coalescent-based methods: [ASTRAL-III](https://github.com/smirarab/ASTRAL) or [wASTRAL](https://github.com/chaoszhang/ASTER/blob/master/tutorial/wastral.md)

### 6. Integrated visualization tools
- `plot_paralog_heatmap.py` (click [here]() to grasp skills);
- `plot_recovery_heatmap.py` (click [here](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Extension-tools#2-plot_recovery_heatmappy) to grasp skills)
- `modified_phypartspiecharts.py` (click [here](https://github.com/Yuxuanliu-HZAU/HybSuite/wiki/Extension-tools#6-modified_phypartspiechartspy) to grasp skills)

### 7. High-Performance Computing

- Parallel processing across samples and loci (option `-process`), which can significantly improve computational efficiency.

