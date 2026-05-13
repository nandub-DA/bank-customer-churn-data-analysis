# **BANK CUSTOMER CHURN ANALYSIS**
**Overview**

Retaining existing customers is often more cost-effective than acquiring new ones. This project analyzes a banking dataset containing information on customers from France, Germany, and Spain to identify why they are leaving the bank (churning).

The goal of this analysis is to uncover the key drivers of churn and provide actionable insights into customer behavior across different demographics.

**Objectives**

Through this analysis, I aim to answer the following business questions:
* What attributes are more common among churners than non-churners?
* Can churn be predicted using the variables in the data?
* What do the overall demographics of the bank's customers look like?
* Is there a difference between German, French, and Spanish customers in terms of account behavior?
* What types of segments exist within the bank's customers?


**Methodology**

To reach these insights, the project is structured into three main phases:
* Data Cleaning & Transformation
* Exploratory Data Analysis (EDA)
* Insight Generation & Visualisation

Environment: Jupyter Notebook (Interactive development and documentation)
Data Manipulation: Pandas & NumPy (Data cleaning, transformation, and Exploratory data analysis)
Visualization: Matplotlib, Seaborn & Plotly (Exploratory data analysis and statistical plotting)

# **DATASET**
* Source: https://mavenanalytics.io/data-playground/bank-customer-churn
* Data contains: Two excel sheets which are two tables 'Customer_Info' and 'Account _Info'. And the columns are:

CustomerId: A unique identifier for each customer
Surname: The customer's last name
CreditScore: A numerical value representing the customer's credit score
Geography: The country where the customer resides (France, Spain or Germany)
Gender: The customer's gender (Male or Female)
Age: The customer's age
Tenure: The number of years the customer has been with the bank
Balance: The customer's account balance
NumOfProducts: The number of bank products the customer uses (e.g., savings account, credit card)
HasCrCard: Whether the customer has a credit card (1 = yes, 0 = no)
IsActiveMember: Whether the customer is an active member (1 = yes, 0 = no)
EstimatedSalary: The estimated salary of the customer
Exited: Whether the customer has churned (1 = yes, 0 = no)

# **STEPS FOLLOWED**

Loaded two sheets in an excel file as two data frame 'ai' (Account_Info) and 'ci' (Customer_Info)

## **DATA INSPECTION**
* Verified that data loaded correctly.
* Checked each columns data type and found EstimatedSalary column in ci dataframe and Balance column in ai dataframe have wrong data type (str).
* Checked number of non-null values in each column and found there are null values in Surname column and Age column in dataframe ci.
* Verified number of columns and rows.
* Verified that there is no extra spaces, inconsistent casing, and hidden characters in the column names.
* Checked number of unique values in each columns and found that there is inconsistent categories in Geography column of dataframe ci ('FRA', 'French', 'France').
* Found that salary has negative values.

## **DATA CLEANING AND TRANSFORMATION**

### WRONG DATA TYPES
* Removed '€' symbol from EstimatedSalary and Balance column for converting data type.
* Converted EstimatedSalary column to float data type.
* Converted Balance column to float data type.

### TRIMMED EXTRA SPACES IN VALUES

### HANDLING MISSING VALUES
* Checked dataframe ci and ai for missing values.
* ci has 3 missing values in both 'Surname' and 'Age' column.
* Missing values in Age column is flagged by creating a new column 'AgeMissing' where 1 means Missing and 0 means Not Missing.
* Missing values in Surname column will not affect the analysis. So ignoring it.

### DUPLICATE ROWS
* Checked for duplicate rows in ci and ai
* There were two duplicate rows in ai and one in ci
* Dropped duplicate rows

### LOGICALLY IMPOSSIBLE VALUES
* Found illogical negative values(-999999) in three rows in EstimatedSalary column.
* The three missing Age, Surname rows and the illogical EstimatedSalary values are in same rows. So removed those three rows.
* Deleted AgeMissing flag column. Because there is no Age missing rows in data anymore.
* Checked for logically impossible credit score
* Checked for logically impossible age
* Checked for logically impossible tenure
* Checked for logically impossible estimated salary

