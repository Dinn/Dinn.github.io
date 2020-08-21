---
title: 'Dev Tips \| MySQL의 character set과 collation 설정'
excerpt: 
date: 2020-07-04
last_modified_at: 2020-07-04T22:32:08

category:
  - Dev Tips

tags:
  - Database
  - collation
---

MySQL을 사용하다가 생성해놓은 database와 저장되는 데이터의 character set이나 collation 이 달라서 에러가 발생하는 경우를 다들 한번쯤 겪어봤을겁니다.

character set은 쉽게 말해 encoding 방식이라 보면 되고, collation은 데이터 비교를 위한 문자들의 순서를 나타내는 기준입니다.


## Database
### 데이터 베이스를 생성하며 설정할 때
```sql
CREATE DATABASE db_name
    [[DEFAULT] CHARACTER SET charset_name]
    [[DEFAULT] COLLATE collation_name]
```

예시

```sql
CREATE DATABASE db_name CHARACTER SET latin1 COLLATE latin1_swedish_ci;
```

### 기존의 데이터 베이스의 설정을 변경할 때
```sql
ALTER DATABASE db_name
    [[DEFAULT] CHARACTER SET charset_name]
    [[DEFAULT] COLLATE collation_name]
```
예시

```sql
ALTER DATABASE db_name CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

## Table
### 데이터 베이스를 생성하며 설정할 때
```sql
CREATE TABLE tbl_name (column_list)
    [[DEFAULT] CHARACTER SET charset_name]
    [COLLATE collation_name]
```

예시

```sql
CREATE TABLE tbl_name CHARACTER SET latin1 COLLATE latin1_swedish_ci;
```

### 기존의 데이터 베이스의 설정을 변경할 때
```sql
ALTER TABLE tbl_name
    [[DEFAULT] CHARACTER SET charset_name]
    [COLLATE collation_name]
```

예시

```sql
ALTER DATABASE tbl_name CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

## utf8mb4
컴퓨터에서 한글을 사용하기 위해선 utf8 인코딩 방식을 따라야 한다는 것은 다들 아실 것입니다.

**utf8mb4**은 기존 utf8에서 4 btyes 를 더해 emoji까지 표현할 수 있는 인코딩 방식입니다.

MySQL에서 `status` 명령어를 통해 현재 database server의 기본 character set / collation을 확인할 수 있습니다.

```bash
mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.30, for Linux (x86_64) using  EditLine wrapper

Connection id:		4
Current database:	
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		5.7.30-0ubuntu0.18.04.1 (Ubuntu)
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	latin1
Db     characterset:	latin1
Client characterset:	utf8
Conn.  characterset:	utf8
UNIX socket:		/var/run/mysqld/mysqld.sock
Uptime:			47 min 58 sec

Threads: 1  Questions: 28  Slow queries: 0  Opens: 112  Flush tables: 1  Open tables: 105  Queries per second avg: 0.009
--------------
```

아래의 방법으론 한 데이터베이스의 character set / collation을 확인할 수 있고요.

```sql
show variables like 'character_set_database';
```

## References
> [MySQL :: MySQL 8.0 Reference Manual :: 10.3.3 Database Character Set and Collation](https://dev.mysql.com/doc/refman/8.0/en/charset-database.html)
>
> [Charset과 Collation에 대한 개념](https://sshkim.tistory.com/128)
