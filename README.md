# PanCancer_Signature
## Introduction
[PanCancerSig](url) is an R package that contains several functions to facilitate the application cancer-specific prognostic gene signatures. 
Prognostic scores have been developed from a Cox regression-based statistical framework that utilized [TCGA](https://cancergenome.nih.gov/) datasets and can be applied to microarray and RNA-seq datasets. 
Each signature relies on the whole gene expression profile of each sample and is tissue-specific. 
Samples will be assigned a Prognostic Score (PS), which indicates the enrichment of protective and hazardous genes. 
Higher prognostic scores are associated with expression of protective genes and convey better prognosis, compared to samples with low PSs that are enriched for hazardous genes.
 PSs are relative to PSs within each dataset and are generally not to be compared between datasets. Prognostic signatures have been developed for:

•	**BLCA** - Bladder Urothelial Carcinoma<br/>
•	**GBM** - Glioblastoma multiforme<br/>
•	**HNSC** - Head and Neck squamous cell carcinoma<br/>
•	**KIRC** - Kidney renal clear cell carcinoma<br/>
•	**LAML** - Acute Myeloid Leukemia<br/>
•	**LIHC** - Liver hepatocellular carcinoma<br/>
•	**LUAD** - Lung adenocarcinoma<br/>
•	**LUSC** - Lung squamous cell carcinoma<br/>
•	**OV** - Ovarian serous cystadenocarcinoma<br/>
•	**PAAD** - Pancreatic adenocarcinoma<br/>
•	**SARC** - Sarcoma<br/>
•	**SKCM** - Skin Cutaneous Melanoma<br/>
•	**STES** - 	Stomach and Esophageal carcinoma<br/>

Signatures for HNSC, LUSC, and SARC need to be used with caution, as these have not been confirmed as prognostic signatures in the initial publication. 


## Installation
1. Download the PanCancerSig_1.0.tar.gz from GitHub or [here](https://morgan.dartmouth.edu/~f002nfh/).<br/>
2. Open R and install the package using ```install.packages("package_path/PanCancerSig_1.0.tar.gz", repos = NULL)```.<br/>
3. ```library("PanCancerSig")```.<br/>

## Example data
To exemplify the use of our package, we have included gene expression profiles and clinical information for 150 Ovarian Cancer (OV) samples. These example data can be accessed using the following commands:<br/>
```{r}
data(ExprData)
data(InfoData)
```
Importantly, users should prepare their own clinical information in a similar format as these example data: sample_ID (string), time_to_death (numeric), event (binary), stage (numeric), grade (numeric), age (numeric).<br/>
## PanCancerSig Usage
(1) **Prognostic Score (PS) calculation.** We have provided 13 cancer-specific signatures, including BLCA, GBM, HNSC, KIRC, LAML, LIHC, LUAD, LUSC, OV, PAAD, SARC, SKCM, and STES. These abbreviations need to be used to specify “canType” to pull the corresponding weight matrix and calculate appropriate PSs. If you set “canType” to “All”, it will utilize all pre-defined weight matrixes to calculate PSs in the context of all 13 cancer types. Our signature is platform independent and can be utilized for one and two channel microarrays as well as RNA-seq data. Data type can be specified in "platform", which accepts "array" and "RNAseq". The default is set to "array". The output of this function will be saved in a text file.
```{r}
calcPS(geneExp = ExprData, outFile = "path_to_folder/filename.txt", canType = "OV", platform = "array")
```
(2) **Survival analysis.** Using the output of calcPS(), a Cox Proportional-Hazards Model is conducted to calculate the p-value, HR and concordance score using both binary and continuous PSs. This output indicates the performance of PSs to predict prognosis in the dataset of interest. HRs < 1 indicate a protective role for high PSs, whereas HRs > 1 indicate high PSs to be hazardous. The concordance score is similar to an ROC score and indicates the probability of a higher PS in a randomly selected patient who had an event compared to the PS of a patient who has not experienced the event. A concordance of 0.5 indicates a random assignment of PSs. 
The cutoff of PS for binary calculations can be altered by changing "psCut". Accepted inputs are "median" (default), "mean" and any numeric value. If a file output of this function is desired, “SaveOut” needs to be set to TRUE and a specification of “OutFile” (Path_to_output/filename.txt) is required.
```{r}
data(InfoData)
myres <- survAnalysis(psFile = "path_to_folder/filename.txt", clinicalInfo = InfoData, psCut = "median", saveOut = F, outFile = "")
```
(3) **Survival plot.** A survival curve can be plotted using the output of calcPS. 
Again, “psCut” can be set to "median" (default), "mean", and any numeric value to specify the separation of samples into two groups for the survival plot. 
The p-value, HR and concordance score of coxph model using binary or continuous PS can be added to the plot by setting “concordanceType”. 
If set to "B", binary results will be added and if set to "C", continuous PS scores will be used for calculations. 
Plots will be saved in a pdf file specified by “saveOut”. If outFile is "", it will be saved in your current workplace.
```{r}
survPlot(psFile = "path_to_folder/filename.txt", clinicalInfo = InfoData, psCut = "median", concordanceType = "B", outFile = "output/survivalPlot.pdf")
```

## Contact
Please contact Yue Wang at yue.wang@dartmouth.edu or Chao Cheng at Chao.Cheng@dartmouth.edu if you have any questions.
