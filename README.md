```SQL

WITH t2 AS (
    WITH t1 AS (
        SELECT DISTINCT
            customer_no,
            account_no,
            MAX(trans_date) OVER (PARTITION BY account_no) AS latest_transaction
        FROM
            transaction
    )
    SELECT
        account_no
    FROM
        t1
    WHERE
        -- assume that "last 3 months" means Mar, Apr, and May of 2022
        latest_transaction <= '2022-05-31' AND latest_transaction >= '2022-03-01'
)
SELECT
    a.customer_no
FROM
    account AS a
JOIN
    t2
ON
    a.account_no = t2.account_no
WHERE
    a.open_date >= '2018-07-01'

```
