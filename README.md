# Electricity Demand Forecasting

_Accurate power generation forecasts, both short term and long term, are critical to a green energy future. Accurate forecasts enable renewable generators to correctly price their energy and maximize contribution from renewable sources (while minimizing carbon sources), and are important to operators for system stability and planning. By being able to forecast well in advance how much electricity demand there will be in a given period of time, the system operator can determine how much of that demand can be met by solar, wind, and other renewable sources and how much and when extra fossil generation will be needed to fill any gaps between the base generation amounts and peak demands._

## Data

In this project I used numerous models to forecast hourly demand approximately seven months in advance using four total years of hourly electricity demand data for all of Spain and weather data for five major cities in Spain. Models tested include simple linear regression, Random Forest, KNN, SARIMAX, FB Prophet, and XGBoost.

The datasets I used were obtained from [Kaggle](https://www.kaggle.com/datasets/nicholasjhana/energy-consumption-generation-prices-and-weather) (sourced from Open Weather, ENTSOE transparency platform, and Red Electrica). The energy dataset contained 35064 hourly records with electricity demand, predicted demand, price, predicted price, as well as generation totals by method (solar, fossil gas, fossil coal, wind, etc.) and the weather dataset contained hourly records with temperature, wind speed, humidity, rainfall, snowfall, and other weather descriptor variables for each of five geographically diverse cities in Spain.

**[Data Wrangling Noteboook](https://github.com/ehardwick2/Capstone2/blob/main/Data_Wrangling.ipynb)**

## EDA

