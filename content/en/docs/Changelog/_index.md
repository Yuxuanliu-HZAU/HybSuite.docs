---
title: "Changelog"
weight: 10
menu:
  main:
    identifier: changelog   # 必须唯一
    name: "Changelog"
    weight: 7
---

**1.1.7** *January, 2026*
- **New function: Steps control in stage 4**
  - Added support for controlling individual steps within Stage 4, allowing users to selectively run specific steps (e.g., gene tree inference, alignment trimming, species tree inference) rather than executing the entire stage in one go. [here](/docs/Tutorial/#4-steps-control-in-concatenation-based-and-coalescent-based-analysis) for details.

**1.1.6** *September, 2025*
- **New dependency: Plotly**  
  [Plotly](https://github.com/plotly) has been integrated into the new script [`plot_recovery_heatmap_v2.py`](https://github.com/Yuxuanliu-HZAU/HybSuite/blob/master/bin/plot_recovery_heatmap_v2.py) to generate an interactive HTML heatmap visualizing target locus recovery in Stage 2. This heatmap provides useful guidance for parameter selection in Stages 3–4.

- **TreeShrink integrated into Stage 3**  
  [TreeShrink](https://github.com/uym2/TreeShrink) has been incorporated into Stage 3 as an optional processing step. Users can enable it by setting the option `-run_treeshrink` to `TRUE`. TreeShrink removes genes with excessively long branches and is available for all seven paralog-handling pipelines except ParaGone, as TreeShrink is already implemented within the ParaGone pipeline.

**1.1.5** *September, 2025* - **MAJOR UPDATE !**

- **Pipeline restructuring**    
  - **Stage consolidation**: Combined previous stages 3 and 4, simplifying the pipeline from 5 to 4 stages.
  - **Stagewise execution**: Added flexible stage-by-stage execution capability.

- **Enhanced functionality**    
  **Gene tree inference**:    
  - Added support for [IQ-TREE](https://github.com/iqtree/iqtree2) and [FASTTree](http://www.microbesonline.org/fasttree/).
  - Deprecated RAxML.
  
  **Alignment trimming**:    
  - New alternative: Integrated [HMMCleaner](https://metacpan.org/dist/Bio-MUST-Apps-HmmCleaner/view/bin/HmmCleaner.pl)
  - Maintained trimAl as the default setting.

  **Species tree inference**:    
  - Added [ASTRAL-pro3](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral-pro3.md) for multi-copy gene aware coalescent analysis.

**1.1.3-1.1.4** *August, 2025*

Fixed some bugs in stages control. These versions have been abondoned.

**1.1.2** *August, 2025*

Integrated [ASTRAL-IV](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral4.md) into the pipeline stage 4.           

**Usage Update:**    
- Run [ASTRAL-IV](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral4.md) with parameter: `-tree 4`    

**New dependency:**     
- [ASTER(conda version)](https://github.com/chaoszhang/ASTER)

**1.1.1** *August, 2025*

Fixed some common bugs.