### MERGING DATAFRAMES
* Merging dataframe ci and ai using CustomerId as the key for the ease of analysis.
* Assigned it in variable 'bc'.
* Removed one Tenure column to avoid repeating it.

### INCONSISTENT CATEGORIES
* Corrected inconsistent categories 'FRA' and 'French' in Geography column to 'France'.

### OUTLIERS
* Checked with a box plot
* Even though there are statistical outliers in Age, there are no logical outliers in Age ranges from 18 to 92.
* Even though there are statistical outliers in CreditScore, there are no logical outliers in CreditScore ranges from 350 to 850.
* No outliers in EstimatedSalary and Balance.

### CROSS-FIELD INCONSISTENCY
* Validated the relationship between Age and Tenure. While some customers joined the bank prior to age 18 (Age - Tenure < 18), these records were retained as they likely represent student or minor accounts that transitioned into adult accounts. No "impossible" joiner ages were detected.

### REPLACING VALUES
* In 'Exited' column 1 means Yes and 0 means No.
* Though this project is not for machine learning purpose I am changing the 1 and 0 to Yes and No for the easiness of understanding.

## **EXPLORATORY DATA ANALYSIS**

### **UNIVARIATE ANALYSIS**

### Summary statistics
* To find mean, median (50%), standard deviation, IQR (75%-25%) of all numerical columns.
* To find number of unique values and most frequent values in categorical column.

### Outliers
* Checking statistical outliers again to understand data.

### Credit score
* Credit Score (Range: 500 points): Scores span from 350 to 850. This wide gap suggests there are customers with very poor credit and those with perfect credit.
* The alignment of Mean and Median around 650 indicates a normal distribution of credit scores.
* A noticeable spike at 850 indicates a significant segment of customers with maximum credit scores.
* The standard deviation is 96.65 points. While the full range is wide (350–850), following the empirical rule, approximately 68% of customers have a credit score within one std of the mean (roughly between 550 and 750).

### Tenure
* The data shows a Uniform Distribution between years 1 and 9, indicating consistent customer acquisition over the last decade.
* The lower counts at Tenure 0 and 10 represent the natural entry and exit points of the customer lifecycle.
* The Mean and Median are both exactly 5 years, with an Interquartile Range (IQR) spanning from 3 to 7 years.

### Estimated salary
* Follows a Uniform Distribution, indicating an equal representation of all income brackets within the customer base.
* The Mean and Median are nearly identical at 100,000, confirming a lack of skewness or influential outliers.

### Customers geography
* 50% of the customer base is located in France. France has a high frequency of 5,013 customers, which is double the count for Germany (2,508) or Spain (2,476).
* This proves the bank is primarily French-based, meaning the overall findings will be heavily influenced by the behaviors of this majority.

### Customers gender
* 54.57% of customers are male and 45.4% are female.

### Number of products used by each customer
* The distribution of NumOfProducts is heavily concentrated at 1 (5081 customers) and 2 (4590) units, accounting for 96.7% of the total customer base.
* The Median of 1.0 indicates that the majority of the bank's clients uses only a single product.
* The sharp "cliff" after 2 products suggests that 3 or 4 products are statistical anomalies for this bank's current model. Should prioritize investigating these segments.

### Customers having and not having credit cards
* 51.5% of customers have credit card and 48.5% have no credit card.

### Active and inactive customers
* 48.5% (4,848 customers) of customers are classified as inactive, representing a significant segment at risk for churn.

### Churned customers
* 20.37% of total customers have churned.

### Age
* Age (Range: 74 years): With a minimum of 18 and a maximum of 92, bank serves everyone from young students to retirees.
* The customer base is right-Skewed, with a median age of 37 and a mean of 38.9.
* A significant "long tail" exists for customers aged 60+. That requires specialized churn analysis.
* Most customers (50%) are concentrated in a 12-year window. The gap between 25% (32 years) and 75% (44 years) is small compared to the full range of 18–92.

