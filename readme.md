
# Cancer origin prediction
## Introduction
   Around 5% of metastatic malignancies are of unknown primary origin (CUP) and 80% of CUP patients have poor prognosis. Cancer origin determination combined with site-specific treatment of CUP has shown to improve patient outcomes. However, determining tissue origin of CUP is challenging in clinical settings. Existing pathology and gene expression-based techniques are time consuming, costly and often have limited performance. We aim to develop a high performance and easily implemented model for cancer origin prediction.
   We developed a deep neuron network (DNN)-based tissue of origin classifier using DNA methylation data of 7,342 patients from The Cancer Genome Atlas (TCGA) that cover 18 different cancer origins. 
This folder contains data, results and codes for DNN-based cancer origin prediction. Specifically, DNA methylation data from 7,342 patients were split into train/dev/test sets according to 60/20/20.

## DNN model
## Documentation
* A. data
This folder contains four subfolders for model training and evaluation
   1. train_dev_test
   csv formate files include train/dev/test data and corresponding metadata across 18 cancer origins. Columns includes 10,360 CPG cites and one primary_site for label. tfrecords formatted files include serialized data for DNN model training and evaluation.
   code.csv: a map file from cancer origin to numeric coded value.
   2. cv_data
   This folder contains data for 10 fold cross-validation evaluation of DNN model
   3. metastatic_data
   This folder contains data from 143 patients across 13 cancer origins for DNN model test. Columns includes 10,360 CPG cites and one primary_site for label
   4. GEO
   This folder contains data from 581 patients across 10 cancer origins from GEO for DNN model test. Columns includes 10,360 CPG cites and one primary_site for label


* B. File description in the ‘results’ subfolder
This fold contains model performance in different data and predicted cancer origins using DNN-based cancer origin prediction model.
   1. CV
   SSPN_stat.csv: An overall model performance using 10 fold cross-validation of training data, including specificity, sensitivity, positive predictive value and negative predictive value.
   2. dev
   Model performance using development data, including accuracy, average precision, confusion_matrix, specificity, sensitivity, positive predictive value and negative predictive value as well as predictive results.
   3. test
   Model performance using test data, including accuracy, average precision, confusion_matrix, specificity, sensitivity, positive predictive value and negative predictive value as well as predictive results.
   4. metastatic
   Model performance using data from metastatic patients, including accuracy, average precision, confusion_matrix, specificity, sensitivity, positive predictive value and negative predictive value as well as predictive results.
   5. test_ind
   Model performance using independent data from GEO, including accuracy, average precision, confusion_matrix, specificity, sensitivity, positive predictive value and negative predictive value as well as predictive results.

* C. File description in the ‘DNN_model’ subfolder
   best_model folder contains the best model obtained from optimization of hyper parameters.
   cv_model folder contains the models from 10 fold cross-validation.

* D. File description in the ‘codes’ subfolder
data_prep.py: Python codes including pipeline from raw data to tfrecords formatted data. Please don't run since raw data is big and not included in Data folder
cancer_orgin_DNN.py:  Python codes including functions for DNN model training and evaluation.

Note: Python code dependency: python 3.6.3, pandas 0.21.0, numpy 1.13.3, tensorflow 1.4.0, sklearn 0.19.1

usage: cancer_origin_DNN.py [-h] [--trainfile [TRAINFILE]]
                            [--testfile [TESTFILE]]
                            [--testmetafile [TESTMETAFILE]]
                            [--modelfile [MODELFILE]]
                            [--codesfile [CODESFILE]]
                            [--CVData_dir [CVDATA_DIR]]
                            [--model_dir [MODEL_DIR]]
                            [--units [UNITS [UNITS ...]]]
                            [--best_model_dir [BEST_MODEL_DIR]]
                            [--sample_size [{1468,143,581,448,431}]]
                            [-f FOLDS] [--results_dir [RESULTS_DIR]]
                            {train,test,cv,model_selection}

Get performance of cancer origin prediction model using test data

positional arguments:
  {train,test,cv,model_selection}
                        Choose the type of program to run

optional arguments:
  -h, --help            show this help message and exit
  --trainfile [TRAINFILE]
                        Methylation file as tfrecords to be used for training model
  --testfile [TESTFILE]
                        Methylation file to be tested as tfrecords format
  --testmetafile [TESTMETAFILE]
                        Meta data file to be used in testing
  --modelfile [MODELFILE]
                        Model file to be used in testing or to be saved in training.
  --codesfile [CODESFILE]
                        Map file from cancer origin name to numeric value as csv format.
  --CVData_dir [CVDATA_DIR]
                        Directory for storing methylation data for each fold
  --model_dir [MODEL_DIR]
                        Directory for storing models for each fold
  --units [UNITS [UNITS ...]]
                        a list of hidden units to test
  --best_model_dir [BEST_MODEL_DIR]
                        Directory for best model
  --sample_size [{1468,701,581,448,431}]
                        Test sample size
  -f FOLDS, --folds FOLDS
                        Number of folds to be generated
  --results_dir [RESULTS_DIR]
                        folder for test results

For example:
python3 ./codes/cancer_origin_DNN.py test \
                                         --testfile ./data/GEO/combined_final.tfrecords \
                                         --testmetafile ./data/GEO/combined_final_meta.csv \
                                         --units 64  \
                                         --modelfile ./DNN_model/DNN_model_100_dev_15_20/best_model/model_0.ckpt \
                                         --codesfile ./data/train_dev_test_15_20/code.csv \
                                         --sample_size 581 \
                                         --results_dir ./results/test_ind/
