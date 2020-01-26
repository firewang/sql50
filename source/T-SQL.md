# 2 Transact-SQL

[Transact-SQL](https://baike.baidu.com/item/Transact-SQL/2756623?fr=aladdin)（又称 T-SQL），是在 Microsoft SQL Server 和 Sybase SQL Server 上的 ANSI SQL 实现，与 Oracle 的 PL/SQL 性质相近（不只是实现 ANSI SQL，也为自身数据库系统的特性提供实现支持），在 Microsoft SQL Server 和 Sybase Adaptive Server 中仍然被使用为核心的查询语言。

[官方文档](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms189312(v=sql.105))

## 2.1 Transact-SQL 元素

| Transact-SQL 元素                                            | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [标识符](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms175874(v%3dsql.105)) | 表、视图、列、数据库和服务器等对象的名称。                   |
| [数据类型](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms187594(v%3dsql.105)) | 定义数据对象（如列、变量和参数）所包含的数据的类型。大多数 Transact-SQL 语句并不显式引用数据类型，但它们的结果受语句中所引用对象的数据类型之间的交互操作影响。 |
| [常量](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190955(v%3dsql.105)) | 代表特定数据类型的符号。                                     |
| [函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190642(v%3dsql.105)) | 语法元素，可以接受零个、一个或多个输入值，并返回一个标量值或表格形式的一组值。示例包括将多个值相加的 SUM 函数、确定两个日期之间相差多少个时间单位的 DATEDIFF 函数、获取 Microsoft SQL Server 实例名称的 @@SERVERNAME 函数或在远程服务器上执行 Transact-SQL 语句并检索结果集的 OPENQUERY 函数。 |
| [表达式](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190718(v%3dsql.105)) | SQL Server 可以解析为单个值的语法单位。表达式的示例包括常量、返回单值的函数、列或变量的引用。 |
| [表达式中的运算符](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms189123(v%3dsql.105)) | 与一个或多个简单表达式一起使用，构造一个更为复杂的表达式。例如，表达式 PriceColumn * 1.1 中的乘号 (*) 使价格提高百分之十。 |
| [注释](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms188621(v%3dsql.105)) | 插入到 Transact-SQL 语句或脚本中、用于解释语句作用的文本段。SQL Server 不执行注释。 |
| [保留关键字](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190011(v%3dsql.105)) | 保留下来供 SQL Server 使用的词，不应用作数据库中的对象名。   |



## 2.2 Transact-SQL标识符

Microsoft SQL Server 中的所有内容都可以有标识符。服务器、数据库和数据库对象（例如表、视图、列、索引、触发器、过程、约束及规则等）都可以有标识符。大多数对象要求有标识符，但对有些对象（例如约束），标识符是可选的。

### 2.2.1 标识符的种类

有两类标识符：

- 常规标识符
  符合标识符的格式规则。在 Transact-SQL 语句中使用常规标识符时不用将其分隔开。

  ```
  SELECT *
  FROM TableX
  WHERE KeyCol = 1024
  ```

- 分隔标识符
  包含在双引号 (") 或者方括号 ([ ]) 内。不分隔符合标识符格式规则的标识符。例如：

  ```
  SELECT *
  FROM [TableX]          --用不用分隔符都可以
  WHERE [KeyCol] = 1024  --用不用分隔符都可以
  ```

  在 Transact-SQL 语句中，必须对不符合所有标识符规则的标识符进行分隔。例如：

  ```
  SELECT *
  FROM [My Table]      --My Table之间包含空格，因此必须包含分隔标识符
  WHERE [order] = 10   --关键字必须包含分隔符
  ```

常规标识符和分隔标识符包含的字符数必须在 1 到 128 之间。对于本地临时表，标识符最多可以有 116 个字符。



### 2.2.2 常规标识符规则

常规标识符格式规则取决于数据库兼容级别。该级别可以使用 [ALTER DATABASE](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/bb510680(v%3dsql.105)) 设置。当兼容级别为 **100** 时，下列规则适用：

1. 第一个字符必须是下列字符之一：

   - Unicode 标准 3.2 所定义的字母。Unicode 中定义的字母包括拉丁字符 a-z 和 A-Z，以及来自其他语言的字母字符。

   - 下划线 (_)、at 符号 (@) 或数字符号 (#)。

     在 SQL Server 中，某些位于标识符开头位置的符号具有特殊意义。以 at 符号开头的常规标识符始终表示局部变量或参数，并且不能用作任何其他类型的对象的名称。以一个数字符号开头的标识符表示临时表或过程。以两个数字符号 (##) 开头的标识符表示全局临时对象。虽然数字符号或两个数字符号字符可用作其他类型对象名的开头，但是不建议这样做。

     > 某些 Transact-SQL 函数的名称以两个 at 符号 (@@) 开头。为了避免与这些函数混淆，不应使用以 @@ 开头的名称。

2. 后续字符可以包括：

   - 如 Unicode 标准 3.2 中所定义的字母。
   - 基本拉丁字符或其他国家/地区字符中的十进制数字。
   - at 符号、美元符号 ($)、数字符号或下划线。

3. 标识符一定不能是 Transact-SQL 保留字。SQL Server 可以保留大写形式和小写形式的保留字。

4. 不允许嵌入空格或其他特殊字符。

5. 不允许使用增补字符。

在 Transact-SQL 语句中使用标识符时，不符合这些规则的标识符必须由**双引号或括号**分隔。



## 2.3 Transact-SQL 数据类型

包含数据的对象都有一个相关联的数据类型，它定义对象所能包含的数据种类，例如字符、整数或二进制。下列对象具有数据类型：

- 表和视图中的列。
- 存储过程中的参数。
- 变量。
- 返回一个或多个特定数据类型数据值的 Transact-SQL 函数。
- 具有返回代码（始终为 integer 数据类型）的存储过程。

为对象分配数据类型时可以为对象定义四个属性：

- 对象包含的数据种类。
- 所存储值的长度或大小。
- 数值的精度（仅适用于数字数据类型）。
- 数值的小数位数（仅适用于数字数据类型）。



### 2.3.1  二进制数据

[binary 和 varbinary](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms188362(v%3dsql.105)) 数据类型存储位串。尽管字符数据是根据 SQL Server 代码页进行解释的，但 binary 和 varbinary 数据仅是位流。

- binary [ ( n ) ]
  长度为 n 字节的固定长度二进制数据，其中 n 是从 1 到 8,000 的值。存储大小为 n 字节。
- varbinary [ ( n | max) ]
  可变长度二进制数据。n 可以是从 1 到 8000 之间的值。max 指示最大存储大小为 2^31-1 字节。存储大小为所输入数据的实际长度 + 2 个字节。所输入数据的长度可以是 0 字节。varbinary 的 ANSI SQL 同义词为 **binary varying**。



二进制常量以 0x（一个零和小写字母 x）开始，后跟位模式的十六进制表示形式。例如，0x2A 表示十六进制值 2A，它等于十进制值 42 或单字节位模式 00101010。

存储十六进制值 [如安全标识号 (SID)、GUID（使用 uniqueidentifier 数据类型）或可以用十六进制方式存储的复杂数字时，使用二进制数据。



### 2.3.2 字符串

[char 和 varchar 数据类型](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms175055(v=sql.105))存储由以下字符组成的数据：

- 大写字符或小写字符。例如，a、b 和 C。
- 数字。例如，1、2 和 3。
- 特殊字符。例如，at 符号 (@)、“与”符号 (&) 和感叹号 (!)。

[使用方式](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms176089(v=sql.105))：

- char [ ( n ) ]
  固定长度，非 Unicode 字符串数据。n 定义字符串长度，取值范围为 1 至 8,000。存储大小为 n 字节。当排序规则代码页使用双字节字符时，存储大小仍然为 n 个字节。根据字符串的不同，n 个字节的存储大小可能小于为 n 指定的值。char 的 ISO 同义词为 character。
- varchar [ ( n | max ) ]
  可变长度，非 Unicode 字符串数据。n 定义字符串长度，取值范围为 1 至 8,000。max 指示最大存储大小是 2^31-1 个字节 (2 GB)。存储大小为输入的实际数据长度 + 2 个字节。varchar 的 ISO 同义词为 char varying 或 character varying。

varchar 数据可以有两种形式。varchar 数据的最大字符长度可以是指定的。例如，varchar(6) 指示此数据类型最多存储六位字符；它也可以是 varchar(max), 形式的，即此数据类型可存储的最大字符数可达 2^31。

每个 char 和 varchar 数据值都具有排序规则。排序规则定义属性，如用于表示每个字符的位模式、比较规则以及是否区分大小写或重音。每个数据库有默认排序规则。当定义列或指定常量时，除非使用 COLLATE 子句指派特定的排序规则，否则将为它们指派数据库的默认排序规则。当组合或比较两个具有不同排序规则的 char 或 varchar 值时，根据排序规则的优先规则来确定操作所使用的排序规则。

字符常量必须包括在单引号 (') 或双引号 (") 中。建议用单引号括住字符常量。



### 2.3.3 Unicode 字符串

Unicode 规格为全球商业领域中广泛使用的大部分字符定义了一个单一编码方案。所有的计算机都用单一的 Unicode 规格将 Unicode 数据中的位模式一致地转换成字符。这保证了同一个位模式在所有的计算机上总是转换成同一个字符。数据可以随意地从一个数据库或计算机传送到另一个数据库或计算机，而不用担心接收系统是否会错误地转换位模式。

每个 Microsoft SQL Server 排序规则都有一个代码页，该代码页定义表示 char、varchar 和 text 值中每个字符的位模式。可为个别的列和字符常量分配不同的代码页。

Unicode 规格通过采用两个字节编码每个字符使这个问题迎刃而解。转换最通用商业语言的单一规格具有足够多的 2 字节的模式 (65536)。因为所有的 Unicode 系统均一致地采用同样的位模式来表示所有的字符，所以当从一个系统转到另一个系统时，将不会存在未正确转换字符的问题。通过在整个系统中使用 Unicode 数据类型，可尽量减少字符转换问题。

在 SQL Server 中，下列数据类型支持 Unicode 数据：

- nchar
- nvarchar
- ntext



字符串数据类型（nchar 长度固定或 nvarchar 长度可变）和 Unicode 数据使用 UNICODE UCS-2 字符集。

- nchar [ ( n ) ]
  固定长度，Unicode 字符串数据。n 定义字符串长度，取值范围为 1 至 4,000。存储大小为 n 字节的两倍。当排序规则代码页使用双字节字符时，存储大小仍然为 n 个字节。根据字符串的不同，n 个字节的存储大小可能小于为 n 指定的值。nchar 的 ISO 同义词为 national char 和 national character。
- nvarchar [ ( n | max ) ]
  可变长度，Unicode 字符串数据。n 定义字符串长度，取值范围为 1 至 4,000。max 指示最大存储大小是 2^31-1 个字节 (2 GB)。存储大小（以字节为单位）是所输入数据实际长度的两倍 + 2 个字节。nvarchar 的 ISO 同义词为 national char varying 和 national character varying。



除下列情况外，nchar、nvarchar 和 ntext 的使用分别与 char、varchar 和 text 的使用相同：

- Unicode 支持更大范围的字符。
- 存储 Unicode 字符需要更大的空间。
- nchar 列的最大大小为 4,000 个字符，与 char 和 varchar 不同，它们为 8,000 个字符。
- 使用最大说明符，nvarchar 列的最大大小为 2^31-1 字节。
- Unicode 常量以 N 开头指定：N'A Unicode string'。
- 所有 Unicode 数据使用由 Unicode 标准定义的字符集。用于 Unicode 列的 Unicode 排序规则以下列属性为基础：区分大小写、区分重音、区分假名、区分全半角和二进制。



### 2.3.4 Text和Image

Microsoft SQL Server 将超过 8,000 个字节的字符串和大于 8,000 个字节的二进制数据分别存储为名为 text 和 image 的特殊数据类型。超过 4,000 个字符的 Unicode 字符串存储为 ntext 数据类型。

例如，您需要将一个大型客户信息文本文件 (.txt) 导入 SQL Server 数据库。应将这些数据作为一个数据块存储起来，而不是集成到数据表的多个列中。为此，可以创建一个 text 数据类型的列。但是，如果必须存储公司徽标，它们当前存储为标记图像文件格式 (TIFF) 图像 (.tif) 且每个图像的大小为 10 KB，则可以创建一个 image 数据类型的列。



### 2.3.5  整数

| [数据类型](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms187745(v%3dsql.105)) | 范围                                                         | 存储   |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----- |
| bigint                                                       | -2^63 (-9,223,372,036,854,775,808) 到 2^63-1 (9,223,372,036,854,775,807) | 8 字节 |
| int                                                          | -2^31 (-2,147,483,648) 到 2^31-1 (2,147,483,647)             | 4 字节 |
| smallint                                                     | -2^15 (-32,768) 到 2^15-1 (32,767)                           | 2 字节 |
| tinyint                                                      | 0 到 255                                                     | 1 字节 |

在数据类型优先次序表中，bigint 介于 smallmoney 和 int 之间。

尽管 SQL Server 有时会将 tinyint 或 smallint 值提升为 int 数据类型，但不会自动将 tinyint、smallint 或 int 值提升为 bigint 数据类型。除非明确说明，否则那些接受 int 表达式作为其参数的函数、语句和系统存储过程都不会改变，从而不会支持将 bigint 表达式隐式转换为这些参数，只有当参数表达式为 [bigint 数据类型](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190689(v=sql.105))时，函数才返回 bigint。



### 2.3.6 decimal、numeric、float和real

[精度](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190476%28v%3dsql.105%29)是数字中的数字个数。小数位数是数中小数点右边的数字个数。例如，数 123.45 的精度是 5，小数位数是 2。

decimal 数据类型最多可以存储 38 个数字，所有这些数字均可位于小数点后面。decimal 数据类型存储精确的数字表示形式，存储值没有近似值。

定义 decimal 列、变量和参数的两种属性为：

- p

  指定精度或对象能够支持的数字个数。

- s

  指定可以放在小数点右边的小数位数或数字个数。

  p 和 s 必须遵守规则：0 <= s <= p <= 38。



[带固定精度和小数位数的数值数据类型](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms187746(v=sql.105))。

- decimal[ **(**p[ **,**s] **)**] 和 numeric[ **(**p[ **,**s] **)**]
  固定精度和小数位数。使用最大精度时，有效值从 - 10^38 +1 到 10^38 - 1。decimal 的 ISO 同义词为 dec 和 dec(p, s)。numeric 在功能上等价于 decimal。

- p（精度）
  最多可以存储的十进制数字的总位数，包括小数点左边和右边的位数。该精度必须是从 1 到最大精度 38 之间的值。默认精度为 18。

- s （小数位数）
  小数点右边可以存储的十进制数字的最大位数。小数位数必须是从 0 到 p 之间的值。仅在指定精度后才可以指定小数位数。默认的小数位数为 0；因此，0 <= s <= p。最大存储大小基于精度而变化。

  

  | 精度  | 存储字节数 |
  | :---- | :--------- |
  | 1 - 9 | 5          |
  | 10-19 | 9          |
  | 20-28 | 13         |
  | 29-38 | 17         |



在 SQL Server 中，numeric 和 decimal 数据类型的默认最大精度为 38。在 SQL Server 早期版本中，默认最大精度为 28。numeric 的功能等同于 decimal 数据类型。



float 和 real 数据类型被称为近似数据类型。float 和 real 的使用遵循有关近似数值数据类型的 IEEE 754 规范。

| 数据类型 | 范围                                                         | 存储          |
| :------- | :----------------------------------------------------------- | :------------ |
| float    | -1.79E + 308 至 -2.23E - 308、0 以及 2.23E - 308 至 1.79E + 308 | 取决于 n 的值 |
| real     | -3.40E + 38 至 -1.18E - 38、0 以及 1.18E - 38 至 3.40E + 38  | 4 字节        |



近似数值数据类型并不存储为许多数字指定的精确值，它们只储存这些值的最近似值。在很多应用程序中，指定值与存储的近似值之间的微小差异并不明显。但有时这些差异也较明显。

在 WHERE 子句搜索条件（特别是 = 和 <> 运算符）中，应避免使用 float 列或 real 列。float 列和 real 列最好只限于 > 比较或 < 比较。

IEEE 754 规范提供四种舍入模式：舍入到最近、向上舍入、向下舍入以及舍入到零。Microsoft SQL Server 使用向上舍入。所有的数值都必须精确到确定的精度，但会产生微小的浮点值差异。因为浮点数字的二进制表示法可以采用很多合法舍入规则中的任意一条，因此我们不可能可靠地量化浮点值。



### 2.3.7 货币数据

Microsoft SQL Server 使用以下两种数据类型存储货币数据或货币值：money 和 smallmoney。这些数据类型可以使用下列任意一种货币符号。

![货币符号表，十六进制值](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/images/ms188688.money01%28zh-cn%2csql.105%29.gif)



代表[货币或货币值](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms179882(v=sql.105))的数据类型。

| 数据类型   | 范围                                                  | 存储   |
| :--------- | :---------------------------------------------------- | :----- |
| money      | -922,337,203,685,477.5808 到 922,337,203,685,477.5807 | 8 字节 |
| smallmoney | -214,748.3648 到 214,748.3647                         | 4 字节 |

money 和 smallmoney 数据类型精确到它们所代表的货币单位的万分之一。



### 2.3.8 日期和时间数据

下表列出了 Transact-SQL 的日期和时间数据类型。 

| 数据类型                                                     | 格式                                      | 范围                                                         | 精确度     | 存储使用字节 |
| :----------------------------------------------------------- | :---------------------------------------- | :----------------------------------------------------------- | :--------- | :----------- |
| [time](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/bb677243(v%3dsql.105)) | hh:mm:ss[.nnnnnnn]                        | 00:00:00.0000000 到 23:59:59.9999999                         | 100 纳秒   | 3 到 5       |
| [date](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/bb630352(v%3dsql.105)) | YYYY-MM-DD                                | 0001-01-01 到 9999-12-31                                     | 1 天       | 3            |
| [smalldatetime](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms182418(v%3dsql.105)) | YYYY-MM-DD hh:mm:ss                       | 1900-01-01 到 2079-06-06                                     | 1 分钟     | 4            |
| [datetime](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms187819(v%3dsql.105)) | YYYY-MM-DD hh:mm:ss[.nnn]                 | 1753-01-01 到 9999-12-31                                     | 0.00333 秒 | 8            |
| [datetime2](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/bb677335(v%3dsql.105)) | YYYY-MM-DD hh:mm:ss[.nnnnnnn]             | 0001-01-01 00:00:00.0000000 到 9999-12-31 23:59:59.9999999   | 100 纳秒   | 6 到 8       |
| [datetimeoffset](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/bb630289(v%3dsql.105)) | YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+\|-]hh:mm | 0001-01-01 00:00:00.0000000 到 9999-12-31 23:59:59.9999999（以 UTC 时间表示） | 100 纳秒   | 8 到 10      |

所有日期和时间数据类型都支持关系运算符（<、<=、>、>=、<>）、比较运算符（=、<、<=、>、>=、<>、!<、!>）以及逻辑运算符和布尔谓词（IS NULL、IS NOT NULL、IN、BETWEEN、EXISTS、NOT EXISTS 和 LIKE）。



### 2.3.9 数据类型转换

可以按以下方案转换数据类型：

- 当一个对象的数据移到另一个对象，或两个对象之间的数据进行比较或组合时，数据可能需要从一个对象的数据类型转换为另一个对象的数据类型。
- 将 Transact-SQL 结果列、返回代码或输出参数中的数据移到某个程序变量中时，必须将这些数据从 SQL Server 系统数据类型转换成该变量的数据类型。

可以隐式或显式转换数据类型：

- 隐式转换对用户不可见。

  SQL Server 会自动将数据从一种数据类型转换为另一种数据类型。例如，将 smallint 与 int 进行比较时，在比较之前 smallint 会被隐式转换为 int。请注意，查询优化器可能生成一个查询计划来在任意时间执行此转换。

- 显式转换使用 CAST 或 CONVERT 函数。

  如果希望 Transact-SQL 程序代码符合 ISO 标准，请使用 CAST 而不要使用 CONVERT。如果要利用 CONVERT 中的样式功能，请使用 CONVERT 而不要使用 CAST。



### 2.3.10 uniqueidentifier 

uniqueidentifier 数据类型可存储 16 字节的二进制值，其作用与全局唯一标识符 (GUID) 一样。GUID 是唯一的二进制数；世界上的任何两台计算机都不会生成重复的 GUID 值。GUID 主要用于在拥有多个节点、多台计算机的网络中，分配必须具有唯一性的标识符。

uniqueidentifier 列的 GUID 值通常通过下列方式之一获取：

- 在 Transact-SQL 语句、批处理或脚本中调用 NEWID 函数。
- 在应用程序代码中，调用返回 GUID 的应用程序 API 函数或方法。

Transact-SQL NEWID 函数以及应用程序 API 函数和方法用它们的网卡的标识号加上 CPU 时钟的唯一编号来生成新的 uniqueidentifier 值。每个网卡都有唯一的标识号。NEWID 返回的 uniqueidentifier 值是通过使用服务器上的网卡而生成的。应用程序 API 函数和方法返回的 uniqueidentifier 值是通过使用客户端中的网卡而生成的。

uniqueidentifier 值通常不定义为常量。您可以按下列方式指定 uniqueidentifier 常量：

- 字符串格式： '6F9619FF-8B86-D011-B42D-00C04FC964FF'
- 二进制格式： 0xff19966f868b11d0b42d00c04fc964ff



uniqueidentifier 数据类型具有下列缺点：

- 值长且难懂。这使用户难以正确键入它们，并且更难记住。
- 这些值是随机的，而且它们不支持任何使其对用户更有意义的模式。
- 也没有任何方式可以决定生成 uniqueidentifier 值的顺序。它们不适用于那些依赖递增的键值的现有应用程序。
- 当 uniqueidentifier 为 16 字节时，其数据类型比其他数据类型（例如 4 字节的整数）大。这意味着使用 uniqueidentifier 键生成索引的速度相对慢于使用 int 键生成索引的速度。



### 2.3.11 XML数据

可以创建 xml 数据类型的变量和列。xml 数据类型有自己的 [XML 数据类型方法](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190798(v%3dsql.105))。

| XML方法                                                      | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [query() 方法（xml 数据类型）](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms191474(v%3dsql.105)) | 说明如何使用 query() 方法查询 XML 实例。                     |
| [value() 方法（xml 数据类型）](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms178030(v%3dsql.105)) | 说明如何使用 value() 方法从 XML 实例中检索 SQL 类型的值。    |
| [exist() 方法（xml 数据类型）](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms189869(v%3dsql.105)) | 说明如何使用 exist() 方法确定查询是否返回非空结果。          |
| [modify() 方法（xml 数据类型）](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms187093(v%3dsql.105)) | 说明如何使用 modify() 方法指定 [XML Data Modification Language (XML DML)](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms177454(v%3dsql.105)) 语句以执行更新。 |
| [nodes() 方法（xml 数据类型）](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms188282(v%3dsql.105)) | 说明如何使用 nodes() 方法将 XML 拆分到多行中，从而将 XML 文档的组成部分传播到行集中。 |
| [在 XML 数据内部绑定关系数据](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms175174(v%3dsql.105)) | 说明如何在 XML 中绑定非 XML 数据。                           |
| [xml 数据类型方法的使用准则](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms175894(v%3dsql.105)) | 说明使用 xml 数据类型方法的指导原则。                        |



可以对 xml 数据类型的列和变量中存储的 XML 数据指定 [XQuery 语言](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms189075(v%3dsql.105))。



### 2.3.12 timestamp和rowversion

每个数据库都有一个计数器，当对数据库中包含 rowversion 列的表执行插入或更新操作时，该计数器值就会增加。此计数器是数据库行版本。这可以跟踪数据库内的相对时间，而不是时钟相关联的实际时间。一个表只能有一个 rowversion 列。

每次修改或插入包含 rowversion 列的行时，就会在 rowversion 列中插入经过增量的数据库行版本值。这一属性使 rowversion 列不适合作为键使用，尤其是不能作为主键使用。对行的任何更新都会更改行版本值，从而更改键值。如果该列属于主键，那么旧的键值将无效，进而引用该旧值的外键也将不再有效。如果该表在动态游标中引用，则所有更新均会更改游标中行的位置。如果该列属于索引键，则对数据行的所有更新还将导致索引更新。

timestamp 的数据类型为 rowversion 数据类型的同义词，并具有数据类型同义词的行为。在 DDL 语句，请尽量使用 rowversion 而不是 timestamp。



### 2.3.13 cursor

[cursor](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190498(v=sql.105))是变量或存储过程 OUTPUT 参数的一种数据类型，这些参数包含对游标的引用。使用 cursor数据类型创建的变量可以为空。

有些操作可以引用那些带有 **cursor** 数据类型的变量和参数，这些操作包括：

- DECLARE *@local_variable* 和 SET *@local_variable* 语句。
- OPEN、FETCH、CLOSE 及 DEALLOCATE 游标语句。
- 存储过程输出参数。
- CURSOR_STATUS 函数。
- **sp_cursor_list**、**sp_describe_cursor**、**sp_describe_cursor_tables** 以及 **sp_describe_cursor_columns** 系统存储过程。



### 2.3.14 table

[table](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms175010(v=sql.105)) 是一种特殊的数据类型，用于存储结果集以进行后续处理。主要用于临时存储一组作为表值函数的结果集返回的行。可将函数和变量声明为 table 类型。table 变量可用于函数、存储过程和批处理中。



### 2.3.15 sql_variant

[sql_variant](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms173829(v=sql.105))用于存储 SQL Server 支持的各种数据类型的值。sql_variant 可以用在列、参数、变量和用户定义函数的返回值中。sql_variant 使这些数据库对象能够支持其他数据类型的值。

最大长度可以是 8016 个字节。这包括基类型信息和基类型值。实际基类型值的最大长度是 8,000 个字节。



### 2.3.16 Transact-SQL 常量
[常量](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190955(v=sql.105))是表示特定数据值的符号。常量的格式取决于它所表示的值的数据类型。常量还称为字面量。

| 使用的常量            | 示例                                                         |
| :-------------------- | :----------------------------------------------------------- |
| 字符串                | 'O''Brien''The level for job_id: %d should be between %d and %d.' |
| Unicode 字符串        | N'Michl'                                                     |
| 二进制字符串常量      | 0x12Ef0x69048AEFDD010E                                       |
| bit 常量              | 0 或 1                                                       |
| datetime 常量         | 'April 15, 1998''04/15/98''14:30:24''04:24 PM'               |
| integer 常量          | 18942                                                        |
| decimal 常量          | 1894.12042.0                                                 |
| float 和 real 常量    | 101.5E50.5E-2                                                |
| money 常量            | $12$542023.14                                                |
| uniqueidentifier 常量 | 0xff19966f868b11d0b42d00c04fc964ff'6F9619FF-8B86-D011-B42D-00C04FC964FF' |



## 2.4 Transact-SQL 函数

SQL Server 提供了可用于执行特定操作的[内置函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190642(v=sql.105))。[具体内置函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms174318%28v%3dsql.105%29)

| 函数类别                                                     | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [聚合函数 (Transact-SQL)](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms173454(v%3dsql.105)) | 执行的操作是将多个值合并为一个值。如 COUNT、SUM、MIN 和 MAX。 |
| [配置函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms173823(v%3dsql.105)) | 是一种标量函数，可返回有关配置设置的信息。                   |
| [加密函数 (Transact-SQL)](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms173744(v%3dsql.105)) | 支持加密、解密、数字签名和数字签名验证。                     |
| [游标函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms186285(v%3dsql.105)) | 返回有关游标状态的信息。                                     |
| [日期和时间函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms180878(v%3dsql.105)) | 可以更改日期和时间的值。                                     |
| [数学函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms177516(v%3dsql.105)) | 执行三角、几何和其他数字运算。                               |
| [元数据函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms187812(v%3dsql.105)) | 返回数据库和数据库对象的属性信息。                           |
| [排名函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms189798(v%3dsql.105)) | 是一种非确定性函数，可以返回分区中每一行的排名值。           |
| [行集函数 (Transact-SQL)](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms187957(v%3dsql.105)) | 返回可在 Transact-SQL 语句中表引用所在位置使用的行集。       |
| [安全函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms186236(v%3dsql.105)) | 返回有关用户和角色的信息。                                   |
| [字符串函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms181984(v%3dsql.105)) | 可更改 char、varchar、nchar、nvarchar、binary 和 varbinary 的值。 |
| [系统函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms187786(v%3dsql.105)) | 对系统级的各种选项和对象进行操作或报告。                     |
| [系统统计函数 (Transact-SQL)](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms177520(v%3dsql.105)) | 返回有关 SQL Server 性能的信息。                             |
| [文本和图像函数](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms188353(v%3dsql.105)) | 可更改 text 和 image 的值。                                  |



## 2.5 Transact-SQL 表达式

[表达式](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms190718(v=sql.105))是标识符、值和运算符的组合，SQL Server 可以对其求值以获取结果。访问或更改数据时，可在多个不同的位置使用数据。例如，可以将表达式用作要在查询中检索的数据的一部分，也可以用作查找满足一组条件的数据时的搜索条件。

表达式可以是下列任何一种：

- 常量
- 函数
- 列名
- 变量
- 子查询
- CASE、NULLIF 或 COALESCE

还可以用运算符对这些实体进行组合以生成表达式。



## 2.6 Transact-SQL 运算符

| 算术运算符                                                   | 含义                                      |
| ------------------------------------------------------------ | ----------------------------------------- |
| [+（加）](https://docs.microsoft.com/zh-cn/sql/t-sql/language-elements/add-transact-sql?view=sql-server-2017) | 加,也可以将一个以天为单位的数字加到日期中 |
| [-（减）](https://docs.microsoft.com/zh-cn/sql/t-sql/language-elements/subtract-transact-sql?view=sql-server-2017) | 减,也可以从日期中减去一个以天为单位的数字 |
| [*（乘）](https://docs.microsoft.com/zh-cn/sql/t-sql/language-elements/multiply-transact-sql?view=sql-server-2017) | 乘                                        |
| [/ (Divide)](https://docs.microsoft.com/zh-cn/sql/t-sql/language-elements/divide-transact-sql?view=sql-server-2017) | 除                                        |
| [%（取模）](https://docs.microsoft.com/zh-cn/sql/t-sql/language-elements/modulo-transact-sql?view=sql-server-2017) | 返回一个除法运算的整数余数.               |

| 逻辑运算符 | 含义                                           |
| ---------- | ---------------------------------------------- |
| ALL        | 如果一组的比较都为TRUE,那么就为TRUE.           |
| AND        | 如果两个布尔表达式都为TRUE,那么就为TRUE.       |
| ANY        | 如果一组的比较中任何一个为TRUE,那么就为TRUE.   |
| BETWEEN    | 如果操作数在某个范围之内,那么就为TRUE.         |
| EXISTS     | 如果子查询包含一些行,那么就为TRUE.             |
| IN         | 如果操作数等于表达式列表中的一个,那么就为TRUE. |
| LIKE       | 如果操作数与一种模式相匹配,那么就为TRUE.       |
| NOT        | 对任何其他布尔运算符的值取反.                  |
| OR         | 如果两个布尔表达式中的一个为TRUE,那么就为TRUE. |
| SOME       | 如果在一组比较中,有些为TRUE,那么就为 TRUE.     |

| 位运算符 | 位运算 |
| ------ | ------ |
|&	|    按位与,如果两个位置上的位均为1,则结果为1.如: $(101)_2 \& (110)_2 = (100)_2$                   |
|&#124;	| 按位或,如果两个位置上任意一个位置的位为1,则结果为1.如: $(101)_2$ |
|^	|    按位异或,如果两个位置上数字相异,则结果为1.如: $(101)_2 \text{^} (110)_2 = (011)_2$            |
|~	|    按位非,对每个位位置上的位值取反.如: $~ (101)_2 = (010)_2$                                    |

| 比较运算符                                                   | 含义       |
| ------------------------------------------------------------ | ---------- |
| [=](https://docs.microsoft.com/zh-cn/sql/mdx/equal-to-mdx?view=sql-server-2017) | 等于       |
| [<>](https://docs.microsoft.com/zh-cn/sql/mdx/not-equal-to-mdx?view=sql-server-2017) | 不等于     |
| [>](https://docs.microsoft.com/zh-cn/sql/mdx/greater-than-mdx?view=sql-server-2017) | 大于       |
| [>=](https://docs.microsoft.com/zh-cn/sql/mdx/greater-than-or-equal-to-mdx?view=sql-server-2017) | 大于或等于 |
| [<](https://docs.microsoft.com/zh-cn/sql/mdx/less-than-mdx?view=sql-server-2017) | 小于       |
| [<=](https://docs.microsoft.com/zh-cn/sql/mdx/less-than-or-equal-to-mdx?view=sql-server-2017) | 小于或等于 |
| !=                                                           | 不等于     |
| !<                                                           | 不小于     |
| !>                                                           | 不大于     |

| 级别 | 运算符优先级（由高到低）                                     |
| ---- | ------------------------------------------------------------ |
| 1    | ~（位非）                                                    |
| 2    | *（乘）、/（除）、%（取模）                                  |
| 3    | +（正）、-（负）、+（加）、+（串联）、-（减）、&（位与）、^（位异或） |
| 4    | =、>、<、>=、<=、<>、!=、!>、!<（比较运算符）                |
| 5    | NOT                                                          |
| 6    | AND                                                          |
| 7    | ALL、ANY、BETWEEN、IN、LIKE、OR、SOME                        |
| 8    | =（赋值）                                                    |



## 2.6 Transact-SQL 注释

[注释](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms188621(v=sql.105))是程序代码中不执行的文本字符串（也称为备注）。注释可用于对代码进行说明或暂时禁用正在进行诊断的部分 Transact-SQL 语句和批。使用注释对代码进行说明，便于将来对程序代码进行维护。

SQL Server 支持两种类型的注释字符：

- --（双连字符）。这些注释字符可与要执行的代码处在同一行，也可另起一行。从双连字符开始到行尾的内容均为注释。对于多行注释，必须在每个注释行的前面使用双连字符。
- $\text{/* ...  */}$（正斜杠-星号字符对）。这些注释字符可与要执行的代码处在同一行，也可另起一行，甚至可以在可执行代码内部。



## 2.7 Transact-SQL 保留关键字

Microsoft SQL Server 将[保留关键字](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms189822(v=sql.105))用于定义、操作和访问数据库。保留关键字是 SQL Server 使用的 Transact-SQL 语言语法的一部分，用于分析和理解 Transact-SQL 语句和批处理。尽管在 Transact-SQL 脚本中使用 SQL Server 保留关键字作为标识符和对象名在语法上是可行的，但规定只能使用分隔标识符。

下表列出了 SQL Server 保留关键字。

| 关键字            | 关键字          | 关键字        |
| ----------------- | --------------- | ------------- |
| ADD               | EXISTS          | PRECISION     |
| ALL               | EXIT            | PRIMARY       |
| ALTER             | EXTERNAL        | PRINT         |
| AND               | FETCH           | PROC          |
| ANY               | FILE            | PROCEDURE     |
| AS                | FILLFACTOR      | PUBLIC        |
| ASC               | FOR             | RAISERROR     |
| AUTHORIZATION     | FOREIGN         | READ          |
| BACKUP            | FREETEXT        | READTEXT      |
| BEGIN             | FREETEXTTABLE   | RECONFIGURE   |
| BETWEEN           | FROM            | REFERENCES    |
| BREAK             | FULL            | REPLICATION   |
| BROWSE            | FUNCTION        | RESTORE       |
| BULK              | GOTO            | RESTRICT      |
| BY                | GRANT           | RETURN        |
| CASCADE           | GROUP           | REVERT        |
| CASE              | HAVING          | REVOKE        |
| CHECK             | HOLDLOCK        | RIGHT         |
| CHECKPOINT        | IDENTITY        | ROLLBACK      |
| CLOSE             | IDENTITY_INSERT | ROWCOUNT      |
| CLUSTERED         | IDENTITYCOL     | ROWGUIDCOL    |
| COALESCE          | IF              | RULE          |
| COLLATE           | IN              | SAVE          |
| COLUMN            | INDEX           | SCHEMA        |
| COMMIT            | INNER           | SECURITYAUDIT |
| COMPUTE           | INSERT          | SELECT        |
| CONSTRAINT        | INTERSECT       | SESSION_USER  |
| CONTAINS          | INTO            | SET           |
| CONTAINSTABLE     | IS              | SETUSER       |
| CONTINUE          | JOIN            | SHUTDOWN      |
| CONVERT           | KEY             | SOME          |
| CREATE            | KILL            | STATISTICS    |
| CROSS             | LEFT            | SYSTEM_USER   |
| CURRENT           | LIKE            | TABLE         |
| CURRENT_DATE      | LINENO          | TABLESAMPLE   |
| CURRENT_TIME      | LOAD            | TEXTSIZE      |
| CURRENT_TIMESTAMP | MERGE           | THEN          |
| CURRENT_USER      | NATIONAL        | TO            |
| CURSOR            | NOCHECK         | TOP           |
| DATABASE          | NONCLUSTERED    | TRAN          |
| DBCC              | NOT             | TRANSACTION   |
| DEALLOCATE        | NULL            | TRIGGER       |
| DECLARE           | NULLIF          | TRUNCATE      |
| DEFAULT           | OF              | TSEQUAL       |
| DELETE            | OFF             | UNION         |
| DENY              | OFFSETS         | UNIQUE        |
| DESC              | ON              | UNPIVOT       |
| DISK              | OPEN            | UPDATE        |
| DISTINCT          | OPENDATASOURCE  | UPDATETEXT    |
| DISTRIBUTED       | OPENQUERY       | USE           |
| DOUBLE            | OPENROWSET      | USER          |
| DROP              | OPENXML         | VALUES        |
| DUMP              | OPTION          | VARYING       |
| ELSE              | OR              | VIEW          |
| END               | ORDER           | WAITFOR       |
| ERRLVL            | OUTER           | WHEN          |
| ESCAPE            | OVER            | WHERE         |
| EXCEPT            | PERCENT         | WHILE         |
| EXEC              | PIVOT           | WITH          |
| EXECUTE           | PLAN            | WRITETEXT     |



## 2.8 Transact-SQL 语法约定

| 约定          | 使用场景                                                     |
| :------------ | :----------------------------------------------------------- |
| 大写          | Transact-SQL 关键字。                                        |
| *斜体*        | 用户提供的 Transact-SQL 语法的参数。                         |
| **粗体**      | 数据库名、表名、列名、索引名、存储过程、实用工具、数据类型名以及必须按所显示的原样键入的文本。 |
| 下划线        | 指示当语句中省略了包含带下划线的值的子句时应用的默认值。     |
| \|（竖线）    | 分隔括号或大括号中的语法项。只能使用其中一项。               |
| [ ]（方括号） | 可选语法项。不要键入方括号。                                 |
| { }（大括号） | 必选语法项。不要键入大括号。                                 |
| [**,**...n]   | 指示前面的项可以重复 n 次。各项之间以逗号分隔。              |
| [...n]        | 指示前面的项可以重复 n 次。每一项由空格分隔。                |
| ;             | Transact-SQL 语句终止符。                                    |
| <label> ::=   | 语法块的名称。此约定用于对可在语句中的多个位置使用的过长语法段或语法单元进行分组和标记。可使用语法块的每个位置由括在尖括号内的标签指示：<标签>。集是表达式的集合，例如 <分组集>；列表是集的集合，例如 <组合元素列表>。 |

除非另外指定，否则，所有对数据库对象名的 Transact-SQL 引用将是由四部分组成的名称，格式如下：

server_name**.**[database_name]**.**[schema_name]**.**object_name

| database_name**.**[schema_name]**.**object_name

| schema_name**.**object_name

| object_name

- server_name
  指定链接的服务器名称或远程服务器名称。
- database_name
  如果对象驻留在 SQL Server 的本地实例中，则指定 SQL Server 数据库的名称。如果对象在链接服务器中，则 database_name 将指定 OLE DB 目录。
- schema_name
  如果对象在 SQL Server 数据库中，则指定包含对象的架构的名称。如果对象在链接服务器中，则 schema_name 将指定 OLE DB 架构名称。
- object_name
  对象的名称。