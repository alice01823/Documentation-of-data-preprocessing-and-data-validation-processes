# Data Preprocessing and Data Validation Documentation

## 1. Introduction
This document outlines the steps taken to preprocess and validate the data for our analysis. The goal is to ensure that the dataset is clean, consistent, and ready for exploration.

## 2. Data Preprocessing Steps

### Step 1: Handling Missing Values
Missing values were identified in the following columns:
- `AnnualIncome`

Action taken:
- For the `AnnualIncome` column, missing values were filled using the column's mean value:
    ```python
    mean_income = df['AnnualIncome'].mean()
    df['AnnualIncome'] = df['AnnualIncome'].fillna(mean_income)
    ```

### Step 2: Removing Duplicates
We checked for duplicate rows in the dataset:
- No duplicate rows were found, ensuring that each entry is unique.

    ```python
    df.drop_duplicates(inplace=True)
    ```

### Step 3: Data Type Validation
We ensured that each column has the correct data type:
- Example: `AnnualIncome` was validated to be a numerical type (`float`).

    ```python
    print(df.dtypes)
    ```

Action taken if necessary:
- If columns had incorrect data types, they were converted accordingly:
    ```python
    df['AnnualIncome'] = pd.to_numeric(df['AnnualIncome'], errors='coerce')
    ```

### Step 4: Normalization and Transformation
Normalization of certain fields was performed to ensure data consistency:
- Example: We scaled income data if necessary using standard normalization techniques.

    ```python
    from sklearn.preprocessing import MinMaxScaler
    scaler = MinMaxScaler()
    df[['AnnualIncome']] = scaler.fit_transform(df[['AnnualIncome']])
    ```

## 3. Data Validation Processes

### Step 1: Validation of Filled Values
After filling missing values, we confirmed that no `NaN` values remained in the critical columns:
    ```python
    print(df['AnnualIncome'].isnull().sum())  # Output should be 0
    ```

### Step 2: Data Consistency Checks
We ran checks to ensure that no invalid or inconsistent data remained:
- Check for negative income values:
    ```python
    assert (df['AnnualIncome'] >= 0).all(), "AnnualIncome contains negative values"
    ```

### Step 3: Outlier Detection
Outliers in `AnnualIncome` were detected and addressed using the interquartile range (IQR) method:
    ```python
    Q1 = df['AnnualIncome'].quantile(0.25)
    Q3 = df['AnnualIncome'].quantile(0.75)
    IQR = Q3 - Q1
    outliers = df[(df['AnnualIncome'] < (Q1 - 1.5 * IQR)) | (df['AnnualIncome'] > (Q3 + 1.5 * IQR))]
    ```

Action taken:
- Depending on the findings, outliers were either investigated further or removed.

## 4. Conclusion
The data preprocessing and validation processes ensured that the dataset is clean and reliable for further analysis. Key steps included handling missing values, removing duplicates, validating data types, and identifying outliers.

---

### Call to Action
If you have any questions or would like to contribute, feel free to raise an issue or open a pull request!

## http://localhost:8888/lab/tree/Data%20processing%20%26%20Data%20validation

## 5. Today's Task - 2024-09-28
**Task Description**: Completed data visualization for customer behavior analysis.

**Steps Taken**:
1. Grouped the dataset by `Gender` and `EducationLevel` and aggregated the order quantities.
2. Created a bar plot to visualize total order quantity by gender and education level using Seaborn.

```python
customer_behavior = merged_sales_customers_df.groupby(['Gender', 'EducationLevel'])['OrderQuantity'].agg(['mean', 'sum', 'count']).reset_index()
plt.figure(figsize=(12, 6))
sns.barplot(data=customer_behavior, x='Gender', y='sum', hue='EducationLevel')
plt.title('Total Order Quantity by Gender and Education Level')
plt.show()

##Outcome: Successfully visualized customer behavior trends, which will help in targeted marketing strategies.

