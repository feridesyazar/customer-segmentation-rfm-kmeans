# 🛍️ Customer Segmentation with RFM Analysis and K-Means

## Overview

This project develops an end-to-end customer segmentation workflow using transactional customer data.

The main objective is to group customers according to their purchasing behavior so that businesses can better understand their customer base and support targeted marketing strategies.

The project combines:

- Data preparation with Pandas
- Relational data storage with SQLite
- RFM analysis
- Feature scaling
- K-Means clustering
- Elbow Method
- Silhouette Score
- Customer segment profiling
- Marketing interpretation

---

## Business Objective

A company wants to segment its customer base in order to better understand customer buying behavior and support targeted marketing.

Instead of treating all customers the same, segmentation makes it possible to identify groups such as:

- frequent and high-value customers
- loyal repeat customers
- recent but occasional customers
- customers at risk of becoming inactive

These groups can then be approached with different marketing strategies.

---

## Dataset

The original dataset contains:

- **4,194 rows**
- **181 columns**

It includes customer, order, product, and order-item information.

The original dataset is separated into three main DataFrames:

- **Customers**
- **Orders**
- **Products**

After preparation:

| Table | Records |
|---|---:|
| Customers | 3,054 |
| Orders | 4,194 |
| Products | 1,710 |

---

## Project Workflow

The project follows the workflow below:

1. Load the original dataset
2. Create Customers, Orders, and Products DataFrames
3. Store the three tables in SQLite
4. Import the tables back from SQLite
5. Merge the tables for analysis
6. Prepare date and order information
7. Create the RFM table
8. Standardize RFM features
9. Evaluate different cluster solutions
10. Select the number of clusters
11. Train the final K-Means model
12. Analyze customer segment characteristics
13. Develop marketing interpretations

---

## SQLite Database

The Customers, Orders, and Products DataFrames are stored in a SQLite database.

This creates a simple relational database workflow and allows the project to simulate working with data retrieved from a business database.

The tables are then imported back into Pandas and merged using customer and product identifiers.

---

## RFM Analysis

Customer behavior is summarized using **RFM Analysis**.

### Recency

How many days have passed since the customer's most recent purchase.

A lower Recency value means the customer purchased more recently.

### Frequency

The number of unique orders placed by the customer.

A higher Frequency value indicates a more frequent customer.

### Monetary

The total amount spent by the customer.

A higher Monetary value indicates a higher-value customer.

The final RFM table contains:

- `customer_id`
- `Recency`
- `Frequency`
- `Monetary`

The RFM snapshot date is based on the day after the latest transaction in the dataset.

---

## Feature Scaling

The RFM variables have different numerical ranges.

For example, Monetary values can be much larger than Frequency values.

To prevent larger numerical values from dominating the clustering process, the RFM features are standardized using:

`StandardScaler`

The following features are used for clustering:

- Recency
- Frequency
- Monetary

---

## Selecting the Number of Clusters

The number of customer segments is evaluated using two complementary methods:

- **Elbow Method**
- **Silhouette Score**

### Elbow Method

The Elbow Method compares the Within-Cluster Sum of Squares (**WCSS**) for different values of `k`.

The Elbow curve begins to flatten around **k = 4**, indicating that additional clusters provide smaller improvements in WCSS.

### Silhouette Score

The Silhouette Score evaluates how well customers fit within their assigned clusters.

Selected results:

| k | WCSS | Silhouette Score |
|---:|---:|---:|
| 2 | 6001.6725 | 0.8796 |
| 3 | 3733.5261 | 0.5540 |
| 4 | 2950.9102 | 0.5743 |
| 5 | 2415.8032 | 0.5749 |
| 6 | 1988.4346 | 0.5104 |

Although **k = 2** produces the highest Silhouette Score, a two-cluster solution provides limited detail for targeted marketing.

The Silhouette Scores for **k = 4** and **k = 5** are very similar.

A **four-cluster solution** therefore provides a good balance between:

- the Elbow Method
- cluster separation
- business interpretability
- actionable customer segments

Therefore, **4 customer segments** were selected for the final K-Means model.

---

## Final K-Means Model

The final model uses:

```python
selected_k = 4
```

The final model achieved:

**Silhouette Score: 0.5743**

The K-Means algorithm identified four customer segments.

---

## Customer Segments

| Segment | Customers | Avg. Recency | Avg. Frequency | Avg. Monetary |
|---|---:|---:|---:|---:|
| Recent / Occasional | 2,044 | 108.35 | 1.07 | 97.62 |
| At Risk | 930 | 506.22 | 1.05 | 106.25 |
| Loyal High-Value | 67 | 183.49 | 3.99 | 995.89 |
| Champions | 13 | 78.38 | 11.23 | 3553.69 |

### Recent / Occasional

Customers who purchased relatively recently but generally have low purchase frequency.

### At Risk

Customers whose last purchase was a long time ago and may require reactivation.

### Loyal High-Value

Repeat customers with relatively high spending and higher purchase frequency.

### Champions

Very frequent, recent, and very high-value customers.

> The segment names are business interpretations of the RFM cluster profiles and are not labels provided by the original dataset.

---

## Marketing Interpretation

| Segment | Main Characteristic | Possible Marketing Action |
|---|---|---|
| **Champions** | Very frequent and high-spending customers | Loyalty rewards, premium offers, early access |
| **Loyal High-Value** | Repeat customers with high spending | Cross-selling, personalized recommendations |
| **Recent / Occasional** | Recent but mostly one-time customers | Encourage a second purchase, follow-up offers |
| **At Risk** | Long time since last purchase | Reactivation campaigns, targeted incentives |

---

## Technologies Used

- Python
- Pandas
- NumPy
- SQLite
- Scikit-learn
- Matplotlib
- Seaborn
- Jupyter Notebook

---

## Key Results

- **3,054 customers** analyzed
- RFM customer profiles created
- SQLite database workflow implemented
- Elbow Method and Silhouette Score used for cluster evaluation
- **4 customer segments** selected
- Final Silhouette Score: **0.5743**
- Customer segments translated into actionable marketing strategies

---

## Conclusion

This project demonstrates how transactional customer data can be transformed into meaningful customer segments using RFM analysis and K-Means clustering.

The original data was first organized into Customers, Orders, and Products tables and stored in a SQLite database. After importing and merging the tables, RFM features were calculated for **3,054 customers**.

The number of customer segments was evaluated using both the **Elbow Method** and **Silhouette Score**. A four-cluster solution was selected to provide a balance between statistical separation and business interpretability.

The final customer segments were:

- **Recent / Occasional:** 2,044 customers
- **At Risk:** 930 customers
- **Loyal High-Value:** 67 customers
- **Champions:** 13 customers

These customer profiles can support targeted marketing activities such as reactivation campaigns, repeat-purchase strategies, personalized recommendations, loyalty programs, and premium offers.

Overall, the project shows how unsupervised machine learning can transform transactional data into actionable business insights.
