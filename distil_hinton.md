# Distilling the Knowledge in a Neural Network

### Abstract

1. Compress the knowledge in an ensemble into a single model. 
2. Specialist models can be trained rapidly and in parallel.

### Introduction

- Knowledge acquired by large ensemble of modes can be transferred to a single model.
- When there are large number of classes the objective is to maximize the average log probability.

### Distillation

- Softmax 
- With T = 1, Using higher T produces softer probability distributions.
- Knowledge is transfered to the distilled model by training it on a transfer set and using soft target distribution for each case.
- Basically used high temp (soft targets) and temp = 1.

### MNIST

- 60K examples
- small net with 2 hidden layer 146 errors.
- big one 67 error.
- Increasing temprature got 74 test errors.

### Results

- Trained 10 models
- Same architecture
- Varying th dataset that each model sees does not significantly change our results
- 10 models were used.
- 

### Training ensembles of specialists on very big datasets

- Ensembles take advantage of parallel computation.
- Disadvantege : Ensemble requires too much computation at the test time.
- Learning Specilaist models that focus on confusable subset.
- Overfitting can be prevented by using soft targets.

### JFT dataset

- JFT is an internel google dataset. 
- 100 Million labelled images with 15,00 lables.
- Baseline model JFT was a deep CNN which had been trained for 6 months with large number of cores.
- 2 Types of Parallelism.
- Replicas of NN on  different sets of cores.
- Processing on different minibatches.
- Each replica computes avg gradient on its current mini-batch and sends back the new valses fo the parametes.
- These new values reflect all the gradientsrecieved by the parameter sever since the last time it send the parameter to the replica.
- Second : Each replica is spread over multiple cores by putting different subsets of neurons on each core.
- Ensembel trainig is yet third type of parallism that can be wrapped around the other two types.

### Specialist Models

- When number of classes is very large we should use an ensemble that contains one generalist model on all the data and many specialist models. 
- Each trained on data highly enriched in examples from confusable subset of classes.
- Each specialist model is initialized with the weights of the generalist model. 

### Assigning classes to specialists

- We focus on cateogirs that our full network often confuses.
- Use covariance matrix of generalist model and from specialist model.
- Obtain cluster using online version of k-means.
- Tried several clustering algorithms which produced similar results.

### Performing inference with ensembles of specialists

- Before distilling we see how ensembles perform.
- Generalist model can deal with classes that have no specialists so that we can decide which specialist to use.
- Step 1 : we find n most probable classes according to generalist model.
- Step 2 : We take specialist model m with confusable classes.
- Step 3 : p^m -> dist of specialist classes. p^g -> dist of general classes.
- Something about KL divergence that I dont understand.

### Results 

- The specialists train extremely fast and completely independently.
- With 61 specialist models there is a 4.4% imporvement.
- There are around 300 classes.
- Specialist with particular class have better accuracy imporvements.

### Soft Targets as Regualarizers

- Hard targets lead to overfitting.
- Soft targtes can recover almost all the information.
- Early stopping was not required as examples converged.
- A specialist is trained on data that is highly enriched in its special classes.

### Relationshipt to Mixture of Experts

- The gating network is learning to choose ehich expert to assign each example based on the relative discriminative performance of experts for that example.
- It is easier to parallize the trainig of multiple specialists.
- Once these subsets have been defined the specialists can be trained independently.

### Discussion

- Distilling works well for transferring knowledge from ensemble to distilled model.
- Ensemble of deep nets can be distilled into a single neural net of the same size.
- Performance of a single really big net for a long time can be improved by learning large number of specialist nets.
- Right noew we cannot distill the knowledge of specialists into the single large net.