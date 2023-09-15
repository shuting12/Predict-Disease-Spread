# Predict-Disease-Spread

## Table of Contents
1. [Project Background](#Project_Background)
2. [Data Exploration and Processing](#Data_exploration_processing)
3. [Modeling](#Modeling)
4. [Results](#results)

## Project background 
This challenge is hosted by DriveData, we can find the full description here: https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread/
Dengue is a mosquito-borne disease that occurs in tropical and sub-tropical parts of the world. 
* Our goal is **predict the total dengue fever cases in San Juan, Puerto Rico & Iquitos**, Peru to prevent the diease spread.


## Data Exploration and analysis
<img width="707" alt="image" src="https://github.com/shuting12/Predict-Disease-Spread/assets/17936385/a6683196-1a3f-4692-8684-e33c72bf9abb">

Predict the total_cases label for each (city, year, weekofyear) in the test set for these 2 cities, test data for each city are 5 and 3 years respectively

#### Features 
-> Precipitation values
1. station_precip_mm – Total precipitation
2. precipitation_amt_mm – Total precipitation
3. reanalysis_sat_precip_amt_mm – Total precipitation
4. reanalysis_precip_amt_kg_per_m2 – Total precipitation

-> Temperature 
1. station_max_temp_c – Maximum temperature
2. station_min_temp_c – Minimum temperature
3. station_avg_temp_c – Average temperature
4. station_diur_temp_rng_c – Diurnal temperature range
5. reanalysis_air_temp_k – Mean air temperature
6. reanalysis_dew_point_temp_k – Mean dew point temperature
7. reanalysis_max_air_temp_k – Maximum air temperature
8. reanalysis_min_air_temp_k – Minimum air temperature
9. reanalysis_avg_temp_k – Average air temperature
10. reanalysis_tdtr_k – Diurnal temperature range

-> Humidity 
1. reanalysis_relative_humidity_percent – Mean relative humidity
2. reanalysis_specific_humidity_g_per_kg – Mean specific humidity

-> Satellite vegetation - Normalized difference vegetation index (NDVI)
1. ndvi_se – Pixel southeast of city centroid
2. ndvi_sw – Pixel southwest of city centroid
3. ndvi_ne – Pixel northeast of city centroid
4. ndvi_nw – Pixel northwest of city centroid

Total 20 climate related features, they are very similar, temperature/humidity/precipitation/vegetation variables 
Besides these 20, we also have "city", "year", "weekofyear", "week_start_date" in the data. 

### Data Exploration & Data Processing 
After initial data exploration, we can find: 
1. There are missing values
    -  forwardfill/backwardfill – ffill/bffill
    -  Interpolate()

2. Data in different scales, in later modeling, consider normalization or scaling 
     - Year/weekofyear are applied with Normalization
     - Other climate variables are applied with standardization
     - NVDIs are normalizated data, no need to work on these variables

3. Week number 53 is a mistake, there are only 52 weeks in a year, need fix the wrong values
     - There are 52 weeks in a year, let’s change the week 53 as week 1 instead.
5. Temperature variables are in different units ( K or C)
     - after the standardization, should be no problem
     - but need further check the correlations 
