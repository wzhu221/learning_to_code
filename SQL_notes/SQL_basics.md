# Introduction to SQL

SQL is short for Structured Query Language. It is used to manipulate databases.

## Housekeeping

> NB... SQL is **case-insensitive**, *e.g.*, `SELECT` is equivalent to `select` and `Select`.

> NB... It is recommended that a semicolon `;` is appended to the end of each SQL sentence.

In a tabulated database, each **row** is called an **observation** and each **column** is called a **variable**.

## `select`

The `select` function can be used as follows:

```sql
select 'col_name' from 'tab_name' where 'conditions';
```

To select all columns, use `select * from ...`

### logical operators for `where`

| operator | meaning |
| --- | --- |
| `=` | is, equal to |
| `<>` / `!=` | is not, not equal to |
| `>` / `<` | greater / less than |
| `>=` / `<=` | greater than or equal to / less than or equal to |
| `between` | in a certain range |
| `like` | conforming to a certain pattern |

Use single quotation marks around strings, but no quotation marks around floats / integers

### multiple conditions

`and` and `or` can be used when multiple conditions are needed when using `select`. It should be used as follows:

```sql
select 'col_name' from 'tab_name' where 'condition_A' and 'condition_B';
```

They can also be nested:

```sql
select 'col_name' from 'tab_name' where ('condition_A' and 'condition_B') or 'condition_C';
```

This means that observations in the database `tab_name` will be selected if the variable `col_name` is either one of the two cases: both `conditions A and B`, or `condition C`. Conditions can be grouped using a pair of parentheses.













































