### Account balance
* Balance (Range: 250,898): Range starts at 0 and goes up to over 250k.
* The data shows a Bimodal Distribution with a significant spike at 0, indicating a large segment of non-funded or dormant accounts.
* 36.17% of total customer accounts have 0 balance.
* Excluding the zero-balance peak, the remaining data follows a Normal Distribution centered around 120,000.
* The Median (97,188) is a more robust measure of central tendency here than the Mean (76,482), which is heavily skewed by the high volume of zero-balance entries.

### Account balance
* Balance (Range: 250,898): Range starts at 0 and goes up to over 250k.
* The data shows a Bimodal Distribution with a significant spike at 0, indicating a large segment of non-funded or dormant accounts.
* 36.17% of total customer accounts have 0 balance.
* Excluding the zero-balance peak, the remaining data follows a Normal Distribution centered around 120,000.
* The Median (97,188) is a more robust measure of central tendency here than the Mean (76,482), which is heavily skewed by the high volume of zero-balance entries.

### **BIVARIATE AND MULTIVARIATE ANALYSIS**

#### Geography
* At a 32.45% churn rate, a German customer is twice as likely to leave as a French or Spanish one.

#### Credit Score
* Poor credit score category has 22.16% churn rate. Which is a little higher than total customer churn rate of 20.37%.

#### Gender
* Females are significantly more likely to churn (25%) than males (16.45%).

#### Age
* Among 40-50 age group churn rate is 33.9% and among 50-60 it's 56.2% (Over half left). And among 60+ is 24.78%. Older customers tend to churn more.
* Churn rate is just 7.45% among customers who joined as minors. Their retention rate is 92.5%.

#### Tenure
* Churn rate is 23% among customers have 0 year tenure. 22.4% for 1 year. 21.6% for 9 year. Churn rate seems a little high in new customers.

#### Estimated Salary
* 21.56% churn rate among very high salary category. Others are 20 or below. Very High Salary group shows a slight uptick in churn.

#### Balance
* The difference between median salary of churned (109349.29) and retained (92063) customers shows that the bank is losing its high value customers.
* Zero balance accounts have 13.8% of churn. Low balance accounts have 33.66 churn. Medium have 19.88 and high balance have 25.22% of churn.

#### Number Of Products
* 50% (5081) of customers use only 1 product. 45.9% (4590) of customers use 2 products. 2.66% (266) use 3 products. 0.6% (60) use 4 products.
* All customers who used to use 4 products were churned (100%). 82.7% of customers who used to use 3 products were churned. Churn is 7.58% in 2 product users. 27.7% is churn rate of 1 product users.

#### Has Credit Card
* 26.85% of no credit card users churned. 14.27% of credit card users churned.

#### Is Active Member
* 26.85% of inactive members churned. 14.27% of active members churned. Being an active member nearly cuts churn in half (14.27% vs 26.85%).
* And another interesting fact here is all credit card holders are active members and all non credit card holders are inactive members.

* **GEOGRAPHY + AGE + CHURN**

**Why:** Found that Germany has a 32.45% churn rate and the 50–60 age group has a 56.2% churn rate.
* While the global churn rate is 20.37% and overall churn rate among 50-60 age group is 56.2%, German customers in this age bracket exhibit a 69.5% churn rate.

**ACTIVITY + NUMBER OF PRODUCTS USING + CHURN**

**Why:** Found that 3-product users churn at 82% and Inactive members churn at 26%.
* Activity significantly boosts retention for 1 and 2-product holders, but its effectiveness evaporates as product counts increase. Customers with 4 products exhibit a 100% churn rate regardless of their activity status.

**GENDER + BALANCE + CHURN**

**Why:** Found females and high-balance customers churn more (~25%).
* High-Balance Females churn at a 30.4% rate, nearly 10 percentage higher than males in the same wealth bracket.
* The bank is facing a 46.5% churn rate among Low-Balance Females.

