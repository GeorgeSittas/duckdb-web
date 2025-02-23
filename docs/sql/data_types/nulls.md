---
layout: docu
title: NULL Values
blurb: The NULL value represents a missing value.
---

`NULL` values are special values that are used to represent missing data in SQL. Columns of any type can contain `NULL` values. Logically, a `NULL` value can be seen as "the value of this field is unknown".

```sql
-- insert a null value into a table
CREATE TABLE integers (i INTEGER);
INSERT INTO integers VALUES (NULL);
```

`NULL` values have special semantics in many parts of the query as well as in many functions:

> Any comparison with a `NULL` value returns `NULL`, including `NULL=NULL`.

You can use `IS NOT DISTINCT FROM` to perform an equality comparison where `NULL` values compare equal to each other. Use `IS (NOT) NULL` to check if a value is `NULL`.

```sql
SELECT NULL=NULL;
-- returns NULL
SELECT NULL IS NOT DISTINCT FROM NULL;
-- returns true
SELECT NULL IS NULL;
-- returns true
```

## NULL and Functions

A function that has input argument as `NULL` **usually** returns `NULL`.

```sql
SELECT cos(NULL);
-- NULL
```

The `coalesce` function is an exception to this: it takes any number of arguments, and returns for each row the first argument that is not `NULL`. If all arguments are `NULL`, `coalesce` also returns `NULL`.

```sql
SELECT coalesce(NULL, NULL, 1);
-- 1
SELECT coalesce(10, 20);
-- 10
SELECT coalesce(NULL, NULL);
-- NULL
```

The `ifnull` function is a two-argument version of `coalesce`.

```sql
SELECT ifnull(NULL, 'default_string');
-- default_string
SELECT ifnull(1, 'default_string');
-- 1
```

## NULL and Conjunctions

`NULL` values have special semantics in `AND`/`OR` conjunctions. For the ternary logic truth tables, see the [Boolean Type documentation](../../sql/data_types/boolean).

## NULL and Aggregate Functions

`NULL` values are ignored in most aggregate functions. 

Aggregate functions that do not ignore `NULL` values include: `first`, `last`, `list`, and `array_agg`. To exclude `NULL` values from those aggregate functions, the [`FILTER` clause](../../sql/query_syntax/filter) can be used.

```sql
CREATE TABLE integers (i INTEGER);
INSERT INTO integers VALUES (1), (10), (NULL);

SELECT min(i) FROM integers;
-- 1

SELECT max(i) FROM integers;
-- 10
```
