# Physio/Psycho-Physiologic Interaction  
Bold deconvolution to create physio- or psycho-physiologic interactions.  

* **Select SPM.mat** (select files)  
Select SPM.mat file  

* **Type of analysis** (choose an option)  
Type of analysis  

    * **Simple deconvolution**   
    Simple deconvolution  

        * **Select VOI** (select files)  
        physiological variable  

    * **Psycho-Physiologic Interaction**   
    Psycho-Physiologic Interaction  

        * **Select VOI** (select files)  
        physiological variable  

        * ** Input variables and contrast weights** (enter text)  
        Matrix of input variables and contrast weights.This is an [n x 3] matrix. The first column indexes SPM.Sess.U(i). The second column indexes the name of the input or cause, see SPM.Sess.U(i).name{j}. The third column is the contrast weight.  Unless there are parametric effects the second column will generally be a 1.  

    * **Physio-Physiologic Interaction**   
    Physio-Physiologic Interaction  

        * **Select VOI** (select files)  
        physiological variables  

* **Name of PPI** (enter text)  
Name of the PPI mat file that will be saved in the same directory than the SPM.mat file. A 'PPI_' prefix will be added.  

* **Display results** (choose from the menu)  
Display results  
