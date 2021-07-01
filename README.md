# Problem Statement
As part of the public outcry to defund the police and dismantle and restructure law enforcement as a whole, there has been a call to provide funding for new and existing services to step in where applicable.

Based on the specific features of a crime, we endeavor to predict which type of intervention is needed for a particular crime to better allocate resources in addressing them.

---
## Contributions:
This project is a collaboration of Nelli Aydinyan, Jessica Bow, Gabrial Cano & Billy Horn
# Software Requirements
The following python packages will be required to run this notebook on your machine:
- Pandas
- Geopandas
- Folium
- Shapely (Point and Polygon)
- Matplotlib
- Seaborn
- Scikitlearn
- XGboost
- Eli5 (for feature weights) ([**instructions**](https://eli5.readthedocs.io/en/latest/overview.html))
- imbalance-learn (for bootstrapping) ([**instructions**](https://imbalanced-learn.readthedocs.io/en/stable/install.html))

---

# Notebook Table of Contents

1. [Data Cleaning and Geographic Visualizations](https://github.com/gabecano4308/San-Francisco-Crime-Predictor/blob/main/1_clean_geoviz.ipynb)
2. [Exploratory Data Analysis and Modeling](https://github.com/gabecano4308/San-Francisco-Crime-Predictor/blob/main/2_eda_model.ipynb)

---

# Methods of Analysis
This project will employ various classification models to optimize for the best predictions. Classifiers explored include Logistic Regression, Bagging, Random Forests (RF), AdaBoosting and XGBoosting. KNearest Neighbors (KNN) was explored initially, but after expensive computation and subpar results, we decided to not pursue this classifier further. Although various hyperparameters were tuned during our modeling process, we left them out of this notebook as the default classifier was superior in most cases.

Success will be based on the Accuracy metric, or how many predictions were correct out of total predictions made, as this is the most pertinent to the task at hand.

---

# Data

### Crime data
The main source of data used in this project consists of crime data from the city of San Francisco, CA spanning years 2016-2018. The data was gathered from [**this**](https://data.sfgov.org/Public-Safety/Police-Department-Incident-Reports-Historical-2003/tmnf-yvry/data) website, filtered by years. The data set contained over 300,000 reported crimes and included information such as crime category, description, location coordinates and the police district.

For cleaning we dropped some columns that were unnecessary for EDA and modeling, which included: PdId, IncidntNum, Incident Code, Address and Location. Additionally, we divided the Category column into three parent categories: Police (emergency), Police (non-urgent) and Specialty or Other. These parent categories were used as our target variable for predictions.

### Geo Data
The crime data has coordinate features: longitude and latitude. In order to classify these coordinates into  neighbourhoods, we used another data set called sf_neighbourhoods.csv. With the use of Point and Polygon from shapely we converted the data into geo objects and hence created a neighbourhood column for every crime. There were a few crime locations that didn’t fall within any neighbourhoods due to the coordinates being slightly skewed. Since there were only 0.2% of the data missing and it’s distribution on the map was random, so we dropped any points missing a neighbourhood.

---

# Findings and Conclusions
The top two models we considered moving forward with were the Random Forest and the XGBoost. Although the Random Forest classifier had higher accuracy, it was exceptionally overfit. For this reason, we decided to move forward with using the XGBoost classifier as our best model due to the balance of accuracy and bias/variance tradeoff.

A summary of the results from all explored models can be seen in the table below:

|**Classifier**|**Train Score**|**Test Score**|
|---|---|---|
|Logistic Regression|0.53|0.53|
|Bagging|0.88|0.68|
|**Random Forest**|**0.89**|**0.69**|
|AdaBoost|0.48|0.48|
|**XGBoost**|**0.55**|**0.54**|

After further digging into the outputs of the XGBoost model, we found the ‘Neighborhood’ category contributed significantly to making correct predictions. In particular, neighborhoods with a higher total count of crimes performed better, while neighborhoods with very low crime counts contributed little to nothing to the predictions. In order to better assess our predictions, we took a look at the coefficients from the Logistic Regression Model. The coefficients provide the opportunity to determine the probability that your prediction is correct with the ability to filter by neighborhood and by target category. With these two outputs, you can develop a type of confidence metric to get a better sense of which neighborhoods will be easier to predict than others.

We plan on taking the following steps in future iterations of this project to improve the power of predictability:

- More robust feature engineering
- Refining target categories
- More NLP on crime description column
- Investigate crime resolution vs neighborhood demographics

Using the insights gained from the data analysis in combination with the predictive modeling, we feel this would be a useful tool in allocating resources to appropriately deal with these categories of crimes.
