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

