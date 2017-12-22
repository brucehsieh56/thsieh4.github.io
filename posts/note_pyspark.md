---
layout: default
title: Notes on PySpark
permalink: /note_pyspark
---

<h2 style="text-align: center;">Notes on PySpark</h2>

### Basic Command
- `sparkSession`
	- `.builder.getOrCreate()`
	- `.catalog.listTables()` | examine the tables in the catalog.
	- `.sql()` | execute sql statement.
	- `.toPandas()` | create a pandas DataFrame from a Spark DataFrame. 
	- `.createDataFrame()` | create a Spark DataFrame from a pandas DataFrame.
	- `.read.csv(file_path, header=True)` | read `csv` data as a Spark DataFrame

