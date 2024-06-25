## Table: Seat

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| student     | varchar |


id is the primary key (unique value) column for this table.
Each row of this table indicates the name and the ID of a student.
id is a continuous increment.

## Problem: 
Write a solution to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.

Return the result table ordered by id in ascending order.

The result format is in the following example.

## Example 1:

### Input: 
Seat table:
| id | student |
-----|----------
| 1  | Abbot   |
| 2  | Doris   |
| 3  | Emerson |
| 4  | Green   |
| 5  | Jeames  |


### Output: 
| id | student |
|----|---------|
| 1  | Doris   |
| 2  | Abbot   |
| 3  | Green   |
| 4  | Emerson |
| 5  | Jeames  |

Explanation: 
Note that if the number of students is odd, there is no need to change the last one's seat.

## Solution

```sql 
SELECT 
    CASE WHEN id = (SELECT MAX(id) FROM Seat) AND id % 2 = 1 
        THEN id
        WHEN id % 2 = 1
        THEN id + 1
        WHEN id % 2 = 0 
        THEN id - 1
        END AS id,
    student
FROM Seat
ORDER BY id ASC
```

### Explanation 

To swap odd and even numbers, use id % 2 to check the parity:

* If id % 2 = 1, it's an odd number. Add 1 to the id.
* If id % 2 = 0, it's an even number. Subtract 1 from the id.

However, the last odd number does not get swapped (e.g., id = 5 in this example). To address this, we must check whether the id is the maximum value using a subquery `SELECT MAX(id) FROM Seat` 

Finally, sort the results by ID.


---