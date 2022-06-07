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
        account_no
    FROM
        t1
    WHERE
        -- assume that "in the last 3 months" means between today and counting backwards 3 months
        latest_transaction <= CURRENT_DATE AND latest_transaction >= (CURRENT_DATE - INTERVAL "3 MONTHS")
)
SELECT
    a.customer_no AS customers
FROM
    account AS a
JOIN
    t2
ON
    a.account_no = t2.account_no
WHERE
    a.product_type_code <> "BUSS" AND
    a.open_date >= "2018-07-01"

```
