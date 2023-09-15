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
The project will be evaluated based on the mean absolute error from the test set, which will be marked by DrivenData. I have used Juypternote book and Google Colab to finish this project.  
<img width="640" alt="image" src="https://github.com/shuting12/Predict-Disease-Spread/assets/17936385/a42b3721-3992-4bac-b2d9-e894b80fdcf4">


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
#### Feature Engineering & Selection 
1. Data correlation
   - Total cases is not highly correlated with the variables, however some features are highly correlated to each other.
   - Even reanalysis_specific_humidity_g_per_kg was suggested with high correlation, I will keep this variable for future prediction. 

2. Create New Features
   *  _Average Temperature Values_: When there are same unit, new features can be created by statistical method, such as average of all columns
   * _Average NVDI index:_ Non of the NVDI seems very correlated to total cases, let’s create one value average all 4 NVDIs
   * _Month & Season variables_: Sicne the week start date is available, let’s create Month and season variables
   * _Cyclical time variables:_ Convert week and year into cosine and sine values, cyclical features
   * _Lags:_ From scientific research, we know dengue fever symptomsusually begin 4–10 days after infection and last for 2–7 days. Let’s add lagged climate variables in model.
   * _Average weekly cases_:For each city we have several year’s historical data, let’s create average weekly cases as an input variables as well.

3. Feature Selection:
- In different models, the feature selection approaches are different. In this project, domain knowledge is very important, so our feature selection part need to be carefully adding the relative features. 
![image](https://github.com/shuting12/Predict-Disease-Spread/assets/17936385/7bb8ddff-3855-46de-908f-279890b3ab5a)

Some things we should consider later:
1. Create new features step is really depend on which model will be used, some of the algorithm doesn’t need all these transformation.
2. Complex model with more variables will cause overfitting, in each model, pick important features for better performance in validation set to avoid overfitting. 

### Modeling 
I have tried 7 different approached to solve this problem. DrivenData also provided a benchmark study (https://drivendata.co/blog/dengue-benchmark/), the benckmark study has a MAE of 25.8173.  

My approach was split the data by city, and create 2 seperate models for San Juan and Iquitos predictions prespectively by different algorithms. 

<img width="708" alt="image" src="https://github.com/shuting12/Predict-Disease-Spread/assets/17936385/5d98a187-9511-47ad-8499-0a9e2816693e">

In conclusion: 
* **Tuned xgboost & prophet** are good models for the prediction in this exercises.

By Sep 15, 2023, I got the best model with MAE of 23+, there are still many things can be improved in the future. The first I will work on again is still on the feature selection part. Feature selection will definitely drive the model performance. 
<img width="586" alt="image" src="https://github.com/shuting12/Predict-Disease-Spread/assets/17936385/3efd3212-f957-4cfb-a416-9dfeb648e1e5">


### Files in this repo
1. Data
    Inputs and Outputs datasets
2.  Notebooks
    Python files for each model
3. Output
    submission csv files  
4. A pdf report regarding to my work (DengAI report updated.pdf)
