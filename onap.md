允许root SSH登陆

1. vi /etc/ssh/sshd\_config

修改下面的内容

\# Authentication:

LoginGraceTime 120

PermitRootLogin yes

StrictModes yes

…

\# Change to no to disable tunnelled clear text passwords

PasswordAuthentication yes

1. service sshd restart或者service ssh restart



Bind9学习



2018年5月4日

15:39



https://blog.csdn.net/hmglly/article/details/1384205

XML的坑



2018年5月14日

11:50



&lt;?xml version="1.0" encoding="UTF-8"?&gt;必须在所有内容之前，即文件内容的第一行。

SSL的安装



2018年5月14日

11:51



问题现象



首先执行了 

update-ca-certificates -f



来自 &lt;https://stackoverflow.com/questions/6784463/error-trustanchors-parameter-must-be-non-empty&gt; 

不知道有没有用，但是错误信息改变了。



然后使用keytool导入证书，到java目录下面。

下载你所访问的SSL站点的证书, 执行以下命令导入（进入到JAVA JDK下的jre\lib\security目录）

Cmd代码  



	1. keytool -import -file /data/test1.cer -keystore cacerts -alias server  

        接下来 会提示输入密码，默认密码为 changeit，输入之后，选择“是”将其安装到JDK 可信证书库中。如果看到以下结果，则导入成功。





来自 &lt;http://bijian1013.iteye.com/blog/2310856&gt; 

