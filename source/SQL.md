# 2. SQL

SQL(Structured Query Language，结构化查询语言)是访问和操作关系数据库的标准语言，是高级的非过程化编程语言。只要是关系数据库，都可以使用 SQL 进行访问和控制。



## 2.1 SQL发展历史

- 在1970年代初，由IBM公司San Jose,California研究实验室的埃德加·科德发表将数据组成表格的应用原则（Codd's Relational Algebra）。1974年，同一实验室的D.D.Chamberlin和R.F. Boyce对Codd's Relational Algebra在研制关系数据库管理系统System R中，研制出一套规范语言-SEQUEL(Structured English QUEry Language)，并在1976年11月的IBM Journal of R&D上公布新版本的SQL（叫SEQUEL/2）。1980年改名为SQL。
- 1979年ORACLE公司首先提供商用的SQL，IBM公司在DB2 和SQL/DS数据库系统中也实现了SQL。
- 1986年10月，美国ANSI采用SQL作为关系数据库管理系统的标准语言（ANSI X3. 135-1986），后为国际标准化组织（ISO）采纳为国际标准。
- 1989年，美国ANSI采纳在ANSI X3.135-1989报告中定义的关系数据库管理系统的SQL标准语言，称为ANSI SQL 89， 该标准替代ANSI X3.135-1986版本。该标准为下列组织所采纳：
- 国际标准化组织（ISO），为ISO 9075-1989报告“Database Language SQL With Integrity Enhancement”
- 美国联邦政府，发布在The Federal Information Processing Standard Publication(FIPS PUB)127
- 目前，所有主要的关系数据库管理系统支持某些形式的SQL， 大部分数据库打算遵守ANSI SQL89标准。

## 2.2 SQL特点

- SQL语言集数据查询、数据操纵、数据定义和数据控制功能于一体
- 面向集合的语言
- 非过程语言
- 类似自然语言，简洁易用
- 自含式语言，又是嵌入式语言。可独立使用，也可嵌入到宿主语言中。

## 2.3 SQL基本语句

- DQL（data query language），数据查询语言；也就是 SELECT 语句，用于查询数据库中的数据和信息
- DML（data manipulation language），数据操作语言；用于对表中的数据进行增加（INSERT）、修改（UPDATE）、删除（DELETE）以及合并（MERGE）操作
- DDL（data definition language），数据定义语言；主要用于定义数据库中的对象（例如表或索引），包括创建对象（CREATE）、修改对象（ALTER）和删除对象（DROP）等
- TCL（transaction control language），事务控制语言；用于管理数据库的事务，主要包括启动一个事务（BEGIN TRANSACTION）、提交事务（COMMIT）、回退事务（ROLLBACK）和事务保存点（SAVEPOINT）
- DCL（data control language），数据控制语言；用于控制数据的访问权限，主要有授权（GRANT）和撤销（REVOKE）。



## 2.4 NoSQL

随着互联网的发展和大数据的兴起，出现了各种各样的非关系（NoSQL）数据库。NoSQL 代表 Not only SQL，表明它是针对传统关系数据库的补充和升级，而不是为了替代关系数据库。

NoSQL 数据库主要用于解决关系数据库在某些特定场景下的局限性，比如海量存储和水平扩展；但同时也会为此牺牲某些关系数据库的特性，例如对事务强一致性的支持和标准 SQL 接口。因此，这类数据库主要用于对一致性要求不是非常严格的互联网业务。常见的 NoSQL 数据库可以分为以下几类：

- 文档数据库，例如 MongoDB（MongoDB 4.0 增加了多文档事务的特性）
- 键值存储，例如 Redis
- 全文搜索引擎，例如 Elasticsearch
- 宽列存储数据库，例如 Cassandra
- 图形数据库，例如 Neo4J。

另一方面，关系数据库也在积极拥抱变化，添加了许多非关系模型（XML 和 JSON）支持。以最流行的开源关系数据库 MySQL 为例，最新的 MySQL 8.0 版本增加了 JSON 文档存储的支持，并且推出了一个新的概念：NoSQL + SQL = MySQL。

Oracle、SQL Server 以及 PostgreSQL 同样也进行了类似的扩展，可以支持原生的 XML 和 JSON 数据，并且提供了许多标准的 SQL 接口。

## 2.5 NewSQL

为了同时获得关系数据库对于事务的支持和标准的 SQL 接口，以及非关系数据库的高度扩展性和高性能。如今市场上已经出现了一类新型关系型数据库系统：NewSQL 数据库。

比较有代表性的 NewSQL 数据库包括 Google Spanner、VoltDB、PostgreSQL-XL 以及国产的 TiDB。这类新型数据库是数据库领域最新的发展方向。