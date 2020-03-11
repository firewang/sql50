SQLite C/C++ API
======================

https://sqlite.org/cintro.html

`完整C/C++ API <https://sqlite.org/c3ref/funclist.html>`_

SQLite重点对外提供2个对象8个方法。
sqlite3 → The database connection object. Created by sqlite3_open() and destroyed by sqlite3_close().

sqlite3_stmt → The prepared statement object. Created by sqlite3_prepare() and destroyed by sqlite3_finalize().

sqlite3_open() → Open a connection to a new or existing SQLite database. The constructor for sqlite3.

sqlite3_prepare() → Compile SQL text into byte-code that will do the work of querying or updating the database. The constructor for sqlite3_stmt.

sqlite3_bind() → Store application data into parameters of the original SQL.

sqlite3_step() → Advance an sqlite3_stmt to the next result row or to completion.

sqlite3_column() → Column values in the current result row for an sqlite3_stmt.

sqlite3_finalize() → Destructor for sqlite3_stmt.

sqlite3_close() → Destructor for sqlite3.

sqlite3_exec() → A wrapper function that does sqlite3_prepare(), sqlite3_step(), sqlite3_column(), and sqlite3_finalize() for a string of one or more SQL statements.



