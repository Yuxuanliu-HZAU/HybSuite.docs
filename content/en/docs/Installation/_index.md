---
title: "Installation"
linkTitle: "Installation"
menu:
  main:
    identifier: installation   # 必须唯一
    name: "Installation"
    weight: 2
  # 设置显示左侧 sidebar
sidebar:
  enabled: true
  section: "Installation"
---

**This page tells you how to install HybSuite step by step.**

> [!IMPORTANT]
> HybSuite is a shell-based tool, invoking some python and R scripts, which is only available for Linux/Unix/WSL/macOS users.

> [!TIP]
> Installing HybSuite via conda is strongly recommended, meanwhile manual installation is also available.
---

## 1. Install HybSuite via conda    

### (1) Prerequisites

- [Conda](https://www.anaconda.com/) installation is required. 
If you don't already have conda installed, see [here](https://www.anaconda.com/docs/getting-started/getting-started) for instructions on installing Anaconda or Miniconda.

### (2) Step-by-step installation

To avoid dependency conflicts, **creating a new conda environment for HybSuite** is more recommended:
```
conda create -n hybsuite
```
And then, you can activate your newly-created conda environment and install hybsuite using the command below directly (from the specified channel):
```
conda activate hybsuite
conda install yuxuanliu::hybsuite
```
Before installing hybsuite, you can edit your `~/.condarc` file as this to avoid installation issues about conda channels:
```
channels:
  - conda-forge
  - bioconda
  - yuxuanliu
  - defaults
```

> [!NOTE] 
> Our official bioconda channel package is currently under review. Until approved, using the command showed above is also useful.

### (3) Verification
After installation, you can check the help menu of HybSuite to confirm successful installation by running:
```
hybsuite -h
```

## 2. Install HybSuite manually

### (1) Prerequisites

- [Conda](https://www.anaconda.com/) installation is required. 
If you don't already have conda installed, see [here](https://www.anaconda.com/docs/getting-started/getting-started) for instructions on installing Anaconda or Miniconda.

### (2) Package installtion

Directly clone the [github repository](https://github.com/Yuxuanliu-HZAU/HybSuite.git):
```bash
git clone https://github.com/Yuxuanliu-HZAU/HybSuite.git
```
### (3) Verification
After installation, you can check the help menu of HybSuite to confirm successful installation by running:
```
bash <absolute or relative path to HybSuite.sh> -h
```

### (4) Dependencies installtion

#### Method 1: Run `Install_all_dependencies.sh` to install all dependencies in one go (more recommended)     
The most convenient way to install all dependencies for HybSuite is directly running our script: `HybSuite-master/Install_all_dependencies.sh` to install all desired dependencies.   
Before running this script, activate your target conda environment.
```bash
conda activate <conda_environment_name>
bash HybSuite-master/Install_all_dependencies.sh
```
#### Method 2: Install dependencies manually
If you fail to install some dependencies when running `Install_all_dependencies.sh`, then it is more advisable to install dependencies manually. Just follow the following steps:       
```
conda create -n <conda_environment_name>
conda activate <conda_environment_name>
conda install conda-forge::mamba -y
mamba install python=3.9.15 -y
mamba install bioconda::hybpiper -y
mamba install bioconda::paragone -y
mamba install bioconda::amas -y
mamba install bioconda::sra-tools -y
mamba install conda-forge::pigz -y
conda install conda-forge::plotly -y
mamba install bioconda::newick_utils -y
mamba install bioconda::mafft -y
mamba install bioconda::trimal -y
mamba install bioconda::iqtree -y
mamba install bioconda::raxml -y
mamba install bioconda::raxml-ng -y
mamba install bioconda::aster -y
mamba install r
pip install ete3
pip install PyQt5
pip install phylopypruner
pip install phykit
```
```r
R
install.packages("phytools")
install.packages("ape")
```

---
## 3. Dependencies

* [sra-tools](https://github.com/ncbi/sra-tools)
* [pigz](https://github.com/madler/pigz)
* [HybPiper](https://github.com/mossmatters/HybPiper)>=2.2.0
* [ParaGone](https://github.com/chrisjackson-pellicle/ParaGone)=1.1.1
* [Newick_Utilities](https://github.com/tjunier/newick_utils)
* [MAFFT](https://github.com/GSLBiotech/mafft)>=7.5  
* [trimAl](https://github.com/inab/trimal)
* [ModelTest-NG](https://github.com/ddarriba/modeltest)
* [IQ-TREE](http://trimal.cgenomics.org/)>=3.0.1
* [RAxML](https://github.com/stamatak/standard-RAxML)  
* [RAxML-NG](https://github.com/amkozlov/raxml-ng)
* [ASTER](https://github.com/chaoszhang/ASTER)
* [Plotly](https://github.com/plotly)
* [R](https://www.r-project.org/about.html)≥3.2.0
    * [ape](https://cran.r-project.org/web/packages/ape/index.html)
    * [phytools](https://cran.r-project.org/web/packages/phytools/index.html)
* [Python](https://www.python.org/downloads/)=3.9.15
    * [ete3](http://etetoolkit.org/)
    * [PyQt5](https://pypi.org/project/PyQt5/)
    * [PhyloPyPruner](https://pypi.org/project/phylopypruner/)

