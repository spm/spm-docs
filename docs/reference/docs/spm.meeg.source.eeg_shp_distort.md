# Distortion Trajectory  
To distort surface information in an M/EEG dataset based on typical normal variation

* **M/EEG datasets** (select files)  
Select the M/EEG mat file

* **Inversion index** (enter text)  
Index of the cell in D.inv where the results will be stored.

* **PCA Template location** (select files)  
Select the generic PCA template file (Template_0.nii). Will download this automatically first time used

* **Force recompute if exists** (choose from the menu)  
Will force recompute template for this subject

* **Range of indices** (enter text)  
The range (from, to) of PCA indices to distort

* **Number points** (enter text)  
The number of points on the distortion trajectory (linearly spaced between z limits)

* **Range of Z** (enter text)  
The normal range (in Z) over which to perterb true brain (eg [-3,+3])

* **Z max** (enter text)  
The max absolute Z value permitted (will flip displacement sign otherwise)

* **Sample surfaces centered about the subject or about the population mean?** (choose from the menu)  
If sub(ject), the generated surfaces will have a latent code in Z[subject] +/- Zrange\n
If can(onical), the generated surfaces will have a latent code in 0 +/- Zrange

* **Random seed** (enter text)  
The random seed that defines trajectory

* **Delete random seed directory** (choose from the menu)  
Will force delete of any existin random seed files
