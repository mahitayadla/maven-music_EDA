# 🎵 Maven Music Customer Churn Analysis

This project analyzes customer data for Maven Music, a music streaming service, to identify factors contributing to customer churn and develop data-driven strategies for improving customer retention. The analysis transforms raw customer data into actionable insights that can guide business decisions and reduce subscription cancellations.

# Tech Stack 

Python
<img width="225" height="225" alt="image" src="https://github.com/user-attachments/assets/4d83ba92-0f50-4f2b-962e-cda0d76aba51" />

# Data Source
The dataset used in this project was provided through a Udemy course on [Python Data Science: Data Prep & EDA with Python](https://www.udemy.com/course/data-science-in-python-data-prep-eda/?srsltid=AfmBOoqUzd9qwlxAtlDCwfd0nvb6J4K1so2AANmw9J-fYzrGrFiBedBr), containing three months of customer subscription and listening history for predictive analysis.


# Skills Highlighted in the Project


**- Data Cleaning & Preprocessing** – correcting data types, handling missing values and outliers, standardizing categories.

**- Data Manipulation & Analysis –** filtering, grouping, aggregating, merging datasets, performing correlation analysis using Pandas & NumPy.

- **Data Visualization & Dashboarding –** building charts and plots with Seaborn & Matplotlib to highlight trends and insights.

**- Feature Engineering –** creating new metrics to track user engagement, churn patterns, and content preferences.



## Data Cleaning and Preprocessing

Before any meaningful analysis could be performed, it was crucial to clean and preprocess the raw datasets (maven_music_customers.csv and maven_music_listening_history.xlsx) to ensure data quality, consistency, and reliability. This stage involved several key steps:

**1. Correcting Data Types**

The initial data types were not suitable for numerical or time-series analysis. Several columns required conversion to ensure they could be used correctly in calculations and modeling.

**2. Handling Missing Values (NaNs)**

The customer dataset contained null values in key columns that needed to be addressed.
- Several customers had a NaN value for their Subscription Plan. By cross-referencing with the Subscription Rate, I discovered that all of these missing plans corresponded to the $2.99 rate. Based on other entries, this rate was identified as the "Basic (Ads)" plan, and the missing values were filled accordingly using .fillna().
- The Discount? column had NaN values, which represented the absence of a discount. This was transformed into a binary numerical column using np.where(), making it suitable for correlation analysis and modeling.

**3. Adressing Outliers and Inconsistent Data**

A review of the descriptive statistics revealed potential data entry errors that could skew the analysis.
- The .describe() function showed a maximum Subscription Rate of $99.99, which was a significant outlier. By filtering the data, I isolated the single record with this typo and corrected it to the appropriate rate of $9.99 based on its "Premium (No Ads)" plan, ensuring the accuracy of our statistical summaries.

**4. Standardizing Categorical and Text Data**

To ensure consistency for grouping and analysis, categorical and text fields were standardized.
- The Genre column in the audio data contained both "Pop" and "Pop Music" as distinct categories. These were consolidated into a single "Pop" category to prevent fragmented analysis and ensure accurate counts.

With the data cleaned, validated, and properly formatted, it was now ready for in-depth exploratory analysis and feature engineering.



## Feature Engineering

To bridge the gap between raw data and actionable insights, I engineered new metrics to quantify user engagement, churn patterns, and content preferences.

**1. Creating a Binary Churn Indicator**

The original dataset indicated churn with a Cancellation Date. To make this feature usable for correlation analysis and future modeling, I created a binary Cancelled flag. Using np.where(), I assigned a 1 to any user with a non-null cancellation date and a 0 to all others. This created a clear, binary target variable representing whether a customer had churned.

**2. Quantifying User Engagement**

A user's activity level is a key predictor of their loyalty. To capture this, I calculated the Number of Sessions for each customer. By grouping the listening_history data by Customer ID and counting the unique Session IDs, I created a metric that summarized each user's overall engagement with the platform over the three-month period.

**4. Deriving Content Preference Metrics**

To understand if specific listening habits were linked to churn, I needed to quantify each user's preference for different content types.
- Genre Analysis: After identifying Pop as the most listened-to genre, I engineered a Pop percent feature. This was calculated for each user by dividing their total number of Pop songs listened to by their total number of audio plays.
- Content Type Analysis: Similarly, I created a Podcast percent feature by aggregating plays from all podcast-related genres (like Comedy and True Crime) and calculating their proportion of a user's total listening history.
These percentage-based features normalized the listening data, allowing for fair comparisons between users with different levels of activity and revealing the strong positive correlation between high pop consumption and churn.


## Data Manipulation & Analysis

Once the data was cleaned, the core of the project involved manipulating and analyzing the datasets to uncover patterns in customer behavior and identify factors contributing to churn. This was accomplished using the powerful data manipulation capabilities of Pandas and NumPy.

**1. Merging Multiple Data Sources**

To create a holistic view of each customer, it was necessary to combine information from four separate data sources:

- customer_data
- listening_history
- audio_data
- sessions_data
This consolidated dataset was the foundation for all subsequent analysis.

**2. Grouping and Aggregation**

To transform raw, transactional listening data into meaningful, user-level metrics, the .groupby() function was essential. I used it to aggregate data by Customer ID and calculate key performance indicators, including:
- Total Listening Sessions: By grouping by customer and counting the number of unique Session IDs, I quantified each user's engagement level.
- Genre Preferences: After one-hot encoding the Genre column, I grouped by customer and summed the values to get a total count of songs listened to per genre for each user. This was critical for understanding content preferences.
These aggregated metrics formed the core of the feature-engineered dataset used for the final analysis.

**3. Correlation Analysis**

To statistically validate the hypotheses generated during the EDA, a correlation matrix was computed using the .corr() method on the final analytical DataFrame. This matrix revealed the strength and direction of relationships between the engineered features and customer churn.

<img width="572" height="138" alt="image" src="https://github.com/user-attachments/assets/eb14de0f-1460-4fc2-b37f-023025674c2f" />

# Business Insights

The exploratory data analysis and correlation analysis yielded several key insights into the behaviors of customers who are most likely to churn. 

**Insight #1:** Discounted Subscriptions Correlate with Higher Churn
The analysis revealed a notable positive correlation between customers who received a subscription discount and those who ultimately cancelled. This suggests that while discounts are effective for initial customer acquisition, these users may perceive less long-term value in the service once the promotional period ends, leading to a higher churn rate within this segment.

**Insight #2:** Higher Engagement Leads to Better Retention
There is a strong negative correlation between the number of listening sessions and cancellation. Customers who engage with the platform more frequently are significantly less likely to churn. The analysis showed that most users have a low number of sessions (2-4 over three months), highlighting a large, low-engagement segment that is at high risk.

**Insight #3:** Pop Music Dominance is a Surprising Churn Indicator
A higher percentage of listening history dedicated to Pop music was positively correlated with cancellation. This may indicate that users who exclusively listen to mainstream Pop might not be fully exploring the platform's diverse catalog (like Jazz, Country, or Podcasts). 

**Insight #4**: Podcast Listening is Not a Significant Factor (Yet)
The analysis showed no significant correlation between podcast listening and customer retention or churn. This might also be due to limited amount of data avvailable.
