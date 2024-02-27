# Optimisation Guide

## Database design

Where possible Dimensional Modeling [The Data Warehouse Toolkit - Kimball Ross] should be used.

The key concepts are

***
### Profiling queries

Freequent use of NewRelic will aid in finding slow queries, additionally the slow query log can be used. If a query appears to be complex, multiple left joins etc... the SQL can be manually ran via ```phpMyAdmin``` or ```MySQL Workbench``` to see execution times and how better to optimise the query.

***
### High table width / column count

The width of a table does not impact it's perforamnce in queries.

***
### Caching parent ID's

Where possible, cache parent ID's to as many levels is necessary. I.e a Campus Tool should contain it's parent Task ID, the Tasks parent Unit ID and the parent Units Course ID.

| ID | TaskID | UnitID | CourseID |
|----|--------|--------|----------|

This allows each to be joined directly from the record that forms the starting point of the query

***
### Indexing

If storing an ID within a column, such as TaskID, UnitID, CourseID, this ID number should be indexed as it is highly likely that it will be used for joining and / or where commands.

A good rule to use is to add an index on columns that either:

* Store unique identifiers of another object
* Are used for joining tables (these are usually unique identifiers anyway)
* Are frequently used for searching, I.e. a ```deleted``` or ```archived``` column, NB text fields or long strings are not performant to index, especailly where ```LIKE``` is used in the search

***
### Date columns

There are multiple ways to store dates in a database, some are more performant than others depending on their use.

***
#### Dates for ordering

A simple date field can be used, or a timestamp filed. A timestamp field will be more efficient than a date field, although this can be mostly mitigated with using the MySQL DATE function ```ORDER BY DATE(date_column)```

| DateTime           |
|--------------------|
|2024-01-01 12:00:00 |

***
#### Dates for searching

If a date field is to be regularly used with ```LIKE```, i.e. 

```
SELECT *
FROM wp_database
WHERE date LIKE '2024-01-01%';
```

It can be more performant to split the date into separated columns

| Year | Month | Day | Hour | Minute | Second |
|------|-------|-----|------|--------|--------|
| 2024 | 01    | 01  | 12   | 00     | 00     |

Each column can then be indexed as an integer with up to 60 unique values per column (excluding the year), the following query can then be used:

```
SELECT *
FROM wp_database
WHERE Year = 2024 AND Month = 01 AND Day = 01
```

Although 3 columns are used for ```WHERE```, if these columns are indexed, search is considerably faster. Further improvements can be made by indexing the desired fields as a single index I.e. if searches regularly use Year, Month and Day 

```
CREATE INDEX ymd
ON wp_database (Year, Month, Day)
```

***
### High number of joins

It is possible to have a large number of joins in a query and it remain performant, what is important is the level of joins. I.e. using a joined table to form another join

```
SELECT * FROM
wp_camline_tools AS t
LEFT JOIN wp_camline_tasks AS ta ON (t.ParentID = ta.ID)
LEFT JOIN wp_camline_units AS u ON (ta.ParentID = u.ID)
LEFT JOIN wp_camline_courses AS c ON (u.ParentID = c.ID)
WHERE t.ID = 1;
```

will be considerably slower (especially in larger databases) than

```
SELECT * FROM
wp_camline_tools AS t
LEFT JOIN wp_camline_tasks AS ta ON (t.TaskID = ta.ID)
LEFT JOIN wp_camline_units AS u ON (t.UnitID = u.ID)
LEFT JOIN wp_camline_courses AS c ON (t.CourseID = c.ID)
WHERE t.ID = 1;
```

Generally if a table has a one to one relation to another table, the ID should be stored in both tables and if a table has a one to many relation to another table, the table with a relation of one should store the other's ID. This will allow efficient joining of tables.

Using this method, a large number of table joins can be included in a query with very little impact on performance.