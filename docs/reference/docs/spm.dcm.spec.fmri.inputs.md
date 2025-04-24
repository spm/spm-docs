# Input specification  
Insert new inputs into a DCM model.
.
This functionality can be used, for example, to replace subject X's inputs by subject Y's. The model can then be re-estimated without having to go through model specification again.

* **Select DCM_*.mat** (select files)  
Select DCM_*.mat files.

* **Select SPM.mat** (select files)  
Select SPM.mat file.

* **Which session** (enter text)  
Enter the session (run) number.

* **Inputs** (create a list of items)  
Conditions to include and their parametric modulations (PMs). Click 'New: Condition' for each condition in your SPM (i.e. SPM.U), up to the last condition you wish  to include. For each Condition, enter:
1 (to include the condition in the DCM) or
[1 1] (the condition and its 1st parameteric regressor) or
[1 0 1] (the condition and its 2nd parameteric regressor)
etc

    * **Condition** (enter text)  
    Values to include for this condition. Enter '1' to include this condition (with no parameteric regressor). Entering [1 0 1] would include this condition and its second parametric regressor.
