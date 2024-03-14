# Model Evaluation Summary for eos2ta5 (cardiotoxnet-herg)
# Ligand-based prediction of hERG blockade

## Model Summary

hERG is the human ether-à-go-go gene that is responsible for encoding a voltage dependent ion that regulates the electrical activities of the heart. Some molecules found in drugs have been observed to inhibit the activities of this gene resulting in scientists and pharmaceuticals working out ways to factor out these molecules during drug production. The cardiotoxnet-herg model is one of the computational models that have been designed to detect these harmful molecules before drug production.

## Model Description

- The model predicts hERG channel blockade based on an ensemble of five deep learning models.
- The model classifies small drug-like molecules as hERG blockers and hERG non blockers.
- The model generates probability score.
- Probability greater than or equal to 0.5 declares the molecule to be hERG blocker.

## Environment setup

Ubuntu 20.04 with Python 3.7

1. Install miniconda3
2. Install GitHub CLI
3. Install and activate Git LFS from Conda using:
        - conda install git-lfs -c conda-forge
        - git-lfs install
4. Install Ersilia using:
        - conda create -n ersilia python=3.7
        - conda activate ersilia
        - python -m pip install isaura==0.1
        - git clone https://github.com/ersilia-os/ersilia.git
        - cd ersilia
        - pip install -e .
5. Test the selected model to be sure it works.
        - ersilia -v fetch model_name
        - ersilia serve model_name
        - ersilia -v api run -i "CCCC"

## Steps

- Select dataset of 1000 molecules from ChEMBL database and extract the SMILES.
- Load this dataset into the model bias python notebook.
- Convert the SMILES to standard SMILES using the function in src folder.
- Use the RDKIT package to get the Inchikey representation of the molecules.
- Export the file containing Inchikey and SMILES and save in a CSV file.
- Select a model and run predictions via google colab on (https://github.com/ersilia-os/ersilia/blob/master/notebooks/ersilia-on-colab.ipynb).
- Create a scatter plot of the predictions (probability) gotten from the step above to show the model bias.

## Findings

Model bias evaluation:

- The initial prediction was carried out using 1000 SMILEs. After prediction, 752 SMILEs where categorized as hERG non-blocker while 248 were categorized as hERG blocker as their probability fall above 0.5.
- The distribution of the probability shows that a large number of the molecules are below 0.5. According to the model's publication, this means that a larger number of molecules in the dataset selected are hERG non blockers while the rest that are 0.5 and above are hERG blockers.
- The histogram also shows the frequency of the probability values which is seen to have a majority value between 0.1 and 0.2.


Model Reproducibility

- The data set used to reproduce the figures from the publication of the model can be found here (https://github.com/Abdulk084/CardioTox/tree/master/data). This repository contains 

	|  Dataset   | Total | hERG blockers | hERG non-blockers |
	|------------|-------|---------------|-------------------|
	|Train       | 12620 |     6643      |      5977         |
	|Test set I  |   44  |       30      |        14         |
	|Test set II |   41  |       11      |        30         |
	|Test set III|   740 |       30      |       710         |

- The original modle was used to carry out this task. This model implementation can be found https://github.com/Abdulk084/CardioTox/blob/master/README.md

- The implemetation was carried out on Ubuntu 20.04 and python 3.8. The initial implementation by the original authors was don on Ubuntu 20.04 and python 3.7.7

- These are the steps followed to setup the environment using Ubuntu 20.04 and python 3.8
	- download environment.yml file at https://github.com/Abdulk084/CardioTox/blob/master/environment.yml
	- conda env create -f environment.yml
	- conda activate cardiotox
	- git clone https://github.com/Abdulk084/CardioTox.git
	- cd Cardiotox 
	- cd PyBioMed
	- python setup.py install
	- cd ..

	Issues faced while setting up the environment
	-  TypeError for protobuf 
		Solution: Downgraded protobut by using "pip install protobuf==3.20.0"
	- Numpy version upgrade required
		Solution: Upgraded numpy by using "pip install numpy==1.19.5"

Reproducibility Summary

- From the publication, Tanimoto similarity was done on the training set and all three test sets to show that the data sets were all different and had no hint of similarity. During this reproducibility evaluation, I used the authors' data set to reproduce the similarity plots as can be found in the **/figures** folder.

- The publication also contains a scatter plot showing the space diversity which was definded using t-SNE components for all the data set which was also successfully reproduced. This shows diverse space distribution of the SMILEs as well as some overlap in the dataset. Even though there is a similarity between the plots from the original authors and the evaluation done here, some outliers were noticed which was not mentioned in the publication.

- Three tests sets were used by the authors to test the final model and some metrics like specificity, sensitivity, negative predictive value, positive predictive value, accuracy, Mathews correlation coefficient were used to evaluate the model's performance. This value was also reproduced in this work. The model performed well based on the sensitivity showing that it has a hogh chnage of predicting true positive values. It also performed well basen on the negative predictive value which tell that it has a high chance of predicting non-blokers correctly. 

- Test set I was run using eos2ta5 model from Ersilia Model Hub and predictions were extracted. These predictions were used to analyse the model performance using the same metrics mentioned above that were used by the authors and Ersilia model produced the same results.



## License

This package is licensed under a GPL-3.0 license. The model contained within this package is licensed under a None license.

Notice: Ersilia grants access to these models 'as is' provided by the original authors, please refer to the original code repository and/or publication if you use the model in your research.
_______

**Author:** Chiamaka Ohaji