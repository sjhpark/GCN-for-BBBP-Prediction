# GCN-for-BBBP-Prediction
## Graph Convolutional Networks (GCN) for Predicting Blood-Brain Barrier Permeability of Compounds

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

The dataset contains the names and SMILE strings of 2050 different compounds.

Sample from the dataset:

![image](https://user-images.githubusercontent.com/83327791/220550198-3446de98-d236-41c4-af8d-5b5814357f52.png)

The dataset's target values (permeability) are distributed as following:
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

## edge_index
I used edge_index as the feature to pass into the GCN model to train.

Here, edge_indx has the shape of [2, num_edges]

Each compound can be seen as an unweighted and undirected graph as the example shown below:
![image](https://user-images.githubusercontent.com/83327791/220552838-4ae9c8c0-c651-40a0-a626-1b4509c98490.png)

Therefore, the first row in the edge_index represents the edge between nodes in a single direction.
The second row represents the edge between nodes in the opposite direction.

Example:

![image](https://user-images.githubusercontent.com/83327791/220553136-e681e982-759f-4a9a-b6d4-65aa909dfe2e.png)
- 1st row: [0, 1, 1, 2] == [node0 -> node1, node1 -> node 2]
- 2nd row: [1, 0, 2, 1] == [node1 --> node0, node2 --> node1]

Reference: https://pytorch-geometric.readthedocs.io/en/latest/get_started/introduction.html

## GCN Model Architecture
![image](https://user-images.githubusercontent.com/83327791/220549483-c2a0a62e-77a5-471b-be75-a6828c0a8e82.png)

## Result
![image](https://user-images.githubusercontent.com/83327791/220549537-68022621-3c0d-466b-9854-bfcf82a81042.png)

## Testing the Trained Model
![image](https://user-images.githubusercontent.com/83327791/220549694-20509256-0df4-4584-ae21-3ec1f554c5d8.png)
![image](https://user-images.githubusercontent.com/83327791/220549741-d5b0dcb9-302c-4233-ae09-75d2f395059b.png)

## Discussion
__Why is the validation loss lower than the training loss for the first few epochs of training?__

In the beginning, I did not understand why the validation loss was lower than the training loss for the first few epochs of training. I initially had thought that the training loss should be lower than the validation loss. However, I did not find any data leakage issue to my validation dataset. Thus, I decided to further research the cause of this phenomenon.

Below are possible reasons for having a lower validation loss than the training loss.
1. __Dropout is not applied during validation while it is applied during training.__ I used the dropout technique after my convolution layer. Remember, dropout is a regularization technique that randomly drops out some of the neurons in the network. Thus, the network is less likely to overfit during training. However, during validation, the network is not trained. Thus, the network is more likely to overfit during validation. Therefore, the validation loss is higher than the training loss.
2. __Regularization is not applied during validation while it is applied during training.__ I used weight decay (L2 regularization) after each of my convolution layer. Remember, regularization is a technique that penalizes the loss function. Thus, the network is less likely to overfit during training. However, during validation, the network is not trained. Thus, the network is more likely to overfit during validation. Therefore, the validation loss is higher than the training loss.

### References:

[1] https://pyimagesearch.com/2019/10/14/why-is-my-validation-loss-lower-than-my-training-loss/

[2] https://towardsdatascience.com/what-your-validation-loss-is-lower-than-your-training-loss-this-is-why-5e92e0b1747e#:~:text=The%20regularization%20terms%20are%20only,loss%20than%20the%20training%20set.

