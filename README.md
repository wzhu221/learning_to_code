```SQL

-- using PostgreSQL for the following code


WITH latest_trans_on_account AS (
    SELECT
        account_no,
        -- acquire the latest record of transaction
        MAX(trans_date) AS latest_transaction
    FROM
        transaction
    GROUP BY
        account_no
    HAVING
        -- assume that "in the last 3 months" means between today and counting backwards 3 months
        MAX(trans_date) <= CURRENT_DATE AND MAX(trans_date) >= (CURRENT_DATE - INTERVAL '3 MONTHS')
)
SELECT DISTINCT
    a.customer_no AS customers
FROM
    account
JOIN
    latest_trans_on_account AS latest
ON
    account.account_no = latest.account_no
WHERE
    a.product_type_code <> 'BUSS' AND
    a.open_date >= '2018-07-01'

```
