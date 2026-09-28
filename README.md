# Customer Segmentation with RFM Analysis and K-Means

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
