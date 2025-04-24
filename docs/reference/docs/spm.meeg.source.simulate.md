# Simulation of sources  
Run simulation  

* **M/EEG datasets** (select files)  
Select the template M/EEG mat file.  

* **Inversion index** (enter text)  
Index of the cell in D.inv where the forward model can be found.  

* **Output file prefix** (enter text)  
Output file prefix  

* **What conditions to include?** (choose an option)  
What conditions to include?  

    * **All**   
      

    * **Conditions** (create a list of items)  
    Specify the labels of the conditions to be replaced by simulated data  

        * **Condition label** (enter text)  
          

* **Use inversion or define sources** (choose an option)  
Use existing current density estimate to generate data or generate from defined sources  

    * **Use current density estimate**   
    Use the current density estimate at the inversion index to produce MEG data  

    * **Set sources**   
    Define sources and locations (in MNI space)  

        * **Time window** (enter text)  
        Time window in which to simulate data (ms)  

        * **How to generate signals ?** (choose an option)  
        Choose whether to simulate a sinusoidal signal OR a random (gaussian white) filtered (and orthogonal) signals OR to load time series from a matfile (s(n,:) is nth dipole time series (all timeserie will be normalized to unit amplitude, and scaled only by dipole moment)  

            * **Frequencies of sinusoid (Hz) ** (enter text)  
            Enter frequencies of sources in Hz  

            * **Bandwidth of Gaussian orthogonal white signals in (Hz) ** (enter text)  
            Enter frequencies over which random signal exists in Hz  

            * **Variable s in a .mat file** (select files)  
            Select the .mat file containing the variable s  

        * **Dipole moment  ** (enter text)  
         EITHER (for perfect dipoles) enter dipole moment in nAm in x,y,z as nAm OR (for surfaces) enter total dipole moment (again nAm), followed by spatial extent (FWHM of a Gaussian) in mm. Note only relative value will be used if SNR is specified later  

        * **Source locations (mm)** (enter text)  
        Input mni source locations (mm) as n x 3 matrix  

* **Set SNR or set white noise level** (choose an option)  
Choose whether to a fixed SNR or specify system noise level  

    * **Sensor level SNR (dBs)** (enter text)  
    Enter sensor level SNR=20*log10(rms source/ rms noise)  

    * **White noise in rms femto Tesla ** (enter text)  
    White noise in the recording bandwidth (rms fT)  
