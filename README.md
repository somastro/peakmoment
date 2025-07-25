# peakmoment
This code enables the extraction of spectra from JWST image cubes, identification of emission lines, and generation of continuum-subtracted moment-0 maps for each detected spectral peak. It uses publicly available JWST archive (https://archive.stsci.edu/missions-and-data/jwst) data to demonstrate the workflow.

## USES

If you are using the notebook version, please open it in any notebook

(1) change the directory/filename. 

(2) Change the continuum window according to your data. 

(3) Define region (pixels or radius in arcsec) where you want to get  the spectra.

(4) you can change the sigma threshold. (test with the default value first). 


## Citation

If you use this code in your work, please cite the following:

> Dutta, Somnath;  *Molecular Jets from an Evolved Protostar: Insights from JWST-ALMA Synergy*; Submitted to ApJ, 2025.  
> DOI: [Zenodo DOI will be added here after deposition


---

## Installation

```bash
git clone https://github.com/somastro/peakmoment.git
cd peakmoment
pip install -r requirements.txt