**BALANCE + ACTIVITY + CHURN**

**Why:** Found that Zero-Balance accounts have very low churn (13.8%).
* 51.76% of zero balance accounts are active.
* Churn is very less (9.6%) among active zero balance accounts.

**AGE + ACTIVITY + CHURN**

**Why:** Found that older age groups and inactive customers have high churn.
* Among inactive accounts of 60+, 82% churned. In 50-60 it's 85.7% and in 40-50 it's 41.6%.
* It is clear that inactive accounts of older age group churns extremely high. 

#### **WHAT ATTRIBUTES ARE MORE COMMON AMONG CHURNERS THAN NON-CHURNERS?**
*20.37% is the churn rate among the total customer base.*

Churners are disproportionately associated with:

* **Demographics & Geography**
    * Customers from Germany (32.45% churn vs ~16% in others)
    * Older customers, especially 40+, with peak churn in 50–60 age group (56.2%)

* **Behaviour & Engagement**
    * Inactive members (26.85% churn vs 14.27% active)
    * Customers with only 1 product (27.7% churn)
    * Customers with 3 or more products (extremely high churn, 82–100%)

* **Financial Profile**
* Low balance customers (33.66% churn)
* Higher balance customers also show elevated churn (25.22%).

* **Gender**
    * Female customers (25% churn vs 16.45% males)
    * Particularly:
        * Low-balance females (46.5% churn)
        * High-balance females (30.4% churn)

* **Early Tenure**
    * Customers with 0–1 year tenure show slightly higher churn (~22–23%)

#### **CAN CHURN BE PREDICTED USING THE VARIABLES IN THE DATA?**
* Yes, churn can be predicted using these variables:
  * Age
  * Geography
  * Activity status
  * Number of products
  * Balance
  * Gender

These variables show strong separation between churners and non-churners

#### **WHAT DO THE OVERALL DEMOGRAPHICS OF THE BANK'S CUSTOMERS LOOK LIKE?**
* **Age**
    * The customer base is tightly concentrated in the working-age bracket. About 50% fall between 32 and 44 years, with a median age of 37. That tells that “typical” customer is mid-career, not young entrants or retirees.
    * The distribution is right-skewed, meaning there are fewer older customers stretching the tail up to 92. So it’s not youth-heavy, it’s slightly tilted towards older ages, but the core mass is still mid-aged.
* **Gender**
   * 54.57% of customers are male and 45.4% are female. This is a mild imbalance, not extreme, but still noticeable.
* **Geography**
    * France: 50%, Germany: 25%, Spain: 25%
    * France clearly dominates the customer base with 50%. So the business is heavily dependent on one region.
* **Financial status**
    * Most customers fall in the fair credit score category (581–670). That’s below “good”, means the bank is not dealing with a high-credit-quality base. Higher default risk compared to a “good” or “excellent” base.
    * Salary is uniformly distributed. That means all income levels are equally represented.
    * 36.17% of customers have zero balance. This is a big red flag.

#### **IS THERE A DIFFERENCE BETWEEN GERMAN, FRENCH, AND SPANISH CUSTOMERS IN TERMS OF ACCOUNT BEHAVIOR?**
* **Balance comparison**
  * Germans hold more money in their accounts. Their mean balance is 119721 and median is 119699.
  * 66.84% of total zero balance accounts are from France and 33.15% are from Spain. Germans don't hold any zero balance accounts.
* **Product usage**
  * France and Spain show a very similar distribution, with customers almost evenly split between 1 and 2 products.
  * Germany has a higher concentration of single-product customers, indicating lower product adoption.
  * Although Spain’s median is 2, the distribution shows that customers are still nearly evenly divided between 1 and 2 products. This means the median slightly overstates the difference. Spain is only marginally different from France
* **Activity level**
  * Spain have the highest 52.98% active members and  Germany have the least with 49.7%.
