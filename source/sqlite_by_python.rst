Python操作SQLite
==================

Python 2.5.x以上版本内置了SQLite3，使用时直接import sqlite3即可，而SQLite3 模块是由 Gerhard Haring 编写的。

python操作流程大概分为以下五步
通过sqlite3.open()创建与数据库文件的连接对象connection；
通过connection.cursor()创建光标对象cursor；
通过cursor.execute()执行SQL语句(CURD)；
通过connection.commit()提交当前的事务，或者通过cursor.fetchall()获得查询结果；
通过connection.close()关闭与数据库文件的连接。
建立连接：

import sqlite3
conn = sqlite3.connect('testDB.db')


“testDB.db”是前面所创建的数据库，当没有此数据库时也会自动创建一个。连接到数据库以后，按照上边的步骤就需要创建光标对象cursor 。
cursor = conn.cursor()


接下来就可以使用cursor.execute()直接执行SQL语句了。

建立数据库表：

cursor.execute('create table student(id int PRIMARY KEY,name text,age int)')


插入数据库表语句
cursor.execute("insert into student(id,name,age) values (1,'zhangsan',22)")
cursor.execute("insert into student values (2,'lisi',24)")
cursor.execute("insert into student values (?,?,?)",(3,"wangwu",25))
conn.commit()   --插入完之后提交

经过提交后使用以下语句查询
cursor.execute('select * from student')
cursor.fetchall()


SQLite3更新语句
cursor.execute(“update student set id=0 where age =22 ”)
>>> cursor.execute("update student set id=1 where name ='lisi'")
>>> conn.commit()
>>> cursor.execute("select * from student")
>>> cursor.fetchall()
[(0, u'zhangsan', 22), (1, u'lisi', 24), (3, u'wangwu', 25)]


SQLite3删除语句

cursor.execute("delete from student where age = ? ",(25, ));

当使用词语去删除时报参数错误，Python认为传递的字符串是一个元组，导致参数过多报错，传递一个参数时括号里一定要加逗号，不然Python会认为是数字，会报不支持的参数类型错误。而使用标准的语法删除时是没有问题的cursor.execute("delete from student where id = 0 ")；建议使用此语法删除。

关闭连接
>>> cursor.close()
>>> conn.close()


连接对象
conn

游标对象
cursor

异常处理
https://docs.python.org/3/library/sqlite3.html#exceptions


更新python内sqlite
python内置sqlite3升级(是对SQLite的升级)
从官网下载相应安装包 替换 sqlite3.dll   ， sqlite3.exe

默认安装下升级
替换路径下.dll 文件
C:\Python35\DLLs

anaconda下升级
C:\ProgramData\Anaconda3\Library\bin 


python使用sqlite3模块集成了SQLite数据库
import sqlite3
print(sqlite3.version_info) #显示python sqlite3模块的版本信息
print(sqlite3.sqlite_version) #显示SQLite3 数据库的版本信息

