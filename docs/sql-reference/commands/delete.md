---
layout: default
title: DELETE
nav_exclude: true
search_exclude: true
description: Reference and syntax for the DELETE command.
parent: SQL commands
---

# DELETE (Alpha)
{: .no_toc}

Deletes rows from the specified table.

* Topic ToC
{:toc}

## Syntax

```sql
DELETE FROM <table_name> WHERE <condition>
```

| Parameter | Description|
| :---------| :----------|
| `<table_name>`| The table to delete rows from. |
| `<condition>` | A Boolean expression. Only rows for which this expression returns `true` will be deleted. Condition can have subqueries doing semijoin with other table(s). |

{: .note}
The `DELETE FROM <table_name>` without `<condition>` will delete *all* rows from the table. It is equivalent to `TRUNCATE TABLE` statement.

### Example with simple WHERE clause

`DELETE FROM product WHERE price = 0`

Table before

```
product
+------------+--------+
| name       | price  |
+---------------------+
| wand       |    125 |
| broomstick |    270 |
| bludger    |      0 |
| robe       |     80 |
| cauldron   |     25 |
| quaffle    |      0 |
+------------+--------+
```

Table after

```
product
+------------+--------+
| name       | price  |
+---------------------+
| wand       |    125 |
| broomstick |    270 |
| robe       |     80 |
| cauldron   |     25 |
+------------+--------+
```

### Example with subqueries

```sql
DELETE FROM product WHERE 
  name IN (SELECT name FROM inventory WHERE amount = 0) OR
  name NOT IN (SELECT name FROM inventory)
```

Tables before

```
product
+------------+--------+
| name       | price  |
+---------------------+
| wand       |    125 |
| broomstick |    270 |
| robe       |     80 |
| cauldron   |     25 |
+------------+--------+

inventory
+------------+--------+
| name       | amount |
+---------------------+
| wand       |      3 |
| cauldron   |     18 |
| robe       |      0 |
| bludger    |      5 |
+------------+--------+
```

Tables after
```
product
+------------+--------+
| name       | price  |
+---------------------+
| wand       |    125 |
| cauldron   |     25 |
+------------+--------+
```