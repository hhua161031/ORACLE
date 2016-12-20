问题描述：
==========
plsql develiper表数据和存储过程注释和表数据中文`？`乱码
原因:
---------
oracle数据库`字符集`和plsql客户端`不一致`
问题解决<br>
--------
1,修改oracle数据库字符集<br>
`SQL> connect sys as sysdba`

`输入口令:`

`ORACLE 例程已经启动。`

`Total System Global Area  146800640 bytes
Fixed Size                  1286220 bytes
Variable Size              75501492 bytes
Database Buffers           67108864 bytes
Redo Buffers                2904064 bytes
数据库装载完毕。`

`SQL>`

`SQL> alter  system  enable  restricted  session  ;`
`系统已更改。`

`SQL> alter  system  set  JOB_QUEUE_PROCESSES=0;`
`系统已更改。`

`SQL> alter  system  set  AQ_TM_PROCESSES=0;`
`系统已更改。`

`SQL> alter  database  open  ;`
`数据库已更改。`

`SQL>`
`SQL> alter  database  character  set  internal_use  ZHS16GBK  ;`
`数据库已更改。`

`SQL>select userenv('language') from dual;  `
`USERENV('LANGUAGE')  `<br>
`----------------------------------------------------`<br>
`SIMPLIFIED CHINESE_CHINA.ZHS16GBK`

2,修该plsql客户端字符集<br>
* 查询客户端字符集
![客户端字符集](https://github.com/hhua161031/ORACLE/blob/master/image/字符.jpg)<br>
如果和数据库不一致需要修改；<br>

3,进入 我的电脑,属性,高级,环境变量,添加2项:LANG=zh_CN.GBK 和 NLS_LANG="SIMPLIFIED CHINESE_CHINA.ZHS16GBK" <br> 

4,PLSQL DEVELOPER,Tools,Preferences,User InterFace,Fonts,Main Font 修改成中文字体
