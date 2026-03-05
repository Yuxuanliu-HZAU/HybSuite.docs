---
title: "Example dataset"
weight: 10
menu:
  main:
    identifier: example_dataset   # 必须唯一
    name: "Example dataset"
    weight: 5
---

This page provides detailed instructions on how to run the example dataset included with HybSuite.

---

## 1. Download the example dataset    
If you have downloaded the HybSuite source package, a directory named **`example_dataset`** is already included. In this case, no additional download is required.

Alternatively, you can download the repository on your server using: 
```
git clone https://github.com/Yuxuanliu-HZAU/HybSuite
cd HybSuite/example_dataset
```

## 2. Configure inputs
The directory **`example_dataset`** contains two folders: **`Angiosperms353`** and **`Arabidopsis100`**, respectively encompassing all inputs for running HybSuite pipeline for the corresponding two example datasets in our analyss.     

### **`Example dataset 1: Angiosperms353`**

```
Angiosperms353/
├── Input_list.txt
├── Target_file_Angiosperms353.fasta
├── Input_sequences/
    ├── Elaeagnus_pungens.fasta
    └── Hippophae_rhamnoides.fasta
```
#### `Input_list.txt` 
This file documents taxon names and their corresponding sequence sources (marked in the second row,seperated by `tab` key):
```
Elaeagnus_angustifolia	SRR12569928
Elaeagnus_bambusetorum	SRR27547630
Elaeagnus_henryi	SRR15533155
Elaeagnus_macrophylla	SRR23618743
Elaeagnus_mollis	SRR30566771
Hippophae_neurocarpa	SRR17549374
Hippophae_salicifolia	ERR7621632
Hippophae_tibetana	SRR17549370
Shepherdia_argentea	ERR7621633
Barbeya_oleoides	SRR16214280	Outgroup
Elaeagnus_oldhamii	A
Elaeagnus_pungens	B
Hippophae_rhamnoides	B
```
- Identifiers prefixed with `SRR` or `ERR`: **Public raw NGS data** of the corresponding samples (the first row) ready to be downloaded in HybSuite pipeline.
- Identifier `A`: **User-provided raw NGS data** of the corresponding samples (the first row) ready to be inputted to HybSuite pipeline.
- Identifier `B`: **User-provided pre-assembled sequences** of the corresponding samples (the first row) ready to be inputted to HybSuite pipeline.
- Identifier `Outgroup` : Specifing the outgroup taxon.

> [!NOTE] 
> - In this example dataset, input data include public raw data, user-provided raw data, and pre-assembled sequences,. Thus, corresponding data are provided in the directory `Input_sequences`. 
> - The outgroup taxon is *`Barbeya_oleoides`*.

