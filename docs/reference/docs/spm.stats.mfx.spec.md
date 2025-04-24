# MFX Specification  
MFX Specification  

* **Select SPM.mat** (select files)  
Design and estimation structure after a 1st-level analysis.  

* **Contrast** (enter text)  
Contrast used to define 2nd level design matrix.  
E.g. ones(n,1) contrast where n is the number of sessions/subjects.  
The specification of a contrast that is not ones(n,1) allows, for example, specified sessions/subjects to be ignored.  
If left empty (default), a contrast ones(n,1) will be specified automatically at run time.  
