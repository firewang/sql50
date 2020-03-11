SQLite基础信息
===================

SQLite语法
-----------------

SQLite 是不区分大小写的，但也有一些命令是大小写敏感的，比如 GLOB 和 glob 在 SQLite 的语句中有不同的含义。

所有的 SQLite 语句可以以任何关键字开始，如 SELECT、INSERT、UPDATE、DELETE、ALTER、DROP 等，
所有的语句以分号（;）结束。

注释是以两个连续的 "-" 字符（ASCII 0x2d）开始，
并扩展至下一个换行符（ASCII 0x0a）或直到输入结束，以先到者为准。

也可以使用 C 风格的注释，以 "\/\*" 开始，并扩展至下一个 "\*\/" 字符对或直到输入结束，以先到者为准。
SQLite的注释可以跨越多行。

其他的DML, DDL, DQL语法在下文进行讲解。

SQLite点命令
-----------------

============================ ==============================================================================
.archive ...                 Manage SQL archives                                        
.auth ON\|OFF                Show authorizer callbacks                                  
.backup ?DB? FILE            备份 DB 数据库（默认是 "main"）到 FILE 文件。              
.bail on\|off                发生错误后停止。默认为 OFF。                               
.binary on\|off              是否输出二进制结果。默认为 OFF。                           
.cd DIRECTORY                切换工作目录到DIRECTORY                                    
.changes on\|off             显示执行的SQL影响的行的数量                                
.check GLOB                  Fail if output since .testcase does not match              
.clone NEWDB                 将当前数据库的数据复制到数据库NEWDB                        
.databases                   列出数据库的名称及其所依附的文件。                         
.dbconfig ?op? ?val?         List or change sqlite3_db_config() options                 
.dbinfo ?DB?                 显示数据库状态信息                                         
.dump ?TABLE? ...            以 SQL 文本格式转储数据库。                                
.echo ON\|OFF                开启或关闭 echo 命令。                                     
.eqp on\|off\|full\|...      Enable or disable automatic EXPLAIN QUERY PLAN             
.excel                       Display the output of next command in spreadsheet          
.exit ?CODE?                 退出 SQLite 提示符。                                       
.expert                      EXPERIMENTAL. Suggest indexes for queries                  
.explain ?on\|off\|auto?     开启或关闭适合于 EXPLAIN 的输出模式。默认：auto            
.filectrl CMD ...            Run various sqlite3_file_control() operations              
.fullschema ?--indent?       Show schema and the content of sqlite_stat tables          
.headers on\|off             开启或关闭头部显示。                                       
.help ?-all? ?PATTERN?       显示PATTERN的帮助消息。                                    
.import FILE TABLE           导入来自 FILE 文件的数据到 TABLE 表中。                    
.imposter INDEX TABLE        Create imposter table TABLE on index INDEX                 
.indexes ?TABLE?             显示所有索引的名称。                                       
.limit ?LIMIT? ?VAL?         Display or change the value of an SQLITE_LIMIT             
.lint OPTIONS                Report potential schema issues.                            
.load FILE ?ENTRY?           加载一个扩展库。                                           
.log FILE\|off               开启或关闭日志。FILE文件可以是 stderr（标准错误）/stdout（标准输出）。 
.mode MODE ?TABLE?           设置输出模式，MODE 可以是下列之一：
                              + **csv** 逗号分隔的值
                              + **column** 左对齐的列
                              + **html** HTML 的 <table> 代码
                              + **insert** TABLE表的SQL插入（insert）语句
                              + **line** 每行一个值**list**
                              + 由 .separator字符串分隔的值
                              + **tabs** 由 Tab 分隔的值
                              + **tcl** TCL列表元素

.nullvalue STRING            在 NULL 值的地方输出 STRING 字符串。                         
.once (-e\|-x\|FILE)         Output for the next SQL command only to FILE                 
.open ?OPTIONS? ?FILE?       Close existing database and reopen FILE                      
.output ?FILE?               发送输出到 FILE 文件或者 stdout                              
.parameter CMD ...           Manage SQL parameter bindings                                
.print STRING...             逐字地输出 STRING 字符串。                                   
.progress N                  Invoke progress handler after every N opcodes                
.prompt MAIN CONTINUE        替换标准提示符。                                             
.quit                        退出                                                         
.read FILE                   读取FILE                                                     
.recover                     尽可能恢复数据库数据                                         
.restore ?DB? FILE           Restore content of DB (default "main") from FILE             
.save FILE                   将内存内数据库写入FILE                                       
.scanstats on\|off           Turn sqlite3_stmt_scanstatus() metrics on or off             
.schema ?PATTERN?            Show the CREATE statements matching PATTERN                  
.selftest ?OPTIONS?          Run tests defined in the SELFTEST table                      
.separator COL ?ROW?         改变列、行的分隔符                                           
.sha3sum ...                 对数据库内容计算SHA3的hash值                                 
.shell CMD ARGS ...          Run CMD ARGS... in a system shell                            
.show                        显示各种设置的当前值。                                       
.stats ?on\|off?             显示统计，开启或关闭统计。                                   
.system CMD ARGS ...         Run CMD ARGS... in a system shell                            
.tables ?TABLE?              列出匹配 LIKE 模式的表的名称。                               
.testcase NAME               Begin redirecting output to 'testcase-out.txt'               
.testctrl CMD ...            Run various sqlite3_test_control() operations                
.timeout MS                  尝试打开锁定的表 MS 毫秒。                                   
.timer on\|off               开启或关闭 CPU 定时器（SQL timer）。                         
.trace ?OPTIONS?             Output each SQL statement as it is run                       
.vfsinfo ?AUX?               Information about the top-level VFS                          
.vfslist                     List all available VFSes                                     
.vfsname ?AUX?               Print the name of the VFS stack                              
.width NUM1 NUM2 ...         为 "column" 模式设置列宽度。                                 
============================ ==============================================================================


SQLite创建数据库
-----------------------

$sqlite3 [DatabaseName].db


SQLite创建表

CREATE TABLE database_name.table_name(

column1 datatype  PRIMARY KEY(one or more columns),

column2 datatype,

.....

columnN datatype,

)；

创建完之后。便可以使用.tables查看相关的表，使用..schema tablename查看表具体信息。使用DROP TABLE database_name.table_name即可删除表，一旦删除表中信息将无法找回了。

SQLite的增、删、改、查和之前学过的MySQL、Oracle语法一样，均是采用标准SQL格式, 简单如下图所示：


更新

查询read
系统级信息
所有表名 
表内列名
表schema

删除 delete