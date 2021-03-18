# mysql
Mysql operation hand book


# mysql-bin log文件清理  
mysql主从会在主会产生大量如mysql-bin*的log日志文件，这会消耗大量的硬盘空间。在保持MySQL主从复制的功能下有两种解决方法：  
1. 设置日志expire_logs_days  

设置日志expire_logs_days  
修改MySQL配置文件，设置expire_logs_days，重启MySQL。  

### vim /etc/my.cnf  

expire_logs_days = x //日志自动删除的天数。一般讲x设置的短点，如10  
直接在MySQL里设置expire_logs_days，无需重启MySQL（临时重启后回复）。  

### mysql -u root -p  

> show binary logs;  
> show variables like '%log%';  
> set global expire_logs_days = 10;  

2. 手动清楚bin日志文件。  

手动清理bin日志文件（特定的）  

### mysql -u root -p  

> purge master logs before date_sub(current_date, interval 50 day);  //删除50天前的mysql-bin log日志文件  
> show master logs; # 查看删除状态  

也可以重置master，删除所有bin文件（所有的）  

### mysql -u root -p  

> reset master;  

备注：  
PURGE MASTER LOGS手动删除用法示例  

### mysql -u root -p

> purge master logs to 'mysql-bin.010’; //清除mysql-bin.010日志  
> purge master logs before '2017-08-28 13:00:00'; //清除2016-02-28 13:00:00前的日志  
> purge master logs before date_sub(now(), interval 3 day); //清除3天前的bin日志  
