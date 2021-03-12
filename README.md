# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree. This is the first of three projects required in fullfillment of the Udacity Nanodegree Program for Machine Learning Engineer with Microsoft Azure. In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
The data used in this project is related to direct marketing campaigns of a banking institution in Europe. The classification goal is to predict if the client will subscribe to a term. The dataset consists of 20 input variables and 32,950 rows with 3,692 positive and 29,258 negative classes. It has multiple columns which include age, marital status, dafault, loan and occupations as well as many more. It consists categorical and numerical variables. We utilized one hot encoding within the dataset for encoding categorical variables. When sampling the hyperparameter space we chose to utilize Random Sampling which supports discrete and continuous hyperparameters. It also supports the termination of low performance runs supporting our Bandit Policy of early stopping. In utilizing Random Sampling - hyperparameter values are randomly selected from the defined search space and utilized. Random Sampling is quite different from Bayesian Sampling where the Bayesian Optimization Algorithm picks samples based on the outcome of the previous sample - therefore continously trying to improve its outcome. Bayesian Sampling can be computationally and resource intensive. Bayesian Sampling also does not support an early termination policy which we were told we must utilize - hence why we did not pick Bayesian Sampling. It should only be utilized if you have the resources to see it through. We also picked the Bandit Policy for early termination. The Bandit Policy ends runs when the primary metric is not within the specified slack/factor amount of the most successful run. There is a very large imbalance between positive and negative classes. This may promote bias within the model. 

## Scikit-learn Pipeline
We use the Logistic Regression algorithm from Sci-KitLearn in conjunction with HyperDrive for hyperameter tuning. The pipeline consists of the following steps:

1. Data Collection
2. Data Cleansing
3. Test - Train - Split
4. Hyperparameter Sampling
5. Model Training
6. Model Testing
7. Activating Early Policy Stopping Evaluation
8. Saving the Model

We use a script **train.py**, to control steps 1-3, 5, 6 and 8. Steps 4 and 7 are controlled by HyperDrive. The execution of the pipeline is managed by HyperDrive. A brief description of each step is provided. 

## Data Collection

A Dataset is collected from the link provided using TabularDatasetFactory.

## Data Cleansing

This process involves dropping rows with empty values and one hot encoding the datset for the categorical columns.

## Data Splitting

Datasets are split into train and test sets which is standard practice. The splitting of a dataset is helpful to validate/tune the model. During this experience I split the data 80-20 which means 80% for training and 20% for testing. 

## Hyperparameter Sampling

Hyperparamters are adjustable parameters that let you control the model training process.

There are two hyperparamters for this experiment, --C and --max_iter. --C is the inverse regularization strength and --max_iter is the maximum iteration to converge for the Sci-KitLearn Logistic Regression model.

We used random parameter sampling to sample over discrete sets of value. Random parameter sampling is great for discovery learning as well as hyperparameter combinations though it requires more time to execute.

## Model Training

Once we have split our dataset into test and training data, selected hyperparameters then we can train our model. This is known as model fitting.

## Model Testing

The test dataset is split and used to test the trained model, metrics are generated and logged. These metrics are then used to benchmark the model. In this case, utilizing the accuracy as a model performance benchmark.

## Activating Early Policy Stopping Evaluation

The metric from model testing is evaluated using HyperDrive early stopping policy. The execution of the pipeline is stopped if conditions specified by the policy are met. 

We have used the BanditPolicy in our model. This policy is based on slack factor/slack amount compared to the best performing run. This helps to improve computational efficiency. 

## Saving the Model

The trained model is then saved, which is important if you want to deploy the model or use it in some other experiments.

## AutoML

AutoML uses the provided dataset to fit on a wide variety of algorithms. It supports classification, regression and time-series forecasting problem sets. The exit criteria is specified in order to stop the training which ensures the resources are not used once the objectives are met. This helps save on costs.

## Pipeline comparison
If you look at the final Jupyter Notebook you will see the model generated by AutoML had a slightly higher accuracy than the HyperDrive Model. The AutoML best model accuracy was MaxAbsScalerXGBoostClassifier at 0.9154 and the HyperDrive Model accuracy was 0.9109. The HyperDrive architecture was restricted to Logistic Regression from Sci-KitLearn. The AutoML has a wide variety of models it could evaluate - somewhere in the neighborhood of 20 models. HyperDrive is definitely at a disadvantage when it comes to going up against AutoML due to the fact that AutoML has upwards of 20 models to select from during an experiment.

## Future work

Future work may include improvements for HyperDrive. You could utilize Bayesian Parameter Sampling instead which works utilizing Markov Chain Monte Carlo methods for sampling a probability distribution. 

Improvements to AutoML may include channging the experiment timeout which would allow for more model experimentation. We could also address the class imbalance within the datset as well. This would help in reducing model bias. 
