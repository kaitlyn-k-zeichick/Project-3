# Project 3: Predicting faulty water pumps in Tanzania

## Description
This project focused on predicting faulty water pumps in Tanzania. The project was divided into two problem statements:
1. Predicting faulty water pumps so that the Ministry of Water can reach out to communities that are likely to have faulty water pumps. This requires a highly accurate model, at the expense of sacrifing interpretability.
2. Identifying features that increase the odds of a faulty water pump so that the Ministry of Water can take preventative measures. For this model, interpretability is the priority.

## Data Used
The data was downloaded off of [DrivenData](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/23/). DrivenData had gotten the data from Taarifa and the Tanzania Ministry of Water.

## Features and Target
Target: Functional/Non-functional

Features: 
* Location
  * Longitude/Latitude
  * Basin
  * Region
  * GPS Height
* Water pump
  * Amount Total Static Head
  * Extraction Type
  * Waterpoint Type
  * Construction Year
* Water
  * Payment
  * Quality
  * Quantity
  * Source
* Management
  * Management Group
  * Public Meeting
  * Scheme Management
  * Permit
* Usage
  * Population

## Data Cleaning and Pre-Processing
I dropped features with high collinearity, and I dropped features that were irrelevant to the problem or that had too many variables. Then I dropped all missing values and removed outliers. I used label encoding for some features and get_dummies for others. Then I used feature engineering to create some new variables that I thought might be relevant, such as gps height and amount tsh.

Basic EDA was used to explore correlations between features. SQL was used to explore specific hypotheses, such as if the type of pump installed is correlated with the population of the surrounding area (which turned out to be true).

## Model 1: Maximizing Accuracy
First, I got the initial scores for various models (logistic regression, KNN, Random Forest, and XGBoost). Random Forest and XGBoost had the highest scores, so I used RandomizedCV to tune their hyperparameters. Random Forest received the highest score. Recall and accuracy were the prioritized metrics to compare models, since it was important in this model to catch as many faulty water pumps as possible (minimizing false negatives) while maintaining a high accuracy. **The final accuracy was 79% and the final recall was 80%.**

## Model 2: Maximizing Interpretability
I chose logistic regression for this model due to its interpretability. First I dropped features that aren't relevant to the problem, such as geographical information, since this model is for a scenario where the Ministry of Water has already chosen a community where a water pump is going to be installed and they want to what they can do to reduce the odds that the pump will break. Then I tried out various C values with an l1 hyperparameter to increase as much lasso regularization without sacrificing accuracy. This was because I wanted to increase interpretability by reducing the number of features used in the model. **The final accuracy was 70% and the final recall was 68%.** The coefficients were manipulated so that they could represent the odds (in percentage format) that the water pump is faulty given the feature. 

The three most impactful features turned out to be:
* When the quantity of the water is dry, the odds of the water pump being faulty increases by 159%.
* When the extraction type is 'other', the odds of the water pump being faulty increases by 46%.
* When the waterpoint type is a handpump, the odds of the water pump being faulty decreases by 38%.

## Tools Used
* Pandas and Numpy
* Matplotlib and Seaborn
* Sklearn
* SQL

## Possible Impacts
The first model can be used by the Ministry of Water to locate faulty water pumps, so that those communities can then be reached out to. The second model can be used to potentially prevent water pumps from breaking in the first place.
