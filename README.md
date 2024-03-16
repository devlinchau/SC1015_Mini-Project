# Predicting Airbnb Prices in European Cities

## SC1015_Mini-Project_B137_Group9

## Problem Formulation

Since the end of the COVID pandemic, the tourism industry in Europe is booming again. A lot of tourists consider booking an AirBnB over a hotel because they tend to be more affordable. However, different listings offer different features at a wide range of prices. Through this project, we intend to predict the price of an AirBnB based on its features and help tourists determine whether their chosen listings are reasonably priced.

## Problem Statement

To what extent do the features of different Airbnbs affect their price? How can we determine whether an Airbnb is fairly priced?

Dataset: **Airbnb Prices in European Cities**

[https://www.kaggle.com/datasets/thedevastator/airbnb-prices-in-european-cities](https://www.kaggle.com/datasets/thedevastator/airbnb-prices-in-european-cities)

In order to investigate how different features of Airbnbs affect their price, we shall use the above dataset and build a model to predict the price based on the features of a listing. The dataset includes many cities from Europe, of which we chose Rome and London as they have the most number of entries. Additionally, we also chose the "weekday" datasets of both cities because as a tourist, we feel it is more realistic that the duration of your stay would exceed a weekend if you had opted to stay in an Airbnb in the first place.

**Variables' Description:**

| **realSum** | The total price of the Airbnb listing in Euros. _**(Numeric)**_ |
| --- | --- |
| **room\_type** | The type of room being offered (e.g. private, shared, etc.). _**(Categorical)**_ |
| **room\_shared** | Whether the room is shared or not. _**(Boolean)**_ |
| **room\_private** | Whether the room is private or not. _**(Boolean)**_ |
| **person\_capacity** | The maximum number of people that can stay in the room. _**(Numeric)**_ |
| **host\_is\_superhost** | Whether the host is a superhost or not. _**(Boolean)**_ |
| **multi** | Whether the listing is for business purposes or not. _**(Boolean)**_ |
| **biz** | Whether the listing is for business purposes or not. _**(Boolean)**_ |
| **cleanliness\_rating** | The cleanliness rating of the listing. _**(Numeric)**_ |
| **guest\_satisfaction\_overall** | The overall guest satisfaction rating of the listing. _**(Numeric)**_ |
| **bedrooms** | The number of bedrooms in the listing. _**(Numeric)**_ |
| **dist** | The distance from the city center. _**(Numeric)**_ |
| **metro\_dist** | The distance from the nearest metro station. _**(Numeric)**_ |
| **lng** | The longitude of the listing. _**(Numeric)**_ |
| **lat** | The latitude of the listing. _**(Numeric)**_ |
  
From the above list of variables, we have decided to filter out those that we feel are less relevant from the perspective of a tourist. As such, we are left with the following variables to explore:

- room\_type
- person\_capacity
- cleanliness\_rating
- guest\_satisfaction\_overall
- bedrooms
- dist
- metro\_dist

## Exploratory Data Analysis

We conducted exploratory data analysis to find out the distribution of the data, to what extent each feature is related to the response variable (realSum), as well any possible relationship between each predictor. From that, we identified outliers to be cleaned from the data. We also found that some of the predictors have a high correlation and are likely related and identified from the ones we shall not include in the model.

Hence as a result of our findings after conducting EDA, we filter out some predictors that have high correlations with each other and are left with the following predictors:

- room\_type
- person\_capacity
- guest\_satisfaction\_overall
- dist

## Data Cleaning/Preparation:

To clean the data, we removed outliers using the interquartile range. We then applied one-hot encoding.

In one-hot encoding, each unique category value in a categorical variable is represented as a binary vector. The length of the vector is equal to the number of categories in the variable. Each element in the vector is set to 0, except for the element corresponding to the category value, which is set to 1. However, during the process of making the model, we decided to try mean target encoding which will be explained more later.

We apply one hot encoding to our only categorical variable, room\_type, and the result is a vector of length 3 with the unique categories:

- Private room
- Entire home/apartment
- Shared room

Additionally, we also incorporate mean target encoding into our exploration. In mean target encoding, we replace each category value in a categorical variable with the mean of the target variable for that category. Specifically, we calculate the mean of the target variable (realSum) for each category value (room\_type), and replace the category value with the calculated mean.

## Machine Learning Models

**Multiple Linear Regression**

Multiple linear regression is a statistical technique used to model the relationship between a dependent variable and multiple (two or more) independent variables. In this case, our dependent (Y) variable is **realSum** and our remaining predictor variables are independent (X).

**Random Forest Regression**

Random Forest Regression forms a model using ensemble learning. It takes subsets of the data and creates a model to predict for that subset. It then makes an average of all the predictions to get the final prediction. We decided to try it as the relationship between the predictors and the realSum is likely non-linear and hoped it would improve our prediction accuracy.

**Neural Network Regression**

In this technique, a neural network is trained to learn the mapping between the input variables (predictors) and the output variable (**realSum**) in a continuous manner. The neural network consists of multiple layers of interconnected nodes, or neurons, which receive inputs, perform calculations, and generate outputs. Each neuron receives inputs from other neurons or directly from the input variables, applies a transformation to those inputs using an activation function, and passes the transformed output to the next layer of neurons. During training, the neural network adjusts its weights and biases through an optimization process to minimize the difference between its predicted output and the actual output.

**R^2 Values**

| **Model** | **R2 Train Rome** | **R2 Test Rome** | **R2 Train**  **London** | **R2 Train London** |
| --- | --- | --- | --- | --- |
| Multiple Linear Regression | 0.420 | 0.400 | 0.647 | 0.622 |
| Random Forest Regression | 0.462 | 0.432 | 0.671 | 0.634 |
| Increase Decision Trees | 0.464 | 0.434 | 0.670 | 0.635 |
| Increase Depth | 0.719 | 0.415 | 0.832 | 0.633 |
| Increasing Decision Trees & Depth | 0.718 | 0.421 | 0.832 | 0.636 |
| CV Random Forest with OHE | 0.529 | 0.443 | 0.693 | 0.644 |
| CV Random Forest with MTE | 0.523 | 0.453 | 0.723 | 0.625 |
| Neural Network OHE | 0.420 | 0.396 | 0.656 | 0.635 |
| Neural Network Mean Target Encoding | 0.404 | 0.394 | 0.656 | 0.635 |
| CV Neural Network OHE | 0.458 | 0.446 | 0.667 | 0.646 |
| CV Neural Network MTE | 0.427 | 0.422 | 0.662 | 0.640 |
  
## Interpretation of Results

We see that Random Forest yields the largest difference between train and test scores even though Cross Validation has been done to minimize overfitting. Furthermore, the Random Forest models have the highest scores overall. Hence, we can say that the Random Forest model has a higher accuracy but is more biased towards the data it is trained on. The Neural Network models are more consistent and more accurate than the basic Linear Regression model.

However, seeing how tourists will be able to access all available listings and will only consider the currently available ones, there is no need for the model to accurately predict listings that are not available to be useful for a tourist. Therefore, it is possible to recommend training a Random Forest model on all available listings to give an estimate on what is a fair price to pay for the given listing's features based on the predictions generated from the model.

That being said, if we are more interested in predicting the price more accurately and with more certainty when given new listings, further investigations involving spatial regression shall be required.

## Future Recommendations

We conducted a preliminary test to see whether there is enough evidence to believe a spatial approach is warranted. We tested for heteroskedasticity, which indicates whether the data has uniform variance or not. It was found that the data is indeed heteroskedastic and thus why the regression models are not able to create highly accurate predictions. Hence, we recommend building a model that can take spatial factors into account such as spatial lag regression.

### Contributions  
**Code** - Etienne, Matthew, Devlin  
**Documentation** - Etienne, Matthew, Devlin  
**Presentation** - Etienne, Matthew, Devlin  

### References  
Spatial Econometrics: https://sustainability-gis.readthedocs.io/en/latest/lessons/L4/spatial_econometrics.html  
Heteroskedasticity test: https://www.statology.org/breusch-pagan-test/
