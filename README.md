# molgrad

[![DOI](https://zenodo.org/badge/244630479.svg)](https://zenodo.org/badge/latestdoi/244630479)


Supporting code for: Jiménez-Luna _et al._'s "Coloring molecules with explainable artificial intelligence for preclinical relevance assessment", available as a preprint in [ChemRxiv](https://chemrxiv.org/articles/preprint/Coloring_Molecules_with_Explainable_Artificial_Intelligence_for_Preclinical_Relevance_Assessment/13252286)



## Installation

The recommended method of usage is via the Anaconda Python distribution. One can use the provided conda environment in the repository (should work for most *nix systems):

```bash
conda env create -f environment.yml
```

To use the graph neural-network models that were trained for the manuscript (plasma protein binding, Caco-2 passive permeability, hERG & CYP3A4 inhibition), you need to download them from:

``` bash
wget https://polybox.ethz.ch/index.php/s/dDDMzi3rTbqkWOV/download -O models.tar.gz
tar -xf models.tar.gz
```

Then activate the environment and prepend the folder to your PYTHONPATH environment variable:

```bash
conda activate molgrad
export PYTHONPATH=/path_to_repo_root/:$PYTHONPATH
```

### (Optional) Download datasets

All the training data used in this study can be freely downloaded from:

```bash
wget https://polybox.ethz.ch/index.php/s/K0orABbeJmwOUEh/download -O data.tar.gz
tar -xf data.tar.gz
```


## Usage

In order to generate explanations for a particular molecule, given a trained model, one only needs to call the `main.py` script. A CUDA-capable GPU is encouraged, but not required:

```bash
python molgrad/main.py -model_path model_weights.pt -smi SMILES -output_f RESULT_DIR
```


For instance, if we wanted to obtain feature colorings for nicotine for the hERG inhibition pre-trained endpoint, and store it under a home subfolder named `results`, one would do:

```bash
python molgrad/main.py -model_path molgrad/models/hERG_noHs.pt -smi "CN1CCCC1C2=CN=CC=C2" -output_f $HOME/results/
```

This will create a comma-separated file `global.csv` in that folder, with feature attributions corresponding to global variables (_i.e_. molecular weight, log _P_, TPSA, and number of hydrogen donors). Another subfolder `svg` will be created with the produced feature colorings.

Further parameters (such as feeding an entire .smi) for batch prediction and coloring can be checked via the provided help:

```
python molgrad/main.py --help
```

## (Optional) Train your own models:

The current framework also provides functionality for model training using custom data with the `train_ext.py` script. It assumes training data comes in a comma-separated (.csv) file, with one column carrying SMILES and another the target value, whose names need to be specified. For instance:


```bash
python molgrad/train_ext.py -data CSV_FILE -smiles_col "SMILES_COL" -target_col "TARGET_COL" -output path_to_weights.pt
```

The trained model can be then used to color molecules via the `main.py` routine as described above. Additional training options can be consulted with:

```bash
python molgrad/train_ext.py --help
```


## Data collection for XAI model validation

A comma-separated file with examples drawn from the literature to validate this and other XAI approaches can be downloaded from [here](https://polybox.ethz.ch/index.php/s/olEIsl2fPngzFYS).



## Citation

If you use this code (or parts thereof), please use the following BibTeX entry:

```
@article{jimenez2020color,
author = "Jose Jimenez-Luna and Miha Skalic and Nils Weskamp and Gisbert Schneider",
title = "{Coloring Molecules with Explainable Artificial Intelligence for Preclinical Relevance Assessment}",
year = "2020",
month = "11",
url = "https://chemrxiv.org/articles/preprint/Coloring_Molecules_with_Explainable_Artificial_Intelligence_for_Preclinical_Relevance_Assessment/13252286",
doi = "10.26434/chemrxiv.13252286.v1"
}

```
