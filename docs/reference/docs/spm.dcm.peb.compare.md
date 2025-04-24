# Compare / Average PEB models  
Addresses the question: which combination of connections best explains the commonalities across subjects and the group differences between subjects?   

If a group difference is to be investigated, this should be the first manually entered between-subjects covariate in the PEB.   

This analysis is performed by comparing a PEB model to nested sub-models where certain parameters have been disabled (fixed at their prior mean of zero). Parameters are then averaged over reduced models to give an averaged PEB (referred to as a Bayesian Model Average, BMA). Each parameter in the PEB or BMA represents the effect of one between-subjects covariate on one connection.   

If only one first-level DCM is provided per subject, a search is made over reduced PEB models to prune away any parameters not contributing to the model evidence. If multiple DCMs are provided per subject, these are used to define the combinations of second level parameters to be compared.   

* **Select PEB file** (select files)  
Select PEB_*.mat file.   

* **DCMs** (select files)  
Select group DCM file (GCM_*.mat). This is a cell array with one row per subject and one column per DCM.   

* **Review PEB parameters** (choose from the menu)  
Review PEB parameters   
