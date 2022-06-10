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
), 
t3 AS (
    SELECT
        a.customer_no,
        a.account_no,
        CASE 
            WHEN crdt.crdt_open_date IS NULL THEN a.open_date
            WHEN crdt.crdt_open_date IS NOT NULL THEN crdt.crdt_open_date
        END AS true_acc_open_date
    FROM
        (
            SELECT
                customer_no,
                account_no,
                open_date
            FROM
                account
            WHERE
                product_type_code <> 'BUSS'
        ) AS a
    LEFT JOIN
        (
            SELECT
                CONCAT('000', card_no) AS crdt_card_no,
                TO_DATE(open_date, 'YYYYMMDD') AS crdt_open_date
            FROM
                credit_card_account 
        ) AS crdt
    ON
        a.account_no = crdt.crdt_card_no
)
SELECT DISTINCT
    t3.customer_no
FROM
    t3
LEFT JOIN
    t2
ON
    t3.customer_no = t2.customer_no
WHERE
    t2.customer_no IS NULL AND
    t3.true_acc_open_date >= '2018-07-01'
;


```
