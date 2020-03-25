Steps to reproduce the results in the manuscrips:
1. python Run_Clini_predict_PPR.py 20190125
2. python make_table_Clini_predict_PPR.py 20190125
Note: the '20190125' is the command line argument for 'dateTag'. This dateTag will be used to name the folders where the predictions will be saved to them.  

#---------------------------------------------
#---------------------------------------------

Main code: Clini_predict_PPR_percentile.py
This code performs 10-fold cross validation for prediction clinical outcomes, 
and returns the performance of each testing fold. 

Prerequisites: sklearn, xgboost, imbalanced-learn
Usage: python Clini_predict_PPR_percentile.py dataset Clinical NumOfFea Criteria Method dateTag
Command Line Arguments --
	dataset: The name of dataset. Please specify either TCGA or Chemo
	Clinical: The clinical outcome to be predicted. 
	          For TCGA, options are Race, ER, PR, HER2.
			  For Chemo, options are Response, ER, PR, HER2.
	NumOfFea: Number of genes to be selected when performing feature selection.
	Criteria: The criteria used when building models. Options are FSCORE, FSCORE_95, PPR90, PPR95.
			  FSCORE builds models using F1-score. Others use our new metric, which is in PPR_score.py 
	Method: Options are RN_only, no_RN_no_SMOTE, RN_SMOTE.
        dateTag: As mentioned above. 

Input Files -- 
	Two input files are needed for predictors.txt/predictors_rand.txt and response.txt in the TCGA/Chemo directories.
	
        In response.txt: Rows are samples, columns are clinical outcomes orderer by [Race, ER, PR, HER2] in TCGA and [Response, ER, PR, HER2] in Chemo.	

Output Files --
	It will create a directory with name {dataset}_OverSampling_{Criteria} if performing OverSampling, otherwise {dataset}_{Criteria}.
	In the directory, there will be 7 text files.
	{Clinical}_{NumOfFea}_F1score_fold.txt : F1-scores under 0.5 cutoff for each testing fold. Rows are folds, columns are scores from LASSO, Random forest, Gradient boosting, SVM.
	{Clinical}_{NumOfFea}_PPR_fold.txt: PPR scores for each testing fold. 
	{Clinical}_{NumOfFea}_PRECISION_fold.txt: Precision scores under PPR cutoff for each testing fold.
	{Clinical}_{NumOfFea}_F1PPR_fold.txt: F1-scores under PPR cutoff for each testing fold.
	{Clinical}_{NumOfFea}_AUROC_fold.txt: AUROC scores for each testing fold.
	
	{Clinical}_{NumOfFea}_valid_precision.txt: Precision scores under PPR cutoff for each validation fold.
	{Clinical}_{NumOfFea}_valid_PPR.txt: PPR scores for each validation fold.
	





