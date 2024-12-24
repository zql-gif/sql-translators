* 找几个基于规则的SQL Translation的baselines，2-3个。
* 为啥找基于规则的，话说跟llm-based的方法也没对比吧？
# 1. 论文与综述
[Mallet: SQL Dialect Translation with LLM Rule Generation | Proceedings of the Seventh International Workshop on Exploiting Artificial Intelligence Techniques for Data Management](https://dl.acm.org/doi/10.1145/3663742.3663973)

这篇论文中提到：
Existing approaches rely on hand-crafted translation rules, which tend to be incomplete and hard to maintain, especially as the number of dialects to translate increases. 
**Though several tools exist to make migrations easier, they all use carefully hand-written rules, which is a tedious approach. They may work well for a specific set of systems [1], but they do not scale well with the number of systems, which explains why all the tools that aim to support a larger number of systems fail on basic queries for some pairs of systems [4, 5, 8, 12].**

[1] L. Antova, D. Bryant, T. Cao, M. Duller, M. A. Soliman, and F. M. Waas. Rapid adoption of cloud data warehouse technology using datometry hyper-q. In Pro ceedings of the 2018 International Conference on Management of Data, SIGMOD ’18, page 825–839, New York, NY, USA, 2018. Association for Computing Machinery. 
[4] Datometry. DATOMETRYOPENDBFORPOSTGRESQL. https://airlift.datometry. com/opendb-for-postrgresql/.
[5] JOOQ. SQL Translation. https://www.jooq.org/translate/.
[8] T. Mao. Python SQL Parser and Transpiler. https://github.com/tobymao/sqlglot, 2024. 
[12] SQLines. Data and Analytics Platform Migration. https://www.sqlines.com/. 

Datometry: https://chatgpt.com/share/676a4f41-0930-8002-ab09-b09fc318d80e
# 2. 工具
## 2.1 [JOOQ. SQL Translation ](https://www.jooq.org/translate/)
### Links
* 在线translate：[Format, pretty print, and translate your SQL from one dialect to another](https://www.jooq.org/translate/)
* 官网：[jOOQ manuals, tutorials, FAQs, references](https://www.jooq.org/learn/)
* sql translator原理简单解释：[Using jOOQ's parser as a SQL translator](https://www.jooq.org/doc/latest/manual/sql-building/sql-parser/sql-parser-translator/)
### Supported Databases
MySQL 
MariaDB
TiDB : 0
PostgreSQL
SQLite
MonetDB : 0
DuckDB 
ClickHouse
### 原理
The input dialect is a mixture of all of jOOQ's currently supported SQL dialects. [The grammar can be found here](https://www.jooq.org/doc/latest/manual/sql-building/sql-parser/sql-parser-grammar/). 
The [SQL parser](https://www.jooq.org/doc/latest/manual/sql-building/sql-parser/ "jOOQ Manual reference: SQL Parser") can be used as a SQL translator. The fact that translation happens is **just an emerging feature of combining**:
* The parser API to parse a SQL string into the jOOQ [query object model API](https://www.jooq.org/doc/latest/manual/sql-building/model-api/ "jOOQ Manual reference: The model API")
- Using jOOQ to render the query object model again into a SQL string.
  The simplest example is this one: System.out.println(create.parser().parse("SELECT 1").toString());
This will parse the generic SQL string `SELECT 1`, and render it according to the configured [Configuration](https://www.jooq.org/doc/latest/manual/sql-building/dsl-context/ "jOOQ Manual reference: The DSLContext API"), [SQLDialect](https://www.jooq.org/doc/latest/manual/sql-building/dsl-context/sql-dialects/ "jOOQ Manual reference: SQL Dialect"), [Settings](https://www.jooq.org/doc/latest/manual/sql-building/dsl-context/custom-settings/ "jOOQ Manual reference: Custom Settings"), etc.

GPT解释：https://chatgpt.com/share/676a6e08-8828-8002-8dc3-abebc5f3909b
方言例子：[The BIT_LENGTH function](https://www.jooq.org/doc/latest/manual/sql-building/column-expressions/string-functions/bit-length-function/)

### Chracteristics
* JOOQ 是基于Java访问关系型数据库的工具包，可以轻松的使用Java面向对象语法来实现各种复杂的sql。
* rule-based
* 支持的db较丰富
* 扩展到新的db上需要为所有新增features添加新的dialect rules
## 2.2 [SqlGlot-Python SQL Parser and Transpiler.](https://github.com/tobymao/sqlglot)
### Links
[SQLGlot重要链接](SQLGlot/重要链接.md)
### Supported Databases
MySQL 
MariaDB : 0
TiDB : 0
PostgreSQL
SQLite
MonetDB : 0
DuckDB 
ClickHouse
### Chracteristics
* SQLGlot is a no-dependency SQL parser, transpiler, optimizer, and engine. It can be used to format SQL or translate between [24 different dialects](https://github.com/tobymao/sqlglot/blob/main/sqlglot/dialects/__init__.py) like [DuckDB](https://duckdb.org/), [Presto](https://prestodb.io/) / [Trino](https://trino.io/), [Spark](https://spark.apache.org/) / [Databricks](https://www.databricks.com/), [Snowflake](https://www.snowflake.com/en/), and [BigQuery](https://cloud.google.com/bigquery/). It aims to read a wide variety of SQL inputs and output syntactically and semantically correct SQL in the targeted dialects.
* python库
* rule-based
* 支持的db较丰富
* 支持用户定制dialects：[sqlglot.dialects API documentation](https://sqlglot.com/sqlglot/dialects.html#dialects), https://chatgpt.com/share/676a732d-d138-8002-8ed6-79d41c32cfaa

## 2.3 [SQL Translator by Datafold](https://sqltranslator.io/)

### Links
* 在线translate：[SQL Translator](https://sqltranslator.io/)
### Supported Databases
MySQL 
MariaDB : 0 
TiDB : 0
PostgreSQL 
SQLite
MonetDB : 0
DuckDB 
ClickHouse
### Chracteristics
* sql translate：AL-powered, llm-based
* 支持的db较丰富
* https://docs.datafold.com/welcome?share_chat=b5d8d0e2-0b5f-40f9-8e6e-5e78313ca108

## 2.4  [Translate SQL queries (dialects) - dbt Power User](https://docs.myaltimate.com/develop/translateSQL/)
### Links
介绍：[Translate SQL queries (dialects) - dbt Power User](https://docs.myaltimate.com/develop/translateSQL/)
### Supported Databases
MySQL 
MariaDB : 0
TiDB : 0
PostgreSQL 
SQLite
MonetDB : 0
DuckDB 
ClickHouse
### Chracteristics
* Power user VSCode extension is open source ([code repo](https://github.com/AltimateAI/vscode-dbt-power-user)), and it's listed on the [VSCode Marketplace](https://marketplace.visualstudio.com/items?itemName=innoverio.vscode-dbt-power-user).
* rule-based
* limitations（官方给出的总结）
	* If there are functions we can't identify, then we will not be able to convert it. In those scenario, it will be kept as is.
	- We do not look at data types. If there are some data types which are not supported in the target database, we may not be able translate those 

---
## 2.5 [SQLines. Data and Analytics Platform Migration.](https://www.sqlines.com/)
### Links
* github链接：[dmtolpeko/sqlines: SQLines Open Source Database Migration Tools](https://github.com/dmtolpeko/sqlines)
* 在线translate：[SQLines - Online SQL Conversion - SQL Scripts, DDL, Queries, Views, Stored Procedures, Triggers](https://www.sqlines.com/online)
### Supported Databases
Microsoft SQL Server, Oracle, MariaDB, MySQL, PostgreSQL, IBM DB2, Sybase, Informix, Teradata, Greenplum and Netezza.

MySQL 
MariaDB 
TiDB : 0
PostgreSQL
SQLite : 0
MonetDB : 0
DuckDB : 0
ClickHouse : 0
### Chracteristics
* SQLines SQL Converter is an open source tool (Apache License 2.0) that allows you to convert database schema (DDL), queries and DML statements, views, stored procedures, packages, functions and triggers between Microsoft SQL Server, Oracle, MariaDB, MySQL, PostgreSQL, IBM DB2, Sybase, Informix, Teradata, Greenplum and Netezza.
* rule-based: 官网给出了migration rules

## 2.6 [SPL: Translating SQL across databases](https://blog.scudata.com/spl-translating-sql-across-databases/)

### Links
介绍：[SPL: Translating SQL across databases - esProc SPL Official Blog - esProc SPL Official Blog](https://blog.scudata.com/spl-translating-sql-across-databases/)
sqltranslate function（call with java）：[sqltranslate()-Functions | esProc Function Reference](https://doc.scudata.com/esproc/func/sqltranslate.html)
### Supported Databases
 support：ORACLE, SQLSVR, DB2, MYSQL, HSQL, TERADATA and POSTGRES

MySQL
MariaDB : 0
TiDB : 0
PostgreSQL 
SQLite : 0
MonetDB : 0
DuckDB  : 0
ClickHouse : 0
### Chracteristics
* rule-based
* 支持用户定义dialects：Users are allowed to define user-defined database types and functions by modifying the configuration file.
* 据sqltranslation function的api doc显示，支持的函数相当有限


# 其他
* oracle官方：[SQL Translation and Migration Guide](https://docs.oracle.com/en/database/oracle/oracle-database/21/drdaa/sql-translation-and-migration-guide.pdf)
* [About | SQL Tran Docs](https://docs.sqltran.com/): 支持的db类型太少而且和我们的工具不太重合
