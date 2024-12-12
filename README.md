# MLGEO2024_Lightning_Aerosol_Interactions
## Project Objectives
Lightning is an important phenomenon to understand, due to its impact on the nitrogen cycle, as a key producer of nitrogen oxides. Additionally, lightning is responsible for a majority of the wildfire area burned in the United States. Studies have determined that the product of convective available potential energy (CAPE) and precipitation [Romps et al., 2014] is a good parameterization for lightning, in addition to an increase in aerosol concentrations being linked with an increase in lightning [Thornton et al., 2014]. However, the relative contributions of each of these factors is not presently understood.

The goal of this project is to evaluate the relative importance of CAPE, precipitation and aerosol concentrations to lightning stroke density through the use of deep learning techniques. Greater understanding of what governs lightning has the potential to improve climate models by better capturing atmospheric chemistry, through better understanding of atmospheric nitrogen concentrations, which impact ozone and methane concentrations. Additionally, being able to predict lightning occurrence would improve wildfire preparation strategies, as wildfire prediction could increase. 

## Data Sources
This project uses convective available potential energy (CAPE) from the ECMWF ERA5 reanalysis, precipitation from NASA's IMERG satellite, aerosol data from NASA's MERRA-2 reanalysis, and lightning data from the World Wide Lightning Location Network (WWLLN), developed at the University of Washington.

## Data Access
### IMERG
The .txt file is located in the data/text_files directory. In order to run the command, the .txt file must be in your present directory, so move it to the directory you wish to extract the files in.

It is possible that the .txt file resets after a few days, in which case another must be generated. In order to do so, download a new list from GES DISC (the proper dataset is located at this link: https://disc.gsfc.nasa.gov/datasets/GPM_3IMERGHH_07/summary?keywords=imerg). In order to download data, an account on GES DISC is necessary.

To recreate the domain used in this project, click on "Subset/Get Data" under Data Access, followed by "Get File Subsets using the GES DISC Subsetter" in the "Download Method" section. Change the data range from 2023-01-01 to 2023-12-31 under Refine "Date Range", and enter -125,24.5,-66.5,50 under "Refine Region". Under "Select Variables", select precipitation. Afer this, click "Get Data" at the bottom. Click "Download Links List" from the following pop-up, and follow the Download Instructions.

A .txt file containing the list of links will be created. Place the .txt file in the desired directory before running the command (I am using wget for Linux/OS). Replace the .txt file at the end of the wget command with the proper link.

### MERRA2
The process for MERRA2 data is very similar to that for IMERG. Data can be found here: https://disc.gsfc.nasa.gov/datasets/M2T1NXAER_5.12.4/summary. Use the same parameters as for the precipitation data, but under "Select Variables", choose the desired variables. For this study, BCCMASS, BCSMASS, DUCMASS, DUCMASS25, DUSMASS, DUSMASS25, OCCMASS, OCSMASS, SO2CMASS, SO2SMASS, SO4CMASS, SO4SMASS, SSCMASS, SSCMASS25, SSSMASS, and SSSMASS25 are used. "CMASS" variables correspond to aerosol column mass densities, while "SMASS" variables correspond to aerosol surface mass concentrations.

### ERA5
CAPE data is taken from the ECMWF's ERA5 reanalysis, linked here: https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=download. In order to use the following code, an account must be made on Copernicus, and the CDSAPI client must be set up. Instructions on how to configure the CDSAPI are here: https://cds.climate.copernicus.eu/how-to-api

### WWLLN
Lightning data is taken from the World Wide Lightning Location Network (WWLLN), developed at the University of Washington. Data is not publicly available, but is available upon request. Instructions are located here: https://wwlln.net/. Since the data is not publicaly available, I will be subsetting lightning data I have already obtained.

## Descriptions of Notebooks in this Repository
Download_Data.ipynb: This notebook is used for and has instructions on downloading data from IMERG, MERRA2 and ERA5. WWLLN data is not publicly available as mentioned above. Due to the sizes of the files producesd, it is recommended to output the files to an external server or save in a Google Drive.

Data_Cleaning.ipynb: This notebook is used for cleaning the data, removing all outliers and NaN values.

Prepare_AI_Ready_Data.ipynb: This notebook is used for resampling the different datasets to the same resolution, as well as combining all the separate datasets into a singular xarray dataset, which can be used in the machine learning models.

EDA.ipynb: This notebook is used for the first steps of data analysis, in which the number of input variables is scaled down through the use of the Spearman correlation. Additionally, climatology maps of the different variables used in the study are made.

AutoML_Hyperparameter_Tuning.ipynb: This notebook is used to tune the hyperparameters of the various classical machine learning techniques.

Computational_Time_Analysis.ipynb: This notebook is used to compare the efficiency of the different classical machine learning approaches to this problem.

Model_Training_Assessment.ipynb: This notebook is used for preliminary evaluation of the different classical machine learning techniques.

ET_LGBM_Evaluation.ipynb: This notebook is used for further evaluation of the Extra Trees Regressor and the Light Gradient Boosting Machine, which were determined to be the best classical machine learning algorithms for this problem. Scatter plots and feature importance plots showing the success of the models are included.

CNN.ipynb: This notebook is used for training and running the U-Nets used in this study.

Deep_Learning_Output_Analysis.ipynb: This notebook is used for evaluation the output from both U-Nets trained and run in the notebook CNN.ipynb.

## Running the Notebooks in this Repository
The notebooks in this repository were run in a remote server, so file paths will need to be edited to reflect where the files are stored on your machine. Additionally, a conda environment was created where all of these notebooks were run. To replicate the environment, download the .yml file and run:

```
conda env create -f lightning.yml
conda activate lightning
```

## Contributions
The majority of the work on this project was done by Randall Jones, with coding assistance during the the classical machine learning section from Ula Jones.
