# Predictive Analysis of COVID-19
*Group project regarding predictive data analysis*

The project regards COVID-19 data analysis and forecasting using different machine learning and deep learning methods. The data used for the analysis comes from the repository of the CSSE John Hopkins University (https://github.com/CSSEGISandData/COVID-19). 

For the purpose of the project, the scope of the analysis has been narrowed down to one continent, Europe. The initial dataset (with global information, before performing data cleaning and preparation) had 145142 observations (gathered from 22 Jan 2020 to 15 Feb 2022) in following columns:

![image](https://user-images.githubusercontent.com/96207926/194898548-abfa97ab-2379-4341-8be5-cbecf6ed9238.png)


After performing data cleaning and preparation, the dataset was left with 25246 rows (561 rows of data per country). In addition to original variables, new columns has been created and merged with the dataset:

![image](https://user-images.githubusercontent.com/96207926/194899552-3b844c5a-1a7a-40d7-bae7-752bd883d0af.png)

For the purpose of data preparation, two additional datasets have been used for easier column generation. First one came from (https://www.kaggle.com/datasets/folaraz/world-countries-and-continents-details?select=Countries+Longitude+and+Latitude.csv) and was used to add the "continent" column to the dataset. 
Second dataset, a lookup table that was included in the dataset provided by John Hopkins University, has been used to add a population column to the dataset.


### Data Cleaning, Preparation and EDA
The data cleaning and preparation, as well as exploratory data analysis have been performed in RStudio (*DataCleaning_Preparation.Rmd*, *ExploratoryDataAnalysis.Rmd*).
During the data investigation, it has been discovered that there is an issue with recovery observations after 04/08/2021 (they could have stopped being recorded). Such fact has forced us to decide how to proceed with the dataset.
It has been decided that it is better to have possibly more variables for later predictions with Machine Learning and Deep Learning methods, rather than having just more
observations, the dataset, after performing data cleaning and preparation, has been decreased to 25,246 rows (561 per country), covering the period between 22/01/2020 and 04/08/2021.


Moving to the exploratory data analysis, it has been checked whether the cleaned and prepared data is correct, along with some data investigation (checking the variable types, data dimensions, etc.) . Since ‘new.d’ is the variable of interest, the correlation has been evaluated between ‘new.d’ and other
variables. The results shown that the variables of ‘new.c’, ‘new.r’ and ‘population’ all have correlation score higher than 0.5 (0.67, 0.598 and 0.536 respectively), which could indicate possible relationship between the variables, thus worth including in prediction models.

For the purpose of making investigation of the data more detailed, we have agreed to split the ten most populated countries into two clusters of five: first five (Russia, UK, Germany, France, Italy) and bottom five of ten most populated (Spain, Ukraine, Poland, Romania, Netherlands).
My area of focus was to investigate second cluster, that is sixth to tenth most populated European country. With that in mind, I have visualised various variables from the dataset to see how they have changed over time.

![image](https://user-images.githubusercontent.com/96207926/194916186-de228a5f-f2cc-406e-9d34-478d0d4f7028.png)

![image](https://user-images.githubusercontent.com/96207926/194916256-3c478dc9-716e-4c23-ad6f-2aae290b7c16.png)

![image](https://user-images.githubusercontent.com/96207926/194916319-7b3e73dd-5ea7-452d-8bd2-b3d056a4b068.png)

![image](https://user-images.githubusercontent.com/96207926/194916365-5aa950e8-6177-4ca0-b4a1-a82f132ca743.png)

![image](https://user-images.githubusercontent.com/96207926/194916408-aa752b86-b831-4030-a600-2dbb1a20c744.png)

![image](https://user-images.githubusercontent.com/96207926/194916435-e82848ed-4ac0-47b2-a2e9-cc57ff6b00e8.png)


Furthermore, an unsupervised learning method was used in order to increase
the depth of the analysis. I have decided to use hierarchical clustering algorithm to group the
observations and try to gain insight into possible patterns hidden in our data. In order to perform
it, the functions from OpenClassrooms “Perform an Exploratory Data Analysis” course
(https://github.com/OpenClassrooms-Student-Center/Multivariate-Exploratory-Analysis) have been
used and slightly modified for my case. Some initial assumptions have been made before proceeding to
the clustering:
- Euclidean distance has been chosen as a distance measure between the data points
- In order to measure distance between two clusters of data points, Ward linkage criterion has
been used, meaning that the comparison between two clusters is done by combining them into
one cluster and calculating the variance of resultant cluster
- The dimensions have been reduced using PCA, where the number of dimensions to which we
want our data to be reduced has been set to 2 by default

Parallel coordinates plot has been visualised to see how individual data points sit across all our variables. Next, the mean of each variable in each cluster has been taken in order to plot the centroids. Since hierarchical classification requires that distances between each and every point are calculated, it has taken few
minutes to compute every distance.

Even though the algorithm suggested to use five clusters, I used four since I have decided that it would be sufficient for the analysis. On the boxplot, we can spot that cluster 0 is strong on daily cases ‘new.c’, deaths ‘new.d’ and recoveries ‘new.r’, and cluster 2 have the ratios grouped , that is: ‘infection_rate’, ‘death_rate’ and ‘recovery_rate’. 
It has been also spotted that ‘new.d’, ‘new.c’ and ‘new.r’ were grouped into one cluster, and parameters’ rates (death_rate, infection_rate, recovery_rate) were clustered together. 

### Predictions using Machine Learning and Deep Learning
The whole process of predictive data analysis have been described in *PredictiveDataAnalysis.ipynb* notebook file.
For proper performance evaluation of the models, three main metrics have been used: Root Mean Squared Error, Mean Absolute Error, and R-squared.

![image](https://user-images.githubusercontent.com/96207926/194923524-8952cf4c-d081-4cf1-ac85-560b1ded28a7.png)


Obviously, some of the models’ results could have been improved, but I suppose it would be necessary to take more time for testing various configurations, including hyperparameters tuning, different deep learning networks architectures or using different input parameters. Also, my LSTM network could have been tried in terms of multivariate time series analysis, thus inputting more factors to the model, like daily cases, daily recoveries, or even time itself. Moreover, exploratory data analysis with unsupervised learning method, in my case that is Hierarchical Clustering have not been investigated as thoroughly as I initially wanted. For example, it could be really beneficial for the analysis to try more different clustering configurations and various principal components numbers, although it was still possible to
reach some sensible conclusions with the current clustering results.
