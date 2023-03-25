# Paper: Deep Learning Approaches for Detecting Freezing of Gait in Parkinson’s Disease Patients through On-Body Acceleration Sensors

## Overview
 - FOG is a simpton for parkinson desease. It is basically a sudden inability to move.
 - Dataset consists of entries from a single sensor (single waist-worn triaxial accelerometer) data over time.
 - Total # of patients: 21
 - RNN model is empoyed to forecast the occurance of FOG


## Overall impression

Describe the overall impression of the paper.

## Key ideas

__Model/s__
Two sets of models are used: 1- Random forest and 2- DL models

_Random forest_ model was employed to find the best features to be used in the DL models.

_DL models_ are used to make the prediction
  1. model 1: denoiser autoencoder []()
  2. model 2: CNN []()
  3. model 3: NN + LSTM []()

Authors later compare the results with the outcome of the following models:
- Random forest
- AdaBoost
- SVM

Evaluate
- Leave-one-out cross-validation is used for evaluaton

Outcome
-  modeling spectral information of adjacent windows through an RNN shows good performance for detecting FOG
-  best outcome:  employ single triaxial accelerometer and without adding delay or increasing the size of the temporal analysis window

## Technical details

__Feature extraction__

__Model architecture__



Summary of technical details

## Notes

Questions and notes on how to improve/revise the current work
