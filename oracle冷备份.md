冷备份:<br>
-----------
### 1，诠释：
数据库文件的物理备份，用shutdown normal 和shutdown immediate关闭数据库后对数据库各文件进行备份，<br>
这些文件构成数据库在关闭前的一个完全的物理镜像。<br>

### 2，需要备份的文件:
   所有的数据文件;
   所有的控制文件;
   所有的联机重做文件;
   初始化参数文件（可选）;
   <br>
### 3，一般步骤:
    正常关闭要备份的实例;
    备份整个数据库到一个目录;
    启动数据库;
### 4，实际操作
#### 4.1  sqlplus连接数据库
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份1.png) 

#### 4.2 查看源库的数据文件、控制文件、日志文件所在位置
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份2.png) 

#### 4.3 关闭数据库
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份3.png) 

#### 4.4 复制数据文件，控制文件，日志文件

`cp G:/working/****.dbf  目标目录`

#### 4.5启动数据库

冷备份遇到问题<br>
-------------------
### 问题1：
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份4.png) 

### 原因：
`在进行冷备份中，因为控制文件是镜像文件，在复制过程中往往会是一个控制文件，如果还原回去会和oracle`<br>
`原始记忆的几个控制文件冲突，或者是由于停电或者其他原因，备份的控制文件和数据库中不一样，会产生冲突。`<br>
`这时候要将控制文件进行修改。`
### 操作：
1，首先shutdown normal 关闭数据库 <br>

2，connect sys as sysdba 连接空闲实例<br>

3，startup nomount  启动实例<br>

4，修改控制文件<br>
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份5.png) 

5，alter database mount<br>

6,alter database open<br>

### 问题2：
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份6.png) 

### 原因：
`主要是控制文件没有记录最新的数据文件和日志文件的信息采用的办法是重写控制文件`
### 操作：
1，startup <br>
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份7.png) 

2,执行下面命令，会在该目录下生成一个原有控制文件内容的文件<br>
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份8.png) 

3,退出数据库<br>
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份9.png) 

4，修改生成的脚本<br>
CREATE CONTROLFILE REUSE DATABASE "ORCL" NORESETLOGS  NOARCHIVELOG
    MAXLOGFILES 16
    MAXLOGMEMBERS 3
    MAXDATAFILES 100
    MAXINSTANCES 8
    MAXLOGHISTORY 292
LOGFILE
  GROUP 1 'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\REDO01.LOG'  SIZE 50M BLOCKSIZE 512,
  GROUP 2 'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\REDO02.LOG'  SIZE 50M BLOCKSIZE 512,
  GROUP 3 'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\REDO03.LOG'  SIZE 50M BLOCKSIZE 512
DATAFILE
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\SYSTEM01.DBF',
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\SYSAUX01.DBF',
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\UNDOTBS01.DBF',
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\USERS01.DBF',
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\PAUL01.DBF',
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\DGP.DBF',
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\DEMOZT.DBF',
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\CQDEMO.DBF',
  'G:\WORKING\SOFTWARE\MYORACLE\ORADATA\ORCL\DGPHZ.DBF'
CHARACTER SET ZHS16GBK
<br>
### 注：
1.在rebuildctl.sql这个文件里边，编辑的时候注意不要有多余的空行。比如：
LOGFILE
  GROUP 1 'D:\ORACLE\PRODUCT\10.2.0\DB_1\TLYDB\REDO01.LOG'  SIZE 50M,
  GROUP 2 'D:\ORACLE\PRODUCT\10.2.0\DB_1\TLYDB\REDO02.LOG'  SIZE 50M,
  GROUP 3 'D:\ORACLE\PRODUCT\10.2.0\DB_1\TLYDB\REDO03.LOG'  SIZE 50M
DATAFILE
  'D:\ORACLE\PRODUCT\10.2.0\DB_1\TLYDB\SYSTEM01.DBF',
datafile和上面的紧紧跟着，不要有空行。（当然，这只是一个小问题）

5，执行脚本要进入numount状态<br>
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份10.png) 

6，执行脚本生成新的控制文件<br>
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份11.png) 

7<br>
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份12.png) 

8<br>
![github](https://github.com/hhua161031/ORACLE/blob/master/image/冷备份13.png) 



