#### maven 编译打包跳过测试命令

mvn clean install -U -DskipTests=true -Dmaven.test.skip=true -Dmaven.javadoc.skip=true

-DskipTests=true 表明编译测试代码但不执行测试
-Dmaven.test.skip=true 表明既不编译测试代码也不执行测试
-Dmaven.javadoc.skip=true 表明跳过生成javadoc




