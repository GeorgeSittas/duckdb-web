---
layout: docu
title: PostgreSQL Import
---

To run a query directly on a running PostgreSQL database, the [`postgres` extension](../../extensions/postgres_scanner) is required.  This can be installed use the `INSTALL` SQL command. This only needs to be run once.

```sql
INSTALL postgres;
```

To load the `postgres` extension for usage, use the `LOAD` SQL command:

```sql
LOAD postgres;
```

After the `postgres` extension is installed, tables can be queried from PostgreSQL using the `postgres_scan` function:

```sql
-- scan the table "mytable" from the schema "public" using the empty (default) connection string
SELECT * FROM postgres_scan('', 'public', 'mytable');
```

The first parameter to the `postgres_scan` function is the [PostgreSQL connection string](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING).

Alternatively, the entire file can be attached using the `postgres_attach` command. This creates views over all of the tables in the PostgreSQL database that allow you to query the tables using regular SQL syntax.

```sql
-- attach the Postgres database using the given connection string
CALL postgres_attach('');
-- the table "tbl_name" can now be queried as if it is a regular table
SELECT * FROM tbl_name;
```

For more information see the [PostgreSQL scanner documentation](../../extensions/postgres_scanner).
