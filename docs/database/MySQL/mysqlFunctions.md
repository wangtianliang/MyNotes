#MySQL常用函数

##查看锁及解除锁

1.查询是否锁表

	show OPEN TABLES where In_use > 0;

2.查询进程

   	show processlist
 	查询到相对应的进程===然后 kill    id

3.查看正在锁的事务

	SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS; 

4.查看等待锁的事务

	SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;

##字段调整

1.增加一个字段

	alter table user add COLUMN new1 VARCHAR(20) DEFAULT NULL COMMENT '注释' AFTER FIELD ; //增加一个字段，默认为空
	alter table user add COLUMN new2 VARCHAR(20) NOT NULL;  //增加一个字段，默认不能为空  www.2cto.com  
 
2.删除一个字段

	alter table user DROP COLUMN new2; 　 //删除一个字段
 
3.修改一个字段

	alter table user MODIFY new1 VARCHAR(10); 　//修改一个字段的类型
 
	alter table user CHANGE new1 new4 int;　 //修改一个字段的名称，此时一定要重新指定该字段的类型