## Table: Accounts

| Column Name | Type |
|-------------|------|
| account_id  | int  |
| income      | int  |

`account_id` is the primary key (column with unique values) for this table.
Each row contains information about the monthly income for one bank account.


Write a solution to calculate the number of bank accounts for each salary category. The salary categories are:

* `Low Salary`: All the salaries strictly less than $20000.
* `Average Salary`: All the salaries in the inclusive range [$20000, $50000].
* `High Salary`: All the salaries strictly greater than $50000.

The result table must contain all three categories. If there are no accounts in a category, return 0.

Return the result table in any order.

The result format is given in the following example-

## Example 1:

### Input: 

Accounts table:

| account_id | income |
|------------|--------|
| 3          | 108939 |
| 2          | 12747  |
| 8          | 87709  |
| 6          | 91796  |

### Output: 

| category       | accounts_count |
|----------------|----------------|
| Low Salary     | 1              |
| Average Salary | 0              |
| High Salary    | 3              |

### Explanation: 
Low Salary: Account 2.
Average Salary: No accounts.
High Salary: Accounts 3, 6, and 8.

## Solution 

### Approach: 
* To ensure the resultant table includes all categories, even with a count of 0 for categories with no accounts, we use `UNION` to combine each category separately instead of using `CASE` or `IF` with `COUNT`. This guarantees that all categories are represented in the result.

#### Query that won't return the categories with no accounts ❌ 

```sql 
SELECT 
    CASE WHEN income < 20000 THEN 'Low Salary'
    WHEN (income >= 20000 AND income <= 50000) THEN 'Average Salary'
    WHEN income > 50000 THEN 'High Salary'
    END AS category,
COUNT(account_id) AS accounts_count    
FROM Accounts
GROUP BY category
```
#### Correct solution using `UNION`✅

```sql 
SELECT 
    'Low Salary' AS category,
SUM(income < 20000) AS accounts_count
FROM Accounts 

UNION 
    SELECT 
        'Average Salary' AS category,  
    SUM(income >= 20000 and income <=50000) AS accounts_count
    FROM Accounts

UNION 
    SELECT 
        'High Salary' AS category,
    SUM(income > 50000) AS accounts_count
    FROM Accounts
```
* Note: A boolean expression returns `TRUE` (1) when a condition is met and `FALSE` (0) when it isn't. Hence why we don't use `CASE` or `IF` statements. 

---