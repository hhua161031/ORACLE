问题描述：
==========
plsql develiper表数据和存储过程注释和表数据中文`？`乱码
原因:
---------
oracle数据库`字符集`和plsql客户端`不一致`
问题解决<br>
--------
1，修改oracle数据库字符集<br>
`SQL> connect sys as sysdba`
输入口令:
ORACLE 例程已经启动。

Total System Global Area  146800640 bytes
Fixed Size                  1286220 bytes
Variable Size              75501492 bytes
Database Buffers           67108864 bytes
Redo Buffers                2904064 bytes
数据库装载完毕。
SQL>

SQL> alter  system  enable  restricted  session  ;

系统已更改。

SQL> alter  system  set  JOB_QUEUE_PROCESSES=0;

系统已更改。

SQL> alter  system  set  AQ_TM_PROCESSES=0;

系统已更改。

SQL> alter  database  open  ;

数据库已更改。

SQL>
SQL> alter  database  character  set  internal_use  ZHS16GBK  ;

数据库已更改。

SQL>select userenv('language') from dual;  
USERENV('LANGUAGE')  
\----------------------------------------------------  
SIMPLIFIED CHINESE_CHINA.ZHS16GBK`

2,修该plsql客户端字符集
