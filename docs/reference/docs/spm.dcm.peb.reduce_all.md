# Search nested PEB models  
Optimises a PEB model by trying different combinations of switching off parameters (fixing them at their prior value), where doing so does not reduce the model evidence. Any parameters not contributing will be set to zero.  
  
This is equivalent to the function of spm_dcm_post_hoc but on the second level (PEB) parameters.  

* **Select PEB file** (select files)  
Select PEB_*.mat file.  

* **DCMs** (select files)  
Select group DCM file (GCM_*.mat). This is a cell array with one row per subject and one column per DCM.  

* **Null prior variance** (enter text)  
This is the prior variance for any 'switched off' parameter. Setting this to 0 (zero) tests the point null hypothesis that parameters are present vs absent. Setting this to a small positive number tests the null hypothesis that the parameter is trivially small. The smaller this setting, the fewer parameters will survive the model search. (See setting gamma in spm_dcm_bmr_all.m)  

* **Review PEB parameters** (choose from the menu)  
Review PEB parameters  
