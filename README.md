# Aurora-Bank-Customer-Profiling-and-Segmentation-Project
![Aurora Bank](https://github.com/user-attachments/assets/9343483d-a557-4dfc-bea2-8ade56e559ba)
## Introduction
Aurora Bank is a dynamic financial institution that leverages data to drive strategic decisions. This project focuses on customer profiling and segmentation, aiming to provide a deep understanding of customer behaviors, financial health, and risk levels. Also, actionable insights to refine marketing approaches, improve risk assessment, and enhance customer engagement.

## Objective
To gain a deep understanding of customer demographics, financial health, and behaviors to enable personalized marketing strategies and assess customer risk levels.

## Focus Areas:
* **Customer Demographics**
* **Credit Score Analysis**
* **Financial Health**
* **Card Ownership**

## 1. Customer Demographics

### a. Gender Distribution

```sql
TOTAL CUSTOMERS BY GENDER
SELECT gender, COUNT(id) total_customers
FROM users_data
GROUP BY gender
ORDER BY total_customers;
```

<img width="477" alt="piechart" src="https://github.com/user-attachments/assets/be010741-ad93-4acc-a698-30a10164fbaf" />


The customer base is nearly evenly split, with **50.8%** being female and **49.2%** male. While the difference is minor, it indicates a slightly stronger female presence.


### b. Age Distribution

```sql
CUSTOMER AGE DISTRIBUTION
SELECT MAX(current_age) Oldest_Customer,
        MIN(current_age) Youngest_Customer,
        AVG(current_age) Average_Age
FROM users_data;
```
<img width="392" alt="cust_age_dist" src="https://github.com/user-attachments/assets/807c72a9-f369-44d0-8eb9-e75d410781ed" />


Customers range from **18** to **101** years old, with an average age of **45** years.


### c. Age Group Segmentation

```sql
SELECT
    CASE
        WHEN birth_year BETWEEN 1901 AND 1927 THEN 'The Greatest Generation(1901-1927)'
        WHEN birth_year BETWEEN 1928 AND 1945 THEN 'The Silent Generation(1928-1945)'
        WHEN birth_year BETWEEN 1946 AND 1964 THEN 'Baby Boom Generation(1946-1964)'
        WHEN birth_year BETWEEN 1965 AND 1980 THEN 'Generation X(1965-1980)'
        WHEN birth_year BETWEEN 1981 AND 1996 THEN 'Millennial Generation(1981-1996)'
        ELSE 'Generation Z(1997-2010)'
    END AS age_group,
    COUNT(*) total_customers
FROM users_data
GROUP BY
    CASE
        WHEN birth_year BETWEEN 1901 AND 1927 THEN 'The Greatest Generation(1901-1927)'
        WHEN birth_year BETWEEN 1928 AND 1945 THEN 'The Silent Generation(1928-1945)'
        WHEN birth_year BETWEEN 1946 AND 1964 THEN 'Baby Boom Generation(1946-1964)'
        WHEN birth_year BETWEEN 1965 AND 1980 THEN 'Generation X(1965-1980)'
        WHEN birth_year BETWEEN 1981 AND 1996 THEN 'Millennial Generation(1981-1996)'
        ELSE 'Generation Z(1997-2010)'
    END
ORDER BY total_customers DESC;
```

<img width="678" alt="colchart" src="https://github.com/user-attachments/assets/bac88170-4088-420f-b50c-8eae149a9a4c" />


The majority of customers fall within **Generation X (601)** and **Millennials (562)**, comprising **58.15%** of the total. **Baby Boomers (428)** follow, while **Generation Z (236)** and older generations such as the **Silent Generation (162)** and **Greatest Generation (11)** form a smaller portion. This indicates that middle-aged groups are the primary customer segment.

### Business Implications:

* The near-even gender split suggests the bank serves a balanced market and has the opportunity to engage both male and female customers equally.
* An average customer age of **45** highlights that middle-aged individuals are the core demographic, likely in their peak earning and borrowing years.
* Generation X and Millennials make up the majority of the customer base, implying that the bank’s products and communication strategies should align with the preferences and digital behavior of these age groups.
* Older generations (Silent and Greatest Generations) represent a smaller share, indicating less demand for digital-first or credit-heavy offerings in these segments.

---
## 2. Credit Score Analysis

### a. Credit Score Distribution

```sql
SELECT MAX(credit_score) Highest_Score,
        MIN(credit_score) Lowest_Score,
        AVG(credit_score) Avg_Score
FROM users_data;
```
<img width="582" alt="CreditScoreDist" src="https://github.com/user-attachments/assets/488462d9-4530-4e5f-b162-3deb134361f1" />


Credit scores range from **480** to **850**, with an average of **709**. 


```sql
SELECT
    CASE
        WHEN credit_score BETWEEN 800 AND 850 THEN 'Exceptional'
        WHEN credit_score BETWEEN 740 AND 799 THEN 'Very Good'
        WHEN credit_score BETWEEN 670 AND 739 THEN 'Good'
        WHEN credit_score BETWEEN 580 AND 669 THEN 'Fair'
        ELSE 'Poor'
    END AS Credit_Score_Category,
    COUNT(*) AS total_customers,
    CAST(ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM users_data), 2) AS DECIMAL(5,2)) AS percent_of_total
FROM users_data
GROUP BY
    CASE
        WHEN credit_score BETWEEN 800 AND 850 THEN 'Exceptional'
        WHEN credit_score BETWEEN 740 AND 799 THEN 'Very Good'
        WHEN credit_score BETWEEN 670 AND 739 THEN 'Good'
        WHEN credit_score BETWEEN 580 AND 669 THEN 'Fair'
        ELSE 'Poor'
    END
ORDER BY total_customers DESC;
```

<img width="608" alt="CreditScoreDist2" src="https://github.com/user-attachments/assets/d407970e-c946-467f-a9ec-b41c5b3de705" />

<img width="858" alt="barchart" src="https://github.com/user-attachments/assets/28e00be0-a677-4daa-ac5e-e8041f9c0546" />


Nearly half the customers (**46.55%**) fall into the "Good" category, followed by "Very Good" (**23.70%**) and "Fair" (**17.40%**). A smaller percentage are in the "Exceptional" (**8.30%**) and "Poor" (**4.05%**) categories. This suggests that the majority have moderate financial health.



### b. Factors Influencing Credit Scores

```sql
SELECT
    CASE
        WHEN credit_score BETWEEN 800 AND 850 THEN 'Exceptional'
        WHEN credit_score BETWEEN 740 AND 799 THEN 'Very Good'
        WHEN credit_score BETWEEN 670 AND 739 THEN 'Good'
        WHEN credit_score BETWEEN 580 AND 669 THEN 'Fair'
        ELSE 'Poor'
    END AS credit_score_category,
    ROUND(AVG(yearly_income), 2) AS avg_income,
    ROUND(AVG(total_debt), 2) AS avg_debt
FROM users_data
GROUP BY
    CASE
        WHEN credit_score BETWEEN 800 AND 850 THEN 'Exceptional'
        WHEN credit_score BETWEEN 740 AND 799 THEN 'Very Good'
        WHEN credit_score BETWEEN 670 AND 739 THEN 'Good'
        WHEN credit_score BETWEEN 580 AND 669 THEN 'Fair'
        ELSE 'Poor'
    END
ORDER BY credit_score_category;
```

<img width="504" alt="FinfCreditScore" src="https://github.com/user-attachments/assets/88df0ca5-6e47-47a3-81d7-b6be27a3b5df" />


**Debt-to-Income Ratio:** 

Customers with "Fair" and "Poor" scores carry the highest average debt (**$78,340** and **$74,023**, respectively) relative to income (**$45,742.92**, and **$46,247.28**), resulting in elevated debt-to-income ratios (**1.71** and **1.60**), indicating financial strain. The highest ratio is observed in the "Fair" category.


```sql
SELECT
    CASE
        WHEN birth_year BETWEEN 1901 AND 1927 THEN 'The Greatest Generation(1901-1927)'
        WHEN birth_year BETWEEN 1928 AND 1945 THEN 'The Silent Generation(1928-1945)'
        WHEN birth_year BETWEEN 1946 AND 1964 THEN 'Baby Boom Generation(1946-1964)'
        WHEN birth_year BETWEEN 1965 AND 1980 THEN 'Generation X(1965-1980)'
        WHEN birth_year BETWEEN 1981 AND 1996 THEN 'Millennial Generation(1981-1996)'
        ELSE 'Generation Z(1997-2010)'
    END AS age_group,
 ROUND(AVG(credit_score), 0) AS avg_credit_score,
 ROUND(AVG(num_credit_cards), 1) AS avg_cards
FROM users_data
GROUP BY
    CASE
        WHEN birth_year BETWEEN 1901 AND 1927 THEN 'The Greatest Generation(1901-1927)'
        WHEN birth_year BETWEEN 1928 AND 1945 THEN 'The Silent Generation(1928-1945)'
        WHEN birth_year BETWEEN 1946 AND 1964 THEN 'Baby Boom Generation(1946-1964)'
        WHEN birth_year BETWEEN 1965 AND 1980 THEN 'Generation X(1965-1980)'
        WHEN birth_year BETWEEN 1981 AND 1996 THEN 'Millennial Generation(1981-1996)'
        ELSE 'Generation Z(1997-2010)'
    END
ORDER BY avg_credit_score DESC;
```
<img width="553" alt="AgeCreditscores" src="https://github.com/user-attachments/assets/321bd755-24ad-4dd5-8f82-aa07490ca6af" />


**Age & Credit Scores:** 

Older generations, particularly the Greatest Generation (**739** average score) and Baby Boomers (**712**) have higher credit scores and more credit cards. This suggests better credit management.


### Business Implications:

* Most customers fall into the "Good" or "Very Good" credit score categories, indicating moderate financial responsibility and manageable credit risk overall.
* A small but notable share of customers in the "Fair" and "Poor" categories face significant financial pressure, evidenced by high debt-to-income ratios.
* Older generations tend to have higher credit scores and more credit cards, reflecting greater financial maturity and better credit behavior over time.
* Younger segments with lower scores may be struggling with debt management or lack of credit history, which could pose future credit risk or missed growth opportunities.

---

## 3. Financial Health & Risk Assessment

```sql
SELECT
    id AS Customer_id,
    yearly_income,
    total_debt,
    ROUND(total_debt / NULLIF(yearly_income, 0), 2) AS debt_to_income_ratio,
    CASE
        WHEN (total_debt / NULLIF(yearly_income, 0)) <= 0.35 THEN 'Low Risk (DTI ≤ 35%)'
        WHEN (total_debt / NULLIF(yearly_income, 0)) BETWEEN 0.36 AND 0.49 THEN 'Moderate Risk (36-49%)'
        ELSE 'High Risk (DTI ≥ 50%)'
    END AS risk_category
FROM users_data
WHERE yearly_income > 0  -- Exclude users with $0 income to avoid division errors
ORDER BY debt_to_income_ratio DESC;
```

<img width="438" alt="doughnutchart" src="https://github.com/user-attachments/assets/03aafb96-b62c-4b6e-ae52-53f1daf55180" />


A significant portion (**79.4%**) of customers fall into the "High Risk" category. The primary risk factors include high debt-to-income ratios in the "Fair" and "Poor" credit categories, where debt levels exceed income capacity. This highlights the need for financial intervention strategies for these segments.

### Business Implications:

* A high concentration (**79.4%**) of customers in the "High Risk" category signals a systemic issue with debt management or income stability.
* Customers in the "Fair" and "Poor" credit categories have debt levels that exceed their income, highlighting financial strain that could impact loan repayment and long-term customer value.
* This imbalance presents a business risk in terms of loan defaults and customer churn, particularly if financial literacy or intervention strategies are lacking.
* The presence of financial stress in a large portion of the base could hinder product uptake or reduce eligibility for premium services.

---


## 4. Card Ownership & Usage Trends


### a. Transactions by Age Group

Behavioral Analytics – Card Usage

```sql
SELECT
    CASE
        WHEN birth_year BETWEEN 1901 AND 1927 THEN 'The Greatest Generation(1901-1927)'
        WHEN birth_year BETWEEN 1928 AND 1945 THEN 'The Silent Generation(1928-1945)'
        WHEN birth_year BETWEEN 1946 AND 1964 THEN 'Baby Boom Generation(1946-1964)'
        WHEN birth_year BETWEEN 1965 AND 1980 THEN 'Generation X(1965-1980)'
        WHEN birth_year BETWEEN 1981 AND 1996 THEN 'Millennial Generation(1981-1996)'
        ELSE 'Generation Z(1997-2010)'
    END AS age_group,
    c.card_type,
    COUNT(t.id) AS total_transactions,
    ROUND(SUM(t.amount), 2) AS total_spent,
    ROUND(AVG(t.amount), 2) AS avg_transaction_amount
FROM transactions_data t
JOIN cards_data c ON t.card_id = c.id
JOIN users_data u ON t.client_id = u.id
GROUP BY
    CASE
        WHEN birth_year BETWEEN 1901 AND 1927 THEN 'The Greatest Generation(1901-1927)'
        WHEN birth_year BETWEEN 1928 AND 1945 THEN 'The Silent Generation(1928-1945)'
        WHEN birth_year BETWEEN 1946 AND 1964 THEN 'Baby Boom Generation(1946-1964)'
        WHEN birth_year BETWEEN 1965 AND 1980 THEN 'Generation X(1965-1980)'
        WHEN birth_year BETWEEN 1981 AND 1996 THEN 'Millennial Generation(1981-1996)'
        ELSE 'Generation Z(1997-2010)'
    END, c.card_type
ORDER BY total_spent DESC;
```

<img width="695" alt="TransAgeGrp" src="https://github.com/user-attachments/assets/a3c82d20-b74e-4aea-870d-b94ca05a60f9" />

<img width="658" alt="treemap" src="https://github.com/user-attachments/assets/c4c1f418-7e77-4936-a382-ce23ce005fdc" />


**Generation X spent the most:**

* Debit: **47,801** transactions worth **$1.8M**
* Credit: **20,048** transactions worth **$1.1M**

Female customers prefer debit cards (**53,182** transactions, **$2.06M** spent) and spend more per credit transaction (**$56.75** on average).
Male customers spend slightly more per debit transaction (**$40.32** vs. **$38.83** for females) but use credit cards less frequently.


### b. Card Type Preferences

Debit cards are the most widely used, whereas prepaid cards remain the least utilized across all demographics.


### Business Implications:

* Debit cards dominate usage, indicating a strong customer preference for immediate-access spending and conservative financial behavior.
* Generation X leads in both debit and credit card activity, suggesting they are highly engaged and potentially more receptive to value-added services.
* Millennials and Baby Boomers also display high debit usage, implying opportunities for cross-product engagement rather than aggressive credit marketing.
* Female customers use debit cards more frequently and spend more per credit transaction, hinting at a value-conscious approach to borrowing and spending.
* Male customers spend slightly more per debit transaction, which may reflect different purchase behavior or financial priorities.
* Prepaid cards are rarely used, signaling that customers either lack awareness or do not find value in them under the current positioning.

---

#### Dashboard
<img width="798" alt="dashboard" src="https://github.com/user-attachments/assets/b2f092de-e9f5-4800-bed3-035954401a7b" />


## Recommendations

Based on the insights derived from the data analysis, the following strategies are recommended to drive business growth, enhance customer engagement, and minimize financial risk:

* **Segmented Marketing and Product Offerings:** Tailor marketing campaigns and product offerings to the distinct needs of Generation X, Millennials, and Baby Boomers, as these groups represent the largest and most engaged customer segments. Focus on offering financial products that align with their specific life stages, such as retirement planning for older generations and investment opportunities for younger, high-spending cohorts.
* **Gender-Specific Strategies:** Leverage gender-based insights to customize financial products and marketing initiatives. For female customers, emphasize financial planning tools and rewards for credit usage. For male customers, introduce value-driven debit products and targeted merchant discounts that align with their spending behavior.
* **Credit Improvement and Risk Management:** Offer credit improvement programs for customers with Fair and Poor credit scores, including access to debt counseling, loan consolidation, and financial literacy resources. Implement stricter credit approval processes for high-risk segments to prevent defaults and protect the bank's financial stability. Additionally, introduce targeted debt management solutions such as personalized repayment plans and financial wellness tools to improve customer financial health.
* **Enhancing Debit and Credit Product Engagement:** Given the heavy usage of debit cards, there’s a strong opportunity to introduce additional features like cashback rewards, budgeting tools, and automated savings programs to retain and engage customers. For credit card users, particularly those in Generation X and Millennials, consider exclusive loyalty programs and premium credit product offers to drive higher spending.
* **Prepaid Card Strategy:** Reposition prepaid cards as budgeting tools for younger or high-risk customers, and market them as gift cards or travel solutions to increase adoption across different demographics. This can help diversify product usage and attract customers who may be wary of traditional banking products.


## Conclusion:

By adopting these recommendations, Aurora Bank can create a more customer-centric, risk-aware, and financially inclusive environment that addresses the diverse needs of its customer base. Implementing tailored marketing strategies, optimizing credit and debit products, and focusing on financial wellness will enhance customer loyalty, minimize financial risks, and ultimately drive sustained business growth. These actionable insights will not only improve customer engagement but also strengthen the bank's position in a competitive financial landscape.
