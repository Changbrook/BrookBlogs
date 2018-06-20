mongoDB命令



2017年11月14日

20:28



	1. 主mongoDB

cd /usr/local/mongodb

./bin/mongod --config /usr/local/mongodb/etc/master.conf



	2. 从mongoDB

cd /usr/local/mongodb

./bin/mongod --config /usr/local/mongodb/etc/slave.conf



检查端口是否在使用

lsof -i :27017



