# SQL-problem-Customer-Order-Frequency


<img width="616" alt="Screenshot 2024-08-20 at 14 01 56" src="https://github.com/user-attachments/assets/87bbe1c5-194f-4422-8e10-cc4ae1fd8373">


<img width="720" alt="Screenshot 2024-08-20 at 14 02 43" src="https://github.com/user-attachments/assets/d3df63b3-073a-4de7-ab8b-66bb070af76c">


The result format is in the following example.

<img width="632" alt="Screenshot 2024-08-20 at 14 03 17" src="https://github.com/user-attachments/assets/7c35fa0a-4444-4f41-ab39-17b1a54ba2b8">

<img width="691" alt="Screenshot 2024-08-20 at 14 03 43" src="https://github.com/user-attachments/assets/ab42a4e6-6b80-4486-bb3f-172c00e019c8">




## Solution:

<img width="627" alt="Screenshot 2024-08-20 at 14 08 58" src="https://github.com/user-attachments/assets/98d0c521-5177-4896-b6b5-836406db2934">



WITH at_least_100_id_June AS (

    SELECT customer_id, 
           SUM(price * quantity) AS spent
    FROM Orders
    LEFT JOIN Product
    ON Orders.product_id = Product.product_id
    WHERE MONTH(order_date) = 6 AND YEAR(order_date) = 2020
    GROUP BY customer_id
    HAVING spent >= 100
),

at_least_100_id_July AS (

    SELECT customer_id, 
           SUM(price * quantity) AS spent
    FROM Orders
    LEFT JOIN Product
    ON Orders.product_id = Product.product_id
    WHERE MONTH(order_date) = 7 AND YEAR(order_date) = 2020
    GROUP BY customer_id
    HAVING spent >= 100
)

SELECT customer_id,
       name
FROM Customers
WHERE customer_id IN (SELECT customer_id FROM at_least_100_id_June)
  AND customer_id IN (SELECT customer_id FROM at_least_100_id_July)
ORDER BY customer_id;
