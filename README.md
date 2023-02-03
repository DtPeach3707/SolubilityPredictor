# SolubilityPredictor
Machine Learning network that predicts solubility of a compound from its SMILES notation plus certain combinations of other parameters using the AqSolDB dataset.

# Jump To

[The Data](#the-data)

[Preprocessing](#preprocessing)

[The Model](#the-model)

[How to Run](#how-to-run-the-code)

[Citation](#citation)

# The Data

This model is trained on the AqSolDB dataset (cited below), a collection of solubilities for many compounds (represented by SMILES notation and InChI) with the following additional molecular descriptors, with the names of the variables in the code that store each descriptor in parentheses:
- Number of rotatable bonds (NRB)
- Number of valence electrons (NVE)
- Number of aromatic rings (NArR)
- Number of saturated rings (NSR)
- Number of aliphatic rings (NAlR)
- Total ring count (RC)
- Topological Polar Surface Area (TPSA)
- Labute's Approximate Surface Area (LASA)
- Balaban's J Index (BalabanJ)
- BertzCT Topological Complexity Index (BertzCT)
- Average Molecular Weight (MolWt)
- Standard deviation of solubility value (SD)
- Wildman-Crippen LogP Value (MLP)
- Wildman-Crippen MR Value (MMR)
- Heavy Atom Count (HC)
- Number of H Acceptors (NHA)
- Number of H Donors (NHD)

# Preprocessing

To preprocess the text, there first is a list of all the unique characters in the SMILES notation that is generated, and with that a dictionary that equates each character to a number. Then, all characters of the SMILES notation are turned into lists with their respective indexes (as deinfed by the dictionary) being 1.0 and all other values being 0.0. 

To preprocess the quantitative inputs, some inputs (like molecule mass) are scaled down by certain amounts so they are closer to 1. The extra inputs are preprocessed for the machine learning via code that takes an inital variable (<code>extra_inputs</code>) that lists all the extra inputs to be used to train the extra inputs model. 

# The Model

The model consists of three main parts. The first part is an LSTM network that makes a representation for each individual character via a TimeDistributed Dense Layer then uses an LSTM to process the sequence of character representations, with a normal Dense layer following. The second part is the processing of the extra inputs, using multiple Dense layers with the first layer having a neuron amount depending on how many extra inputs there are (if there are no extra inputs this part is ignored). The final part of the model combines the results from the first two parts (if there is no extra inputs just the first part), and produces the final solubility preciction. Because the solubilities in the dataset are negative and with an absolute value greater than 1, a linear activation function is used.

# How to Run the Code

To run the code, you first need to download the <a href="https://www.kaggle.com/datasets/sorkun/aqsoldb-a-curated-aqueous-solubility-dataset">AqSolDB Dataset</a>, as well as to open <code>AqSolDBSolubilityPredictor.ipynb</code> in Google Colab. Then, upload the AqSolDB csv into your runtime, then Runtime -> Run All. It is recommended to use a GPU Runtime so training takes shorter.

# Citation

Sorkun, M.C., Khetan, A. & Er, S. AqSolDB, a curated reference set of aqueous solubility and 2D descriptors for a diverse set of compounds. *Sci Data* **6**, 143 (2019). https://doi.org/10.1038/s41597-019-0151-1
