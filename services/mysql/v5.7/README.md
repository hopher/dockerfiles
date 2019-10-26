## 配置文件

```
/etc/mysql
/etc/mysql/mysql.conf.d/mysqld.cnf
```

## 日志输出配置

```
[mysqld]
log-output=FILE
general-log=1
general_log_file="/logs/mysqlquery.log"
```

或者

MySQL 控制台执行以下语句:
```
SET global log_output = 'file';
SET global general_log_file='/var/log/mysql/query.log'; 
SET global general_log = on;
```