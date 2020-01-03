# Mass-Specyrometry-models
Deep learning using tumor HLA peptide mass spectrometry datasets improves neoantigen identification
This project is to repeat the work from paper: [Deep learning using tumor HlA peptide mass spectrometry datasets improves neoantigen identification.](https://www.nature.com/articles/nbt.4313) 
## Packages
* Python 3.7
* Keras
* BeautifulSoup
* Theano
* MHCflurry
## Pipeline
![The pipeline of this project is showed as follow:](https://github.com/yutongLi1997/Mass-Specyrometry-models/blob/master/pipeline.png)
## Datasets
All the data used for MS model is available on the [online version of the paper.](https://www.nature.com/articles/nbt.4313#Sec33)
The data for generating binding affinity logos is available on Immune Epitope Database. ([IEDB])(https://www.iedb.org/database_export_v3.php)
The negative datasets are randomly generated from the MS dataset.
The CD8 T-cell epitopes data is available in Supplementary Dataset 3a,b.
**Training set**: This contains data from 101 samples, with a positive-negative ratio at 1: 2,000
**Test set**: There are two MS sets used for testing: (i) a tumor sample test set including five samples with a positive-negative ratio at 1: 2,500, and (ii) a single-allele cell line test set with a positive-negative ratio at 1: 1, 10,000.
**Validation set**: This is generated by taking 10% of the full dataset, and only used for early stopping.
## Features
**Peptide Sequences:** The peptide sequences data is available in MS dataset
**Flanking Sequences:** This can be done by taking the N-terminal and C-terminal flanking sequence of the corresponding peptide
**HLA alleles**: The HLA alleles data is available in Supplementary Dataset 1.
**Sample id**: The sample id data is available in Supplementart Dataset 1.
**TPM**: This can be done by taking the logarithm of per-peptide TPM.
**Family id**: The family id can be obtained by requesting [PantherDB](http://pantherdb.org/geneListAnalysis.do).
## Data Pre-Processing
1. Remove all the peptides whose lengths are not in the range of 8-11 mer
2. Vectorize the peptides and flanking sequences by a one-hot scheme
3. Compute per-peptide TPM as the sum of per-isoform TPM
## Full MS model
The full presentation model has the following functional form:
<img src="http://chart.googleapis.com/chart?cht=tx&chl= Pr(peptide\_i\_presented) = \sum_{k = 1}^{m}a_{k}^{i}*Pr(peptide\_i\_presented\_by\_allele\_k)" style="border:none;">
The per-allele probabilities of presentation are modeled as:
<img src="http://chart.googleapis.com/chart?cht=tx&chl= Pr(peptide\_i\_presented\_by\_allele\_k) = sigmoid\{NN_k (peptide_i) + NN_{flanking}(flanking_i)+ NN_{RNA}(log(TPM_i))+ \alpha_{samplie(i)} + \beta_{protein(i)}\})" style="border:none;">
Further description of the full MS model can be found in the paper.
## Model Evaluation
This project consists of two model evaluation procedures, namely model evaluation on held-out mass spectrometry data and model evaluation on retrospective neoantigen T-cell data. The evaluation metrics used here are precision and recall. Specifically, the recall is first set to an certain value to classify the test dataset. Precision is then computed at the certain recall.
## Model evaluation on held-out mass spectrometry
The performance of full MS model is compared with peptide MS model and MHCflurry model at different gene expression on test set. The evaluation between full MS model and peptide model is to verify the usefulness of RNA expression features (i.e. flanking sequences, RNA abundance, per-gene coefficients) while the evaluation between peptide MS model and MHCflurry is to verify the performance of peptide modeling (i.e. MS data vs binding affinity data).
The peptide motifs learned by MS model are compared with binding affinity motifs from IEDB dataset.
## Model evaluation on retrospective neoantigen T-call data
A dataset containing mutations of peptides is given to evaluate the performance of identifiying human tumor CD8+T-cell epitopes. Full MS model and MHCflurry model are evaluated by ranking the mutations with per-mutation probability and per-mutation binding affinity followed by counting the number of pre-existing T-cell responses in the top-ranked 5, 10 or 20 mutations for each patient.
## Identification of neoantigen-reactive T-cells in cancer patients
Peptides with a high score from full MS model but weak predicted binding affinity are observed in this section. This section is not included in this project.