#### `Input_sequences`
This directory should contain either user-provided raw reads, pre-assembled sequences, or both, according to the information provided in `Input_list.txt`.
- **type1:** `user-provided raw reads`      
In our analysis, only the data of species *`Elaeagnus oldhamii`* belongs to user-provided raw reads, which needs to be downloaded [here](https://ngdc.cncb.ac.cn/bioproject/browse/PRJCA047010) prior to running HybSuite pipeline.
After downloading the raw data, transfer them to FASTQ.GZ format and move them to this directory. The two pair-ended files should be named as:   
```
Elaeagnus_oldhamii_1.fastq.gz
Elaeagnus_oldhamii_2.fastq.gz
```

- **type2:** `pre-assembled sequences`      
Two taxa with pre-assembled sequences are provided: *`Elaeagnus_pungens`*, and *`Hippophae_rhamnoides`* (corresponding to the taxon name along with theidentifier `B` provided in the sample list file `Sample_list.tsv`. Their FASTA files are named as `Elaeagnus_pungens.fasta` and `Hippophae_rhamnoides.fasta` respectively. (`<taxon>.fasta`)

> [!TIP] 
> These two files are directly downloaded from [PAFTOL project](https://treeoflife.kew.org/tree-of-life/):      
> - [**Downloading pre-assembled sequences of `Elaeagnus_pungens`**](https://sftp.kew.org/pub/paftol/current_release/fasta/by_recovery/oneKP.RBYC.Elaeagnus_pungens.a353.fasta)     
> - [**Downloading pre-assembled sequences of `Hippophae_rhamnoides`**](https://sftp.kew.org/pub/paftol/current_release/fasta/by_recovery/INSDC.ERR1294015.Hippophae_rhamnoides.a353.fasta)

#### `Target_file_Angiosperms353.fasta`
This file is the target sequence file for Angiosperms353.     
The gene name for a sequence should be placed immediately after the final hyphen (`-`) in the line:
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

### **`Example dataset 2: Arabidopsis100`**
```
Arabidopsis100/
├── Input_list.txt
└── Target_file_Arabidopsis100.fasta
```
#### `Input_list.txt`
This file documents taxon names and their corresponding sequence sources (marked in the second row,seperated by `tab` key):
```
Elaeagnus angustifolia	SRR26705271
Elaeagnus bambusetorum	SRR26757993
Elaeagnus henryi	SRR26705270
Elaeagnus macrophylla	SRR26753865
Elaeagnus mollis	SRR26758012
Elaeagnus oldhamii	SRR26705501
Elaeagnus pungens	SRR26705285
Hippophae neurocarpa	SRR26705287
Hippophae rhamnoides	SRR26756417
Hippophae salicifolia	SRR26705274
Hippophae tibetana	SRR26704952
Shepherdia argentea	SRR26756705
Barbeya_oleoides	SRR26756183	Outgroup
```
> [!NOTE] 
> - In this example dataset, all input data are public raw data from NCBI SRA (the identifiers are prefixed with SRR/ERR), which are ready to be downloaded in HybSuite pipeline. Thus, no data needs to be provided locally by users. 
> - The outgroup taxon is *`Barbeya_oleoides`*.

#### `Target_file_Arabidopsis_thaliana100.fasta`
This file is the target sequence file for Arabidopsis100.     
The gene name for a sequence should be placed immediately after the final hyphen (`-`) in the line:
```
>Locus-1
MAFRRVLTTVILFCYLLISSQSIEFKNSQKPHKIQGPIKTIVVVVMENRSFDHILGWLKSTRPEIDGLTGKESNPLNVSDPNSKKIFVSDDAVFVDMDPGHSFQAIREQIFGSNDTSGDPKMNGFAQQSESMEPGMAKNVMSGFKPEVLPVYTELANEFGVFDRWFASVPTSTQPNRFYVHSATSHGCSSNVKKDLVKGFPQKTIFDSLDENGLSFGIYYQNIPATFFFKSLRRLKHLVKFHSYALKFKLDAKLGKLPNYSVVEQRYFDIDLFPANDDHPSHDVAAGQRFVKEVYETLRSSPQWKEMALLITYDEHGGFYDHVPTPVKGVPNPDGIIGPDPFYFGFDRLGVRVPTFLISPWIEKGTVIHEPEGPTPHSQFEHSSIPATVKKLFNLKSHFLTKRDAWAGTFEKYFRIRDSPRQDCPEKLPEVKLSLRPWGAKEDSKLSEFQVELIQLASQLVGDHLLNSYPDIGKNMTVSEGNKYAEDAVQKFLEAGMAALEAGADENTIVTMRPSLTTRTSPSEGTNKYIGSY*
>Locus-2
MSDQQLETEINFWGETSEEDYFNLKGIIGSKSFFTSPRGLNLFTRSWLPSSSSPPRGLIFMVHGYGNDVSWTFQSTPIFLAQMGFACFALDIEGHGRSDGVRAYVPSVDLVVDDIISFFNSIKQNPKFQGLPRFLFGESMGGAICLLIQFADPLGFDGAVLVAPMCKISDKVRPKWPVDQFLIMISRFLPTWAIVPTEDLLEKSIKVEEKKPIAKRNPMRYNEKPRLGTVMELLRVTDYLGKKLKDVSIPFIIVHGSADAVTDPEVSRELYEHAKSKDKTLKIYDGMMHSMLFGEPDDNIEIVRKDIVSWLNDRCGGDKTKTQV*
>Locus-3
MSSRENPSGICKSIPKLISSFVDTFVDYSVSGIFLPQDPSSQNEILQTRFEKPERLVAIGDLHGDLEKSREAFKIAGLIDSSDRWTGGSTMVVQVGDVLDRGGEELKILYFLEKLKREAERAGGKILTMNGNHEIMNIEGDFRYVTKKGLEEFQIWADWYCLGNKMKTLCSGLDKPKDPYEGIPMSFPRMRADCFEGIRARIAALRPDGPIAKRFLTKNQTVAVVGDSVFVHGGLLAEHIEYGLERINEEVRGWINGFKGGRYAPAYCRGGNSVVWLRKFSEEMAHKCDCAALEHALSTIPGVKRMIMGHTIQDAGINGVCNDKAIRIDVGMSKGCADGLPEVLEIRRDSGVRIVTSNPLYKENLYSHVAPDSKTGLGLLVPVPKQVEVKA*
```

> [!NOTE] 
> - Differing from the target file for Angiosperms353, the target sequences in this file are all protein sequences.
> - Even so, there is no need to specify the type of target file, since HybSuite can automatically recognize the sequence type (nucleotide/protein).

---

## 3. Run the pipeline 
First of all, change your working directory to the downloaded example dataset file:
```
cd <the path to the directory of "example_dataset">
``` 
Next, create output directories (or specify an existing directory when running HybSuite):
```
mkdir -p ./Angiosperms353/Output ./Arabidopsis100/Output
```
After setting the right working directory, run the following commands for the two example datasets:

### Angiosperms353
```
hybsuite full_pipeline \
-input_list ./Angiosperms353/Input_list.txt \
-input_data ./Angiosperms353/Input_sequences \
-output_dir ./Angiosperms353/Output \
-nt 5 \
-process 5 \
-t ./Angiosperms353/Target_file_Angiosperms353.fasta \
-seqs_min_length 100 \
-seqs_min_sample_coverage 0.1 \
-PH 1234567 \
-sp_tree 14
```

### Arabidopsis100
```
hybsuite full_pipeline \
-input_list ./Arabidopsis100/Input_list.txt \
-output_dir ./Arabidopsis100/Output \
-nt 5 \
-process 5 \
-t ./Arabidopsis100/Target_file_Arabidopsis_thaliana100.fasta \
-seqs_min_length 100 \
-seqs_min_sample_coverage 0.1 \
-PH 1234567 \
-sp_tree 14
```

> [!TIP]
> 
> - The command format **`hybsuite full_pipeline [options] ...`** and **`hybsuite stage1 [options] ...`** applies to the Conda installation.
>     
> - If HybSuite is installed locally from source, run:
>     
>     **`bash <absolute_path_to_HybSuite/bin/HybSuite.sh> full_pipeline ...`**
>     
>     instead of **`hybsuite full_pipeline`**.
>