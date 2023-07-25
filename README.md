# Predictive Analysis of COVID-19

Group project regarding COVID-19 data analysis and forecasting using different machine learning and deep learning methods. The data used for the analysis comes from the repository of the [CSSE John Hopkins University](https://github.com/CSSEGISandData/COVID-19).

For the purpose of the project, the scope of the analysis has been narrowed down to one continent, Europe. The initial dataset (with global information, before performing data cleaning and preparation) had 145,142 observations (gathered from 22 Jan 2020 to 15 Feb 2022) in following columns:

| Name | Domain | Description |
|------|--------|-------------|
| X | integer | *Current row/observation* |
| country | string | *Name of the country* |
| iso3c | string | *3 letter country code* |
| date | date | *Date of the observation (YYYY-MM-DD)* |
| confirmed | integer | *The cumulative amount of confirmed positive cases recorded in a country on that date* |
| deaths | integer | *The cumulative amount of deaths recorded in a country on that date* |
| recovered | integer | *The cumulative amount of recovered recorded in a country on that date* |

After performing data cleaning and preparation, the dataset was left with 25246 rows (561 rows of data per country). In addition to original variables, new columns has been created and merged with the dataset:


| Name | Domain | Description |
|------|--------|-------------|
| new.d | integer | *New deaths on that day* |
| new.c | integer | *New positive cases on that day* |
| new.r | integer | *New recovered on that day* |
| death_rate | float | *The number of deaths per 100,000 people in each country per day* |
| recovery_rate | float | *The number of recovered per 100,000 people in each country per day* |
| infection_rate | float | *The number of infected people per 100,000 people in each country per day* |

For the purpose of data preparation, two additional datasets have been used for easier column generation. First one came from [Kaggle](https://www.kaggle.com/datasets/folaraz/world-countries-and-continents-details?select=Countries+Longitude+and+Latitude.csv) and was used to add the "continent" column to the dataset. 
Second dataset, a lookup table that was included in the dataset provided by John Hopkins University, has been used to add a population column to the dataset.




## Data Cleaning, Preparation and Exploratory Data Analysis
The data cleaning and preparation, as well as exploratory data analysis have been performed in RStudio (*data_preprocessing.Rmd*, *exploratory_data_analysis.Rmd*).
During the data investigation, it has been discovered that there is an issue with recovery observations after 04/08/2021 (they could have stopped being recorded). Such fact has forced us to decide how to proceed with the dataset.
It has been decided that it is better to have possibly more variables for later predictions with Machine Learning and Deep Learning methods, rather than having just more
observations, the dataset, after performing data cleaning and preparation, has been decreased to 25,246 rows (561 per country), covering the period between 22/01/2020 and 04/08/2021.


Moving to the exploratory data analysis, it has been checked whether the cleaned and prepared data is correct, along with some data investigation (checking the variable types, data dimensions, etc.) . Since `new.d` is the variable of interest, the correlation has been evaluated between `new.d` and other
variables. The results shown that the variables of `new.c`, `new.r` and `population` all have correlation score higher than 0.5 (0.67, 0.598 and 0.536 respectively), which could indicate possible relationship between the variables, thus worth including in prediction models.

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
it, the functions from [OpenClassrooms “Perform an Exploratory Data Analysis” course](https://github.com/OpenClassrooms-Student-Center/Multivariate-Exploratory-Analysis) have been
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

Even though the algorithm suggested to use five clusters, I used four since I have decided that it would be sufficient for the analysis. On the boxplot, we can spot that cluster 0 is strong on daily cases `new.c`, deaths `new.d` and recoveries `new.r`, and cluster 2 have the ratios grouped , that is: `infection_rate`, `death_rate` and `recovery_rate`. 
It has been also spotted that `new.d`, `new.c` and `new.r` were grouped into one cluster, and parameters’ rates (`death_rate`, `infection_rate`, `recovery_rate`) were clustered together. 




## Predictions using Machine Learning and Deep Learning
After data cleaning and preparation, and exploratory data analysis, it's time to perform predictive data analysis on the dataset.

In order to simplify the prediction process, the dataset have been appropriately converted for each predictive method; some selected variables have been summed up to make predictions for Europe as a whole, rather than analyzing each country separately. The presented approach also leaves some space for analyzing some particular countries separately if desired.

<img width="554" alt="Screenshot 2023-05-02 at 13 43 24" src="https://user-images.githubusercontent.com/96207926/235669690-6ca589cb-3c83-4cca-a209-e52c6f0ac5f5.png">


For evaluation purposes, following metrics have been used:
* Mean Absolute Error (MAE) - scale-dependent forecasting error that measures the absolute difference between actual and predicted values.
* Mean Absolute Scaled Error (MASE) - scaled version of mean absolute error that is used when comparing forecast accuracy across series with different units.
* Mean Absolute Percentage Error (MAPE) 
* Mean Squared Error (MSE) - measures the average of the square of the errors
* Root Mean Squared Error (RMSE)
* R-squared (R2) - represents the proportion of the variance for a dependent variable that's explained by an independent variable in a regression model

Since not every metric can be completely useful in analysed scenario, main metrics taken into consideration when evaluating performance are MAE, MSE, and R2. The metrics description can be found here: [Forecasting: Principles and Practice 3rd Ed. 5.8](https://otexts.com/fpp3/accuracy.html).

### Polynomial Regression
In order to perform prediction using machine learning method, I have used **polynomial regression**, which is a form of a linear regression and a special case of multiple linear regression. 

Polynomial regression is a reasonably simple method that tries to estimate the relationship between the independent and dependent variables as an nth degree polynomial. 

The con of this method is that it is highly sensitive to the outliers, however, it has been checked during data cleaning and preparation that no outliers are present in out dataset, hence this method was possible to use. The dataset have been split into training and testing set with an 80/20 split, and scaled 
by using `fit_transform()` function so that it could be eligible for fitting the polynomial regression model. 

While fitting the model I have tried different configurations, that is using different variables as predictors, and trying different polynomial degree values for fitting the data. 

Furthermore, since it has been noticed during performing Exploratory Data Analysis that the variable `new.d` has higher correlation values with `new.c` and `new.r`, they have been used as predictors in the model.

Next plots show how the polynomial regression models performed on test data:

<img width="1318" alt="Screenshot 2023-05-02 at 13 27 08" src="https://user-images.githubusercontent.com/96207926/235666022-a1bf4816-5b8e-4752-b861-82ca40c69b99.png">

<img width="1315" alt="Screenshot 2023-05-02 at 13 30 18" src="https://user-images.githubusercontent.com/96207926/235666588-f34c408c-6ee0-4887-9a4c-0eaba9f19bb3.png">




### LSTM Model
Long-Short Term Memory Network is a special case of recurrent neural network that is able to learn long-term dependencies, especially in sequence prediction problems, therefore it is well suited for forecasting tasks.

For simplification, univariate time series analysis has been performed, meaning that only daily deaths variable 'new.d' has been used to predict the future values.

In order to achieve better performance, it has been decided that the data should be normalised.
Normalization is a process of rescaling the data from the original range so that all values are within the range of 0 and 1. Since neural networks tend to train better and faster on rescaled data, and it is possible in this case to estimate the minimum and maximum values in the available data, the daily deaths variable `new.d` has been normalized. 

Also, the data has been initially pre-processed using 'tf.Data' API for better performance and decreasing training time.

Model summary:

<img width="336" alt="Screenshot 2023-05-02 at 14 05 52" src="https://user-images.githubusercontent.com/96207926/235676111-043bad83-52db-406d-beea-6a86f52b4c9f.png">

The results achieved by the LSTM on test data has been shown below: 

<img width="1302" alt="Screenshot 2023-05-02 at 13 40 38" src="https://user-images.githubusercontent.com/96207926/235669210-ee39bd3a-31b6-452b-a9ae-101401942ce5.png">


Table containing evaluation metrics has been shown below. It is worth to note here that the data normalisation is not recommended to use for polynomial regression, that is why the comparison between LSTM and PolyReg can seem misleading.

| Metric | PolyReg (deg = 2) | PolyReg (deg = 3) | LSTM (normalised data) |
|--------|-------------------|-------------------|------------------------|
| *MAE* | 591.15 | 694.84 | 0.036 |
| *MSE* | 400621.34 | 539553.1 | 0.0025 |
| *R2* | 0.5 | 0.33 | 0.83 |
| *RMSE* | 632.95 | 734.54 | 0.05 |
| *MAPE* | 47.86 | 58.76 | 18.79 |
| *MASE* | 2.37 | 2.79 | 1.046 |


### Conclusions
Generally some of the models’ results could have been improved, but it would probably be necessary to take more time for testing various configurations, including hyperparameters tuning, different deep learning networks architectures or using different input parameters. Also, LSTM network could have been configured for multivariate time series analysis, thus inputting more factors to the model, like daily cases, daily recoveries, or even time itself. Moreover, exploratory data analysis with unsupervised learning method (in my case that is Hierarchical Clustering) have been investigated briefly without diving into too much details. For example, it could be really beneficial for the analysis to try more different clustering configurations and various principal components numbers, although it was still possible to
reach some sensible conclusions with the current clustering results.
Definitely the biggest disadvantage we had to face during the project was relatively small amount of data to work with for forecasting purposes. Having only 561 days of data is definitely not sufficient to use deep neural networks, that is why the results achieved by using LSTM could be not satisfying enough. Even though the evaluation might suggest the network is performing reasonably well, there is still high risk that it might not generalise well to the unseen data. However, it serves as a good example that the traditional machine learning algorithms are still great tools to use when having insufficient amounts of data.
