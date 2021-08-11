reactiondatabase
==============================

A database of reaction properties containing the atom-mapped reaction SMILES and the target properties. This repository belongs to the manuscript [Machine learning of reaction properties via learned representations of the condensed graph of reaction](https://chemrxiv.org/engage/chemrxiv/article-details/6112ac487117507542e68bef), a detailed description of the datasets and their respective preprocessing is available in the article.

### Reaction database

All reactions can be found in the `data` folder as CSV files.

Activation energies E2/S<sub>N</sub>2 reactions
-------------------------------------
Data extracted from [DOI:10.1088/2632-2153/aba822](https://doi.org/10.1088/2632-2153/aba822), CCSD level of theory. Atom-mapped manually.

Reaction enthalpies Rad-6-RE
----------------------------
Data extracted from [DOI:10.1038/s41467-020-19267-x](https://doi.org/10.1038/s41467-020-19267-x). Unbalanced reactions were discarded. Atom-mapped using [Grzybowski's algorithm](https://doi.org/10.1038/s41467-019-09440-2). Only forward reactions are given in the CSV file (the manuscript also includes reverse reactions).

Log10 of reaction constants
---------------------------
Data extracted from [DOI:10.1021/acs.jpca.7b07361](https://doi.org/10.1021/acs.jpca.7b07361). Atom-mapped using [Grzybowski's algorithm](https://doi.org/10.1038/s41467-019-09440-2).

Phosphatase assay
-----------------
Data extracted from [DOI:10.1073/pnas.1423570112](https://doi.org/10.1073/pnas.1423570112), atom-mapped manually. Only substrates with a single phosphate group were kept. One-hot encoding of the enzymes given in the same order as the Reaction SMILES.


### Data splits and models from manuscripts

To reproduce the exact results in the manuscript, the folder `data_splits` holds the exact data splits into training, validation and test set. Per database, five folds of splits are reported. The folder `models` reports the CGR GCNN (default hyperparameters) models for each dataset, one for each data split. Unpack the directories you plan to use. Note that for USPTO-1K-TPL, the individual folds are archieved instead of the full folder to limit the filesize. The `data_splits` and `models` folders also hold splits for three databases not reported in the `data` folder, namely:

Activation energies &omega;B97X-D3
-------------------------------------
Data extracted from [DOI:10.1038/s41597-020-0460-4](https://doi.org/10.1038/s41597-020-0460-4), at the &omega;B97X-D3/def2-TZVP level of theory (and B97-D3/def2-mSVP for pretraining).

Activation energies S<sub>N</sub>Ar
----------------------------
Data extracted from [DOI:10.1039/D0SC04896H](https://doi.org/10.1039/D0SC04896H).

USPTO-1K-TPL
---------------------------
Data extracted from [DOI:10.1038/s42256-020-00284-w](https://doi.org/10.1038/s42256-020-00284-w). 


### Predict with the provided models

To predict with one of the models, for example to utilize the first model for the first data split of the `lograte` database, install [Chemprop](https://github.com/chemprop/chemprop) and run:

`chemprop_predict --test_path data_splits/lograte/fold_0/aam_test.csv --checkpoint_path models/lograte/fold_0/model.pt --preds_path preds_lograte_0.csv`

to save the predictions to the file `preds_lograte_0.csv` (or choose any other file name). To use all five models for an ensemble prediction on your own set of reactions (provided as atom-mapped SMILES string, refer to the manuscript or the files in `data_splits` whether to include explicit hydrogens), use:

`chemprop_predict --test_path <path to your csv> --checkpoint_dir models/lograte/ --preds_path <path to save preds>`

The models `snar` and `phosphatase` furthermore require the input of molecular features, refer to the manuscript for an in-detail explanation. To predict with the first model on the first data split of the `snar` database, run:

`chemprop_predict --test_path data_splits/snar/fold_0/aam_test.csv --features_path data_splits/snar/fold_0/features_test.csv  --checkpoint_path models/snar/fold_0/model.pt --preds_path preds_snar_0.csv`


### Copyright

Copyright (c) 2021, Esther Heid. For individual licenses, please refer to the respective manuscripts.

