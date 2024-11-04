# LITA-Capstone-Project-2

## Project Title: Customer Segmentation for a Subscription Service

### Table of Contents
- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Tools Used](#tools-used)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis and Visualization](#data-analysis-and-visualization)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)

---

### Project Overview

This project focuses on analyzing customer subscription data to uncover key insights on demographics, subscription trends, and cancellation patterns. Using SQL for data preparation, Power BI for visualization, and Excel for calculations, I developed a comprehensive dashboard suite to aid in customer segmentation and subscription analysis. This project demonstrates my skills in data cleaning, trend analysis, and dashboard creation, helping stakeholders understand subscription dynamics and make data-driven decisions.

---

### Objectives
The primary objectives of this project are:

- To classify customers by subscription type and analyze their revenue contributions.
- To examine regional revenue distribution and customer concentration for growth opportunities.
- To identify trends in subscription duration, active vs. canceled subscriptions, and customer loyalty.

---

### Tools Used

- **Excel**: For data cleaning, calculations, and pivot table analysis.
- **SQL**: For data segmentation, key metric calculations, and subscription trend exploration.
- **Power BI**: To create interactive dashboards, providing visual insights on customer and revenue distribution.
- **GitHub**: For organizing and showcasing this project as part of my portfolio.

---

### Data Cleaning and Preparation

**Data preparation steps ensured accuracy and consistency across tools**:

- **Data Import and Format Checks**: Imported data into Excel, verifying field types for date, text, and numerical consistency.
- **Handling Missing Values**: Identified and removed rows with missing values to ensure data integrity.
- **Standardization**: Standardized text in columns like `Region`, `SubscriptionType`, and `CustomerName` to prevent duplicate records due to format inconsistencies.
- **Derived Columns**:
  - **Subscription Duration**: Calculated as the difference between `SubscriptionStart` and `SubscriptionEnd` in Excel.
  - **Active/Cancelled Status**: Derived by evaluating if `SubscriptionEnd` has a date, indicating a canceled subscription.
  - **Revenue per Subscription**: Aggregated revenue per customer across active and canceled subscriptions.

**Key SQL Queries Used for Cleaning and Preparation**: 

- **Duplicate Management**:
  ```SQL
  SELECT *, COUNT(*) AS duplicate_count
  FROM CustomerData
  GROUP BY CustomerID, CustomerName, Region, SubscriptionType, SubscriptionStart, SubscriptionEnd, Canceled, Revenue, "Subscription Duration"
  HAVING duplicate_count > 1;

  DELETE FROM CustomerData
  WHERE rowid NOT IN (
      SELECT MIN(rowid)
      FROM CustomerData
      GROUP BY CustomerID, CustomerName, Region, SubscriptionType, SubscriptionStart, SubscriptionEnd, Canceled, Revenue, "Subscription Duration"
  );
  ```

- **Null Value Detection and Removal**:
  ```SQL
  SELECT COUNT(*) AS null_count
  FROM CustomerData
  WHERE CustomerID IS NULL OR CustomerName IS NULL OR Region IS NULL
        OR SubscriptionType IS NULL OR SubscriptionStart IS NULL
        OR SubscriptionEnd IS NULL OR Canceled IS NULL OR Revenue IS NULL
        OR "Subscription Duration" IS NULL;

  DELETE FROM CustomerData
  WHERE CustomerID IS NULL OR CustomerName IS NULL OR Region IS NULL
        OR SubscriptionType IS NULL OR SubscriptionStart IS NULL
        OR SubscriptionEnd IS NULL OR Canceled IS NULL OR Revenue IS NULL
        OR "Subscription Duration" IS NULL;
  ```

---

### Exploratory Data Analysis
Key questions explored in this analysis:

1. Which subscription types drive the most revenue?
2. How does revenue distribution vary across regions?
3. What are the characteristics of active vs. canceled subscriptions?
4. What are the average subscription duration and cancellation rates?

These insights aid in understanding revenue, subscription type popularity, and customer loyalty.

---

### Data Analysis and Visualization

1. **Excel Analysis**: I used Excel online, so you can download the file here: [Download Here](https://1drv.ms/x/c/41bec79bae4bb512/EaOvzB2De4dKh3UD2P5_T08BaV3IeyUJoaf8c_w6c3HF8w?e=Ne8teT).

   - **Key Formulas Used**:
     - Average Subscription Duration: `=AVERAGE(SubscriptionDurationRange)`
     - Active Subscriptions: `=COUNTIF(CancelledRange, "FALSE")`
     - Cancelled Subscriptions: `=COUNTIF(CancelledRange, "TRUE")`
     - Cancellation Rate: `=COUNTIF(CancelledRange, "TRUE") / COUNTA(CustomerNameRange)`
     - Average Revenue per Subscription: `=AVERAGE(RevenueRange)`

   Pivot tables were used to summarize revenue by region, count subscriptions, and average subscription durations.

   **Visualization**: ![Pivot Table for Customer Data](https://github.com/user-attachments/assets/e5268713-7ac9-493f-9f84-4f51bbca6500)

2. **SQL Analysis**

   - **Total Number of Customers from Each Region**:
     ```SQL
     SELECT Region, COUNT(CustomerID) AS Total_Customers
     FROM CustomerData
     GROUP BY Region
     UNION ALL
     SELECT 'Total', COUNT(CustomerID)
     FROM CustomerData;
     ```

     **Visualization**: ![SQL Total Number of Customers from Each Region](https://github.com/user-attachments/assets/594d08e5-1101-47d6-9125-61e475a6e97f)

   - **Total Revenue by Subscription Type**:
     ```SQL
     SELECT SubscriptionType, SUM(CAST(REPLACE(Revenue, ',', '') AS INTEGER)) AS Total_Revenue
     FROM CustomerData
     GROUP BY SubscriptionType
     UNION ALL
     SELECT 'Total', SUM(CAST(REPLACE(Revenue, ',', '') AS INTEGER))
     FROM CustomerData;
     ```

     **Visualization**: ![SQL Total Revenue by Subscription Type](https://github.com/user-attachments/assets/e707e7ef-b4ce-4bab-b4f3-39e0a042951a)

   - **Top 3 Regions by Subscription Cancellation**:
     ```SQL
     SELECT Region, COUNT(CustomerID) AS Cancellations
     FROM CustomerData
     WHERE Canceled = 'TRUE'
     GROUP BY Region
     ORDER BY Cancellations DESC
     LIMIT 3;
     ```

     **Visualization**: ![SQL Top 3 Regions by Subscription Cancellations](https://github.com/user-attachments/assets/f70bde7d-2d91-4f46-963b-2098505b0027)

3. **Power BI Visualizations**: [Download Here](https://app.powerbi.com/groups/me/reports/1defa032-0b23-405a-9b42-7e89fdb081b6?ctid=b6de804f-51cd-47ef-a151-26514ed475f0&pbi_source=linkShare&bookmarkGuid=c26374cf-d21e-4a4f-8c66-1f0883790118)

---

### Key Findings

**What is Working**

- **Active Subscription Distribution**:
  - *Basic Subscription*: High active count in the East (8,488) and North (3,366), indicating strong regional preference.
  - *Premium and Standard*: Active counts concentrated in the South (Premium, 3,382) and West (Standard, 3,376), hinting at regional inclinations for premium tiers.

- **No Early Cancellations Within Six Months**: This suggests that customers generally remain engaged during the initial period, indicating effective onboarding or initial satisfaction with the service.

- **Revenue from Premium Subscribers**: Despite a lower count, Premium subscriptions show high revenue potential, especially in the South.

**What Needs Improvement**

- **High Cancellation Rate Across Tiers**:
  - *Basic*: Highest cancellations in the North (5,067), implying that the Basic model may not meet expectations there.
  - *Premium and Standard*: High cancellations in the South and West, respectively, suggesting that value perception may need improvement.

---

### Recommendations

1. **Targeted Retention Strategies**:
   - Basic Subscription (North): Region-specific engagement initiatives, such as tailored onboarding and targeted content.
   - Premium and Standard (South and West): Loyalty programs and personalized incentives to improve retention.

2. **Enhanced Value Proposition**: Align subscription benefits with customer expectations. Consider adding features for each tier to enhance perceived value.

3. **Regionally Tailored Campaigns**: Focus on campaigns that highlight the unique benefits of each subscription type to improve satisfaction and retention.

---

### Conclusion

This project provides actionable insights into customer behavior within a subscription service, identifying key areas for revenue enhancement and retention. The use of Excel, SQL, and Power BI enables a thorough analysis of the subscription landscape, empowering stakeholders to make strategic, data-backed decisions. Future directions could include tracking the impact of recommended strategies and refining segmentation based on customer feedback.

--- 
