# GCN-for-BBBP-Prediction
## Graph Convolutional Networks (GCN) for Predicting Brain Barrier Blood Permeability of Compounds

Some of my work was inspired by the following work:
https://www.kaggle.com/code/priyanagda/simple-gcn

---

## Dataset Used:
deepchem's BBBP (Blood-brain Barrier Penetration) dataset

_"The blood-brain barrier penetration (BBBP) dataset is designed for the modeling and prediction of barrier permeability. As a membrane separating circulating blood and brain extracellular fluid, the blood-brain barrier blocks most drugs, hormones and neurotransmitters. Thus penetration of the barrier forms a long-standing issue in development of drugs targeting central nervous system."_

- 1st column: number of row (unnecessary data)
- 2nd column: compound name
- 3rd column: p_np (boolean labels for permeability of blood brain barrier)
- 4th column: SMILES (simplified molecular-input line-entry system)

![image](https://user-images.githubusercontent.com/83327791/220548696-15ba89e1-a668-4e73-8482-366b65055897.png)

Reference:
https://deepchem.readthedocs.io/en/latest/api_reference/moleculenet.html

## Pre-processing of Dataset
- Featurizer: Transforming raw input data into processed data
  - MolGraphConvFeaturizer: a featurizer of general graph convolution networks for molecules. 
    - Reference: https://deepchem.readthedocs.io/en/latest/api_reference/featurizers.html
- Stratified Train/Vaild/Test Split: Splitting dataset into train, valid, and test sets
  - 80% for training
  - 10% for validation
  - 10% for testing

## GCN Model Architecture
![image](https://user-images.githubusercontent.com/83327791/220549483-c2a0a62e-77a5-471b-be75-a6828c0a8e82.png)

## Result
![image](https://user-images.githubusercontent.com/83327791/220549537-68022621-3c0d-466b-9854-bfcf82a81042.png)

## Testing the Trained Model
![image](https://user-images.githubusercontent.com/83327791/220549694-20509256-0df4-4584-ae21-3ec1f554c5d8.png)
![image](https://user-images.githubusercontent.com/83327791/220549741-d5b0dcb9-302c-4233-ae09-75d2f395059b.png)



