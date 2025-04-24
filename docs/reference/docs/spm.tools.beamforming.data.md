# Prepare data  
Prepare the input for beamforming   

* **Directory** (select files)  
Select a directory where the B.mat file containing the beamforming data will be written.   

* **M/EEG dataset** (select files)  
Select the M/EEG mat file.   

* **Inversion index** (enter text)  
Index of the cell in D.inv where the forward model is stored.   

* **Where to get MEG sensors** (choose from the menu)  
Taking sensors from D.sensors makes it possible to   
use the same head model and coregistration with multiple datasets.   
This relies on the assumption that the sensors are in head coordinates   
and the fiducals are at the same locations   

* **Coordinate system to work in** (choose from the menu)  
Select the coordinate system for the forward model   

* **Overwrite BF.mat if exists** (choose from the menu)  
Choose whether to overwrite the existing BF.mat file   
