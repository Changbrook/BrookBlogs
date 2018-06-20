oracle操作命令





	1. su - oracle 进入oracle账户

lsnrctl status 查看状态

	2. lsnrctl start 启动服务

	3. sqlplus /nolog 进入sql命令

	4. conn as sysdba 登陆用户

用户:cmedb/oracle

	5. startup 启动数据库

	

设置数据库文件大小

alter database datafile '/home/oracle/app/oradata/orcl/cmedb.dbf' resize 30000m;



