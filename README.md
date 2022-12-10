# Electricity Demand Forecasting
<p align="center">
  
![image](https://user-images.githubusercontent.com/102127193/206871375-092987c0-3e36-4f97-86fc-f155490ad1f3.png)

</p>

*Accurate power generation forecasts, both short term and long term, are critical to a green energy future. Accurate forecasts enable renewable generators to correctly price their energy and maximize contribution from renewable sources (while minimizing carbon sources), and are important to operators for system stability and planning. By being able to forecast well in advance how much electricity demand there will be in a given period of time, the system operator can determine how much of that demand can be met by solar, wind, and other renewable sources and how much and when extra fossil generation will be needed to fill any gaps between the base generation amounts and peak demands.*

## Data

In this project I used numerous models to forecast hourly demand approximately seven months in advance using four total years of hourly electricity demand data for all of Spain and weather data for five major cities in Spain. Models tested include simple linear regression, Random Forest, KNN, SARIMAX, FB Prophet, and XGBoost.

The datasets I used were obtained from [Kaggle](https://www.kaggle.com/datasets/nicholasjhana/energy-consumption-generation-prices-and-weather) (sourced from Open Weather, ENTSOE transparency platform, and Red Electrica). The energy dataset contained 35064 hourly records with electricity demand, predicted demand, price, predicted price, as well as generation totals by method (solar, fossil gas, fossil coal, wind, etc.) and the weather dataset contained hourly records with temperature, wind speed, humidity, rainfall, snowfall, and other weather descriptor variables for each of five geographically diverse cities in Spain.

**[Data Wrangling Noteboook](https://github.com/ehardwick2/Capstone2/blob/main/Data_Wrangling.ipynb)**

## EDA

One of the most enlightening visuals I created during EDA plotted a time series of one year of demand and also showed some of the major generation sources (solar, wind, fossil gas, and nuclear). This plot clearly illustrates the weekly seasonality with demand being higher during the week and lower on the weekend, as well as the winter/spring/summer/fall seasonality where demand is highest in winter and summer when more electricity is needed for heating and cooling, respectively.

![image](https://user-images.githubusercontent.com/102127193/206870642-3e261ea2-da6f-427d-b93f-2171ea5c47cf.png)

The EDA process also helped with some outlier detection and I also learned that humidity is an excellent predictor of solar generation. On a daily basis, humidity is high at night when there is no solar generation and low during the day when solar generation is occurring. In the absence of a solar radiation measurement, humidity is an excellent predictor, at least in Spain’s climate.

**[EDA Notebook](https://github.com/ehardwick2/Capstone2/blob/main/Exploratory_Data_Analysis.ipynb)**

## Feature Engineering

The base dataset with which I initiated modeling included time-indexed: total load (demand (MWH), temperature (K), pressure (mb), humidity (%), wind speed (km/hr), rain, snow, and cloud percents. I also engineered some useful time-based features for regression-based models. I used one-hot encoding to create binary categorical variables for the hour of the time (1-24), the day of the week, work/non-work day, and season (winter, summer, or spring/fall). I also created Fourier series terms for the multiple seasonal periods to use in the SARIMAX model to try to help it handle the multiple seasonalities, since it can only handle one seasonality. I used the FourierFeatures class of the sktime library to do this. 

## Modeling

**[Modeling Notebook](https://github.com/ehardwick2/Capstone2/blob/main/Modeling.ipynb)**

I first forecast seven months of energy demand using simple linear regression to provide a basline to compare other models to. I also tested out a simple year-over-year basline, just using the previous year's values to get an idea of the error metrics on this model. Next I tried Random Forest and KNN (both tuned with 5-fold RandomSearchCV). I also wanted to see how classical times series forecating using SARIMAX would do on a shorter forecast window, since this model can only handle a single seasonality. Next, I tried Prophet, a times series model developed by Facebook capable of handling multiple seasonalities and using exogenous regressors (like weather data). Finally, I also tested XGBoost, a gradient boosted trees-based model, which is good at finding patterns like seasonality. XGBoost cannot extrapolate but this dataset essentially has no trend (p=0 from ADF test).

<p align="center">
  
![image](https://user-images.githubusercontent.com/102127193/206873240-7757fc47-87e6-44e8-802d-cb21c781bc28.png)
  
</p>

The error metrics are sorted by lowest RMSE, which is the accuracy metric I chose to use for model comparison over mean absolute error (MAE) or mean absolute percent error (MAPE) because the errors are squared before they are averaged which gives the RMSE a higher weight to large errors, making RMSE more useful when large errors are undesirable. The lower the RMSE, the more accurate the prediction. Prophet has the lowest RMSE for a six day forcast window and for the seven month forecast window on the test sets.

<p align="center">

![image](https://user-images.githubusercontent.com/102127193/206873601-d36f5b31-23ba-4718-8bd2-8bbe695470c3.png)

</p>
  
## Conclusions
### Take Home:
Prophet and XGBoost are both good models for long term energy demand forecasting. They both capture the general pattern of the three seasonalities quite well but tend to underpredict peak demands, at least with this dataset. They would be good models to use for general long-term planning of the right energy generation mix, unless there is an overall trend to the data, in which case Prophet should be used. XGBoost wins out on training time though. For forecasts of several hours to a few days ahead, SARIMAX does a good job as long as there are no anomalous events that cause demand to spike or drop. 

### Further Investigation:
Due to time constraints, there were only so many model tweaks I could compare, but there are few worth noting that I have not yet investigated. Other avenues with this data that would be interesting to investigate would be to predict the contribution of wind-generated energy given the wind speed and energy price given wind generation, solar generation, and total demand. Energy forecasting is an interesting and important topic in machine learning and although I’ve not even scratched the surface with this project, I’m keen to dive deeper and learn more in the future! 
