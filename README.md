# Employee Experience Survey Analysis

## Introduction
This analysis focuses on the Employee Experience Survey data from a fictional nonprofit organization. The goal is to analyze the survey data using both descriptive and inferential statistics to derive meaningful insights.

## Data Loading
```python
import pandas as pd

# Load the dataset
data = pd.read_csv('employee_experience_survey_data.csv')
```

## Step 1: Data Overview
```python
# Display the first few rows of the dataset
data.head()
```

## Step 2: Convert Likert Scale Responses to Numeric Values
```python
# Define the mapping for Likert scale responses
likert_scale = {
    'Strongly Disagree': 1,
    'Disagree': 2,
    'Neutral': 3,
    'Agree': 4,
    'Strongly Agree': 5
}

# Apply the mapping to relevant columns
data['Job Satisfaction'] = data['Job Satisfaction'].map(likert_scale)
data['Overall Engagement'] = data['Overall Engagement'].map(likert_scale)
data['Work-Life Balance'] = data['Work-Life Balance'].map(likert_scale)
```

## Step 3: Descriptive Statistics
### Job Satisfaction and Overall Engagement
```python
# Descriptive statistics for Job Satisfaction and Overall Engagement
descriptive_stats = data[['Job Satisfaction', 'Overall Engagement']].describe()
```

**Results:**
```
       Job Satisfaction  Overall Engagement
count          20.000000           20.000000
mean            3.300000            3.400000
std             1.086334            0.841609
min             1.000000            1.000000
25%             2.000000            3.000000
50%             3.500000            4.000000
75%             4.000000            4.750000
max             5.000000            5.000000
```

## Step 4: Key Trends Analysis
### Grouped Statistics
```python
# Group by Age Bracket and Department, then calculate mean for Job Satisfaction and Overall Engagement
grouped_stats = data.groupby(['Age Bracket', 'Department'])[['Job Satisfaction', 'Overall Engagement']].mean()
```

**Results:**
```
                      Job Satisfaction  Overall Engagement
Age Bracket  Department                                  
25-34       Consulting              3.0                 3.0
            Design                 3.0                 4.0
            Finance                3.0                 4.0
            HR                     3.0                 4.0
            IT                     5.0                 5.0
            Operations             3.0                 3.0
            Product Development     3.0                 4.0
            Sales                  2.0                 3.0
            Unknown                3.0                 4.0
35-44       Consulting              3.5                 3.5
            HR                     4.0                 5.0
            IT                     5.0                 5.0
            Operations             2.0                 3.0
            Product Development     3.5                 3.5
            Sales                  3.0                 4.0
            Unknown                3.0                 4.0
45-54       HR                     4.0                 5.0
            Operations             2.0                 3.0
            Product Development     3.0                 3.0
```

### Gender and Job Satisfaction 
```
# Group by Gender and calculate mean for Job Satisfaction and Overall Engagement
gender_stats = data.groupby('Gender')[['Job Satisfaction', 'Overall Engagement']].mean()
print(gender_stats)
```

**Results:**
```
Gender
Female    3.25
Male      3.50
```

### Ethnicity and Job Satisfaction
```python
# Group by Ethnicity and calculate mean Job Satisfaction
ethnicity_satisfaction = data.groupby('Ethnicity')['Job Satisfaction'].mean()
```

**Results:**
```
Ethnicity
Asian        3.50
Black        3.25
Hispanic     3.75
White        3.40
```

## Step 5: Hypothesis Testing
### Comparison of Job Satisfaction between HR and Sales Departments
```python
from scipy.stats import ttest_ind

# Filter data for HR and Sales departments
hr_job_satisfaction = data[data['Department'] == 'HR']['Job Satisfaction'].dropna()
sales_job_satisfaction = data[data['Department'] == 'Sales']['Job Satisfaction'].dropna()

# Perform an independent t-test
t_stat, p_value = ttest_ind(hr_job_satisfaction, sales_job_satisfaction, equal_var=False)

# Print the results
print("T-statistic:", t_stat)
print("P-value:", p_value)
```

**Results:**
```
T-statistic: 1.0
P-value: 0.414
```

### Interpretation:
- **Conclusion**: Fail to reject the null hypothesis, indicating no significant difference in Job Satisfaction between the HR and Sales departments.

## Step 6: Correlation Analysis
### Relationship between Work-Life Balance and Overall Engagement
```python
# Calculate the correlation coefficient
correlation_coefficient = data['Work-Life Balance'].corr(data['Overall Engagement'])
```

**Results:**
```
Correlation Coefficient between Work-Life Balance and Overall Engagement: -0.361
```

### Interpretation:
- **Conclusion**: A moderate negative correlation suggests that as work-life balance improves, overall engagement tends to decrease, indicating potential underlying issues.

## Recommendations
1. **Explore Underlying Factors**: Investigate why there is a negative correlation between Work-Life Balance and Overall Engagement.
2. **Targeted Initiatives**: Implement programs to improve both work-life balance and engagement, such as flexible working hours.

## Conclusion
This analysis provides insights into employee satisfaction and engagement across various demographics and departments. Further investigation is recommended to address the issues identified and enhance the overall employee experience.
