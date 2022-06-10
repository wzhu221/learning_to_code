```SQL

-- using PostgreSQL for the following code

WITH t2 AS (
    WITH t1 AS (
        SELECT DISTINCT
            customer_no,
            account_no,
            -- acquire the latest record of transaction
            MAX(trans_date) OVER (PARTITION BY account_no) AS latest_transaction
        FROM
            transaction
    )
    SELECT
        customer_no
    FROM
        t1
    WHERE
        -- assume that "in the last 3 months" means between today and counting backwards 3 months
        latest_transaction <= CURRENT_DATE AND latest_transaction >= (CURRENT_DATE - INTERVAL '3 MONTHS')
)
SELECT DISTINCT
    a.customer_no
FROM
    account as a
LEFT JOIN
    t2
ON
    a.customer_no = t2.customer_no
WHERE
    t2.customer_no IS NULL AND
    a.product_type_code <> 'BUSS' AND
    a.open_date >= '2018-07-01'


```