* **Credit card ownership**
  * Spain have the highest 52.98% credit card holders and  Germany have the least with 49.7%.
* **Tenure**
  * Average tenure of customers from all countries seems 5.

#### **WHAT TYPE OF SEGMENTS EXISTS WITHIN THE BANK'S CUSTOMERS?**

* **Stable Core Customers**
    * France
    * Active members
    * 2 products
    * Medium or above balance (>50000)
    * Males
    * Age 18-40

Their churn rate is very low (4.9%).

They are the most stable and core customers of the bank.

But they are just 1.22% of the total customers. It's concerning.

* **High-Value but At-Risk Customers**
    * High balance (>100000)
    * Germany
    * Age 50-60 (52% high balance)
    * Inactive

High churn (90%) despite high value

Most critical segment

They are 1.1% of total customers

* **Engaged Multi-Product Users**
    * Active
    * 2 products

Their churn rate is just 5.56%.

Strong retention.

They consists of 24.46% of total customers.

* **Dormant Customers**
    * Inactive
    * Zero Balance

Weak relationship with bank.

They cosists of 17.44% of total customers.

* **Early-Stage Customers**
  * Low tenure
  * Inactive

Churn rate is 30.15%. Needs monitoring.

They consists of 6.6% of total customers.

### **INSIGHTS, RECOMMENDATION AND VISUALIZATION**

**1. OVERALL CHURN LEVEL**
* Total churn rate is 20.37%, indicating a significant retention problem

***RECOMMENDATION:***
* Focus on targeted retention rather than broad strategies, prioritising high-risk segments

**Used pie chart**

**2. HIGH-RISK CUSTOMER SEGMENT**
* Customers aged 40 and above, especially inactive users, show the highest churn
* Churn peaks at 56.2% in the 50–60 age group and it is 85.7% in inactive 50-60 accounts.

***RECOMMENDATION:***
* Target this segment with retention campaigns, personalised offers, and engagement strategies before churn happens

**Used pie charts**

**3. HIGH-VALUE CUSTOMERS ARE AT RISK**
* Customers with high account balances contribute the majority of churn volume
* A significant portion (59.4%) of churners come from high-value segments.

***RECOMMENDATION:***
* Prioritise retention of high-balance customers through premium services, relationship management, and loyalty benefits

**Used bar chart** 

**4. ACTIVITY STATUS IS A STRONG CHURN DRIVER**
* Inactive customers (48.5% of base) show significantly higher churn (26.85%)

***RECOMMENDATION:***
* Improve customer engagement through reminders, offers, and usage incentives to reduce inactivity-driven churn

**Used pie charts** 

**5. PRODUCT USAGE**
* Customers with 2 products are the most stable segment
* Customers with only 1 product show higher churn (27.7%)
* 3–4 product churn is extremely high but segment size is very small, so limited business impact

***RECOMMENDATION:***
* Encourage cross-selling to move customers from 1 to 2 products
* Check if churn happens after specific product combinations
* Improve post-sale engagement instead of just cross-selling

**Used pie charts** 

**6. GENDER-BASED CHURN DIFFERENCE**
* Female customers churn more (25%) compared to males (16.45%)

***RECOMMENDATION:***
* Investigate possible differences in service experience or product suitability across genders

**Used bar chart**

**7. HIGH VALUE GERMAN CUSTOMERS**
* The high churn rate among German customers is particularly concerning because they represent the most valuable segment of the customer base as high-balance account holders.
* 78.3% of German accounts have high balance. And they do not hold any zero balance accounts.

***RECOMMENDATION:***
* Investigate service quality and competition in that region
* Design region-specific retention strategies instead of global ones

**Used pie charts** 

**8. EARLY-AGE ONBOARDING DRIVES STRONG RETENTION**
* Customers who joined as minors show very low churn (7.4%) vs overall 20.37%.

***RECOMMENDATION:***
* Invest in acquiring younger customers
* Design long-term engagement strategies to grow their value over time

**Used bar chart**
