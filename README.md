# Predictive Analysis of COVID-19
*Group project regarding predictive data analysis*

The project regards COVID-19 data analysis and forecasting using different machine learning and deep learning methods. The data used for the analysis comes from the repository of the CSSE John Hopkins University (https://github.com/CSSEGISandData/COVID-19). 

For the purpose of the project, the scope of the analysis has been narrowed down to one continent, Europe. The initial dataset (with global information, before performing data cleaning and preparation) had 145142 observations (gathered from 22 Jan 2020 to 15 Feb 2022) in following columns:

![image](https://user-images.githubusercontent.com/96207926/194898548-abfa97ab-2379-4341-8be5-cbecf6ed9238.png)


After performing data cleaning and preparation, the dataset was left with 25246 rows (561 rows of data per country). In addition to original variables, new columns has been created and merged with the dataset:

![image](https://user-images.githubusercontent.com/96207926/194899552-3b844c5a-1a7a-40d7-bae7-752bd883d0af.png)

For the purpose of data preparation, two additional datasets have been used for easier column generation. First one came from (https://www.kaggle.com/datasets/folaraz/world-countries-and-continents-details?select=Countries+Longitude+and+Latitude.csv) and was used to add the "continent" column to the dataset. 
Second dataset, a lookup table that was included in the dataset provided by John Hopkins University, has been used to add a population column to the dataset.

The data cleaning and preparation, as well as exploratory data analysis have been performed in RStudio (*DataCleaning_Preparation.Rmd*, *ExploratoryDataAnalysis.Rmd*).
During the data investigation, it has been discovered that there is an issue with recovery observations after 04/08/2021 (they could have stopped being recorded). Such fact has forced us to decide how to proceed with the dataset.
It has been decided that it is better to have possibly more variables for later predictions with Machine Learning and Deep Learning methods, rather than having just more
observations, the dataset, after performing data cleaning and preparation, has been decreased to 25,246 rows (561 per country), covering the period between 22/01/2020 and 04/08/2021.


Moving to the exploratory data analysis, it has been checked whether the cleaned and prepared data is correct, along with some data investigation (checking the variable types, data dimensions, etc.) . Since ‘new.d’ is the variable of interest, the correlation has been evaluated between ‘new.d’ and other
variables. The results shown that the variables of ‘new.c’, ‘new.r’ and ‘population’ all have correlation score higher than 0.5 (0.67, 0.598 and 0.536 respectively), which could indicate possible relationship between the variables, thus worth including in prediction models.
