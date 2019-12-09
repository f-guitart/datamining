# Pipeline Building

We have seen a lot of things during this course:
* How to process Data using a Python interpreter
* How to load datasets into Pandas Structures
* How to analyze these datasets to get more insights and know more about our data
* How to transform the datasets
* How to approach data cleaning providing Technical Correctness and Consistency
* What are Pipelines and how to build them using scikit-learn

It's time to put it all together and see what we can get from a dataset.


## Materials
To follow this lab, we will use:
* Spyder IDE
* Data from the [Kaggle Competition - House Prices: Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data)

Of course we will use all the modules and packages we have seen during the course.

## Objective

We will try to estimate the value of an unknown variable using a set of independent variables.

### What is Kaggle?
*(from https://en.wikipedia.org/wiki/Kaggle)*

Kaggle is an online community of data scientists and machine learners, owned by Google.

Kaggle allows users to find and publish data sets, explore and build models in a web-based data-science environment, work with other data scientists and machine learning engineers, and enter competitions to solve data science challenges.

## What we will learn in this Laboratory
 Basically one single big thing:
 * How useful is all the knowledge we have acquired during the course

## Outline
* Exploratory Data Analysis
* Dataset Creation
* Pipeline Building
* Evaluation

## Exploratory Data Analysis

##### Set up your project Environment
There's no gold standard about how to organize your project.

However, try to be as organized as possible.

* **Regarding the data:** in this laboratory we will have a dataset (possibly split into few files).

  Makes sense to have a directory devoted to place your **data**.

* **Regarding the code** we will probably end with few scripts for handling data and generating results.

  Makes sense to have a directory devoted to place your **s**ou**rc**e code.

* **Regarding the results** we probably will generate different kind of **results**:
  * New **data**sets
  * Plot **im**a**g**es
  * **Models**

Taking this into account a reasonable project structure would be the following one:
```
project
│   README.md│
└───data
│      train.csv
│      test.csv
│      data_description.txt
│      sample_submission.csv
└───src
│      01_script1.py
│      02_script2.py
└───results
    └───data
    └───img
    └───models
```


Next, download the data from the Kaggle Competition: [Kaggle Competition - House Prices: Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data)

You have to sign up in Kaggle. If you're not interested at all, I found a repository with the data.
* [train.csv](https://github.com/data-doctors/kaggle-house-prices-advanced-regression-techniques/blob/master/data/train.csv)
* [test.csv](https://github.com/data-doctors/kaggle-house-prices-advanced-regression-techniques/blob/master/data/test.csv)
* [sample_submission.csv](https://github.com/data-doctors/kaggle-house-prices-advanced-regression-techniques/blob/master/data/sample_submission.csv)
* [data_description.txt](https://github.com/data-doctors/kaggle-house-prices-advanced-regression-techniques/blob/master/data/data_description.txt)

Open Spyder.

You can install Spyder via:
  * [Anaconda Navigator](https://docs.anaconda.com/anaconda/install/): desktop graphical user interface (GUI) included in Anaconda. Allows you to launch applications and easily manage conda packages, environments, and channels without using command-line commands.
  * `pip install spyder`

Import "The Three Data Science Tenors":
  ```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
  ```

Now we can load the dataset.
```python
# load data
train_df = pd.read_csv("../data/train.csv")
train_df["dataset"] = "train"
test_df = pd.read_csv("../data/test.csv")
test_df["dataset"] = "test"
```

As we want to explore the data we will concatenate both datasets
```python
# merge data
df = pd.concat([train_df, test_df]).reset_index()
```

Let's first see variables and its types.
```python
df.dtypes
```

We have 82 variables. Let's try to divide them into numeric and descriptive.

```python
# descriptive
df.dtypes[df.dtypes == 'object']
```

```python
# numeric
df.dtypes[df.dtypes != 'object']
```

Let's check `NaNs`.
```python
df.isnull().sum()
```

We can't see much, so let's filter them, getting only those that have `NaNs` and showing the percentage.
```python
number_of_nans = df.isnull().sum()
pct_number_of_nans = number_of_nans/df.shape[1]
pct_exist_nan = pct_number_of_nans[df.isnull().sum()>0]
pct_exist_nan.sort_values(ascending=False)   
```

We have a lot of variables, so let's select these that might be more important for our objectives.

The most important variable, is our **objective** (or **target**) variable: `SalePrice`.

```python
df["SalePrice"].describe()
```

```python
df["SalePrice"].isnull().sum()/df.shape[0]
```

We see that there are many `NaNs`, but let's check where they are.

```python
df.groupby('dataset').agg({'SalePrice': lambda x: x.isnull().sum()})
```

Let's plot the distribution of the variable.
```python
df["SalePrice"].hist(bins=50)
```
We can see that it is a bit skewed towards a mean value around 130K.

```python
plt.scatter(x=range(df.shape[0]), y=df["SalePrice"].sort_values())
```
An it looks like a   S, with two asymptotes near min and max. This is something that we have tot take into constituting when selecting the "learning" method.

To find other important variables, we can do a correlation test:

```python
abs(df.corr()["SalePrice"].sort_values(ascending=False)).head(15)
```

This tests show how much `SalePrice` changes, when the other variables change.
```python
df.pivot(columns="OverallQual").SalePrice.plot.box()
```

But we can only do this for numeric variables.

For categorical, we can plot `box` plots and see if we can find any insight in there.

https://deparkes.co.uk/2016/11/04/sort-pandas-boxplot/
```python
def boxplot_sorted(df, by, column, rot=0):
    # use dict comprehension to create new dataframe from the iterable groupby object
    # each group name becomes a column in the new dataframe
    df2 = pd.DataFrame({col:vals[column] for col, vals in df.groupby(by)})
    # find and sort the median values in this new dataframe
    meds = df2.median().sort_values()
    # use the columns in the dataframe, ordered sorted by median value
    # return axes so changes can be made outside the function
    return df2[meds.index].boxplot(rot=rot, return_type="axes")

for descr_var in df.dtypes[df.dtypes == "object"].index:
    fig, ax = plt.subplots()
    ax_ = boxplot_sorted(df, by=descr_var, column="SalePrice", rot=45)
    ax.set_title("Boxplot of SalePrice by {}".format(descr_var))
    ax.plot()

```

After this analysis we have found that:
* Objective variable does not look like a line
* We have seen how to measure "importance" of numerical variables
* We have found a set of interesting categorical to include in our model

## Dataset Creation

Let's take a look at how informed are the variables we have chosen:

##### Categorical
```python
train_df.loc[:, categoric].isnull().sum()/ train_df.shape[0]
```
It looks like `PoolQC` is not very well informed. We should discard this one.

##### Numerical
```python
train_df.loc[:, numeric].isnull().sum()/ train_df.shape[0]
```

Numerical variables are perfectly informed.

We will finally select the following variables:

##### Objective:
* SalePrice

##### Numerical:
* OverallQual
* GrLivArea
* GarageCars
* GarageArea
* TotalBsmtSF
* 1stFlrSF
* FullBath
* TotRmsAbvGrd
* YearBuilt
* YearRemodAdd

##### Categorical:
* SaleType
* RoofMatl
* PoolQC
* Neighborhood
* KitchenQual
* ExterQual
* BsmtQual

```python
target = ["SalePrice"]

numeric = [
"OverallQual",
"GrLivArea",
"GarageCars",
"GarageArea",
"TotalBsmtSF",
"1stFlrSF",
"FullBath",
"TotRmsAbvGrd",
"YearBuilt",
"YearRemodAdd"
]

categoric = [
"SaleType",
"RoofMatl",
#"PoolQC",
"Neighborhood",
"KitchenQual",
"ExterQual",
"BsmtQual"
]
```


Regarding the rows, we have seen that initially our dataset was split into two sets: **test** and **train**.

This is because, we might want to evaluate our model using **cross-validation**. 

In this kind of validation approaches, we split our data into two sets: train and test.

* **train** is used for fitting the model, this is why in the test set we have the objective variables
* **test** is used for evaluating the model. Ideally we want to have the objective variable as well. However in Kaggle competitions we don't have the values, because test validation is done using the platform

**Why do we need two sets?** to check how our model behaves in an independent set of data. This is data that has no relation to the training set and that is never used to fit the model (in any case)

**What is the validation set?** on top of these two sets, we can also use a **validation set**. This set is used in many machine learning tasks to control the decision making and avoid bias toward our train set. We can measure, for example, if our model is just memorizing the values it has seen.

Normally the **validation set** is a random (but sometimes stratified) small (around 20% of the train set) set from the training.

Let's split the `train_df` into `train_df` and `val_df` using `scikit-learn`

```python
from sklearn.model_selection import train_test_split

X_train, X_val, y_train, y_val = train_test_split(
  train_df.loc[:,categoric+numeric],
  train_df.loc[:,target],  
  test_size=0.2
)
```

## Pipeline Building

We will build a Pipeline to integrate prepossessing and estimation. We will do this using `scikit-learn`.


##### Reprocessing

During the prepossessing step we will build three `transformers`:
* Log transformer: to make the data more linear
* Imputer and scaler for numerical data
* Imputer and one hot encoder for categorical data

```python
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.preprocessing import FunctionTransformer


numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))])
```


Once we have the transformers, we build a `ColumnTransformer`.

```python
from sklearn.compose import ColumnTransformer

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric),
        ('cat', categorical_transformer, categoric)])
```

##### Estimating

For the `estimator` we will use a `LinearRegressor`. We basically define two steps: `preprocessor` and `regressor`.


```python
from sklearn.linear_model import LinearRegression

lr = Pipeline(steps=[('preprocessor', preprocessor),
                      ('regressor',LinearRegression())])
```

Now, all we have to do is to fit the pipeline.

```python
lr.fit(X_train, y_train)
```

Once it is fitted, we can predict values using the variables we have defined.

```python
y_pred = lr.predict(X_test)
```

## Evaluation

The question is, is this a good model? How can we know it.

We can define a measure of how good results are by comparing predicted values (`y_pred`) to actual values (`y_test`).

There are many ways of doing this, but we will overview two of them.

* **Mean Absolute Error:** measures the average magnitude of the errors in a set of predictions, without considering their direction. It’s the average over the test sample of the absolute differences between prediction and actual observation where all individual differences have equal weight.

* **Mean Squared Error:** measures the average of the squares of the errors—that is, the average squared difference between the estimated values and the actual value.

```python
from sklearn.metrics import mean_squared_error, mean_absolute_error

mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
```

**To what we compare?** we can define baselines, this means previous results so we can compare and see if we improve these previous results. If it's the first model, we can define default baselines like for example:
* **Random guess:** we select a random value between the max and mean
```python
y_test_max, y_test_min = np.max(y_test), np.min(y_test)
y_rand = np.random.uniform(y_test_min, y_test_max, y_test.shape[0])
rand_mse = mean_squared_error(y_test, y_rand)
rand_mae = mean_absolute_error(y_test, y_rand)
```
* **Constant prediction:** we select a constant value, like for example the mean of the prices

```python
y_test_mean = np.mean(y_test)
y_const = np.array([y_test_mean]*y_test.shape[0])
const_mse = mean_squared_error(y_test, y_const)
const_mae = mean_absolute_error(y_test, y_const)
```

##### Parameter Selection

For the estimation we have taken as regressor `LinearRegression()`. Note that we invoked this function with no parameters. This does not mean that it has no parameter, but that we have selected the default ones.

If we take a look at the function signature, we can see that it has few parameters.
```python
?LinearRegression
```

Normally, most used ML algorithms take a lot of parameters to be configured, like for example `RandomForests`, `SVMs`, etc.

How do we select these parameters:
* **Expertise:** we know very well how each parameter affects to learning and predicting and we set them by hand
* **Random Pick:** We pick some random values and try to adjust them so we can minimize/maximize our evaluation metrics
* **Grid Search**: We discretize our hyperparameter space and try all possible combinations to end with the one that minimizes our metrics
* **Hyperparameter Optimization**: We use a non-differentiable optimizer to find best hyperparamters

For all these methods, the same approach holds: train on as much as data as possible, decide over validation, evaluate over text

Let's see how Grid Search works:

```python

from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import GridSearchCV

rf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('regressor',RandomForestRegressor())])

param_grid = {
    'regressor__n_estimators': [200, 500],
    'regressor__max_features': ['auto', 'sqrt', 'log2'],
    'regressor__max_depth' : [4,5,6,7,8]}

CV = GridSearchCV(rf, param_grid, n_jobs= 1, scoring="neg_mean_absolute_error")

CV.fit(X_train, y_train)  
print(CV.best_params_)    
print(CV.best_score_)
```
