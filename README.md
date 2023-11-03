# Click-Prediction-Using-Spark (Classification)
---
---
<center><h1> Spark ML - Click Prediction </h1></center>

----

An Advertising company delivers more than 3 Billion clicks per month to its advertisers delivering 4.5 Million monthly sales events. 

It buys space on the Publishers site and then shows an advertisement about the Advertiser at that space. The advertiser pays the network for every conversion from the clicks. The network in turns pays to the publisher after keeping it's commission. The company wants to leverage the machine learning to improve the conversions for its customers.

So, in this notebook we will build a classification model to predict whether a particular advertisement will be clicked or not.

----

So for this notebook, we have divided the dataset into 3 different parts:
   * **`TRAIN`** - It has 1400000 Rows which will be used to train the model.
   * **`VALIDATION`** - It has 350000 Rows which will be used to evaluate the model.
   * **`TEST`** - It has 350000 Rows on which we will test our final model.



---

---

# `DATA EXPLORATION`

Now, that we have read the data, the first step is to explore the data. We will try to find out the following things from the data.

---
**`1. Variable Identification`**

- Data Type of the columns

**`2. Univariate Analysis`**
- Explore the Target Variable ( `ConversionStatus` )
- Check for the Null Values in each column
- Check for the Distinct Values in each column
- Frequency of each category for the following columns
    - Country
    - Browser
    - Device
    - OS

**`3. Bivariate Analysis`**

- Number of Clicks for each category of Country
- Number of Clicks for each catefory of Browser
- Number of Clicks for each catefory of Device
- Number of Clicks for each catefory of OS

---

##  `VARIABLE INDENTIFICATION`

#### `Data Type of the Columns`

---
---

**`Numerical`**
- ID
- Carrier (It is numerical in nature but it is `network code`, so we will treat it as `categorical variable`)
- Fraud
- advertiserCampaignId (It is numerical in nature but still values are code representing different campaigns, so we will treat it as `categorical variable`)

**`Categorical`**
- Country
- TrafficType
- Device
- Browser
- OS
- publisherId


**`Time-Based`**
- ClickDate

**`Boolean`**
- ConversionStatus (We will have to type cast it into the integer type to do some aggregation operations on it for the data exploration purposes.)


---

## `UNIVARIATE ANALYSIS`

#### `Exploring the Target Variable`

---

The `Target Variable` for our use-case is `ConversionStatus` and this column has `boolean` data type. Let's find out how much the data is imbalance, i.e. what percentage of data points has the target variable `true` and `false`?

---

---

## `BIVARIATE ANALYSIS`

---

#### `Number of Clicks for each category of Country`

---
---

## `Pre-Processing`

---

- We will fill the Null Values.
- We will do label-encoding the categorical variable.
- We will do one-hot encoding the categorical variables.


---

---

#### `Fill Null Values`
---

We will fill the null values with the following values.


- **`Country`** with **`IN`**: As most of the Ads were shown in this country.
- **`TrafficType`** with **`U`**: `U` means unidentified, we have discussed this in the exploration part.
- **`Device`** with **`1`**: `Generic` for 1 as they are unidentified devices.
- **`Broswer`** with **`chrome`**: As most of the data points have `chrome` browser.
- **`OS`** with **`Android`**: As most of the data points have `Android` OS.
- **`total_c_id`** with 0:  As for the new campaigns which comes in future, we have no data.
- **`total_p_id`** with 0:  As for the new campaigns which comes in future, we have no data.

---

---


## `MODEL BUILDING`

Now, we have prepared the dataset and it is ready to be trained with Machine Learning models. This is a `Classification Problem`, so we will train the data on the following alogrithms.

 * **Logistic Regression**
 * **Decision Tree Classification**



---

---

#### `VECTOR ASSEMBLER`

- Before passing the data into the ML model, we need to convert the required features into a Vector. We can do this using a `VectorAssembler`.
---


----

So, now we will select the features and create a vector using `VectorAssembler`

 * traffic (One Hot Encoded)
 * Fraud   (As it is)
 * total_p_id (Feature Created: Frequency per Publisher id)
 * total_c_id (Feature Created: Frequency per Campaign Id)
 * country_ohe (One Hot Encoded)
 * device_modified (Label Encoded)
 * browser_ohe (One Hot Encoded)
 * os_ohe (One Hot Encoded)

----


---

# `Model Tuning`

* **Cross-Validation**
* **Grid-Search**

---


----
----


### `Machine Learning Pipelines in Spark`

----


 *   `Transformer` 
 > - It transforms the input data (X) in some ways.
 > - Implements a method `transform()`, which converts one DataFrame into another, generally by appending one or more columns.
 > - It includes `feature transformers` and `learned models`.
 > > - `Feature transformer` should take a DataFrame, read a column (e.g., text), map it into a new column (e.g., feature vectors), and output a new DataFrame with the mapped column appended.
 > > -  `Learning model` should take a DataFrame, read the column containing feature vectors, predict the label for each feature vector, and output a new DataFrame with predicted labels appended as a column.
 *   `Estimator` 
 > - It predicts a new value (or values) (y) by using the input data (X).
 > - It implements a method `fit()`, which accepts a DataFrame and produces a `Model`, which is a `Transformer`.
 > > - For example, a learning algorithm such as `LogisticRegression` is an `Estimator`, and calling `fit()` trains a `LogisticRegressionModel`, which is a `Model` and hence a `Transformer`.
 * `Pipeline`
 > - It represents a sequence of steps to apply in an ML workflow. Example:
 > > - Stage 1 : Split text into words.
 > > - Stage 2 : Convert words into numeric data.
 > > - Stage 3 : Apply machine learning model on the numeric data.
 > - These steps are represented as `Transformers` or as `Estimators`.
 > - A `Pipeline` is comprised of `Stages`.
 > > - These stages are run in order.
 > > - The input DataFrame is transformed as it passes through each stage.
 > > - Each stage is either a `Transformer` or an `Estimator`.
 
 ---
 

