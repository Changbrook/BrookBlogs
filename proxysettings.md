

```
#!/bin/bash
echo "export ftp_proxy=\"http://x.x.x.x:3128\"" >> /root/.bash_profile
echo "export https_proxy=\"http://x.x.x.x:3128\"" >> /root/.bash_profile
source /root/.bash_profile

#apt
echo "Acquire::http::proxy \"http://x.x.x.x:3128\";" > /etc/apt/apt.conf
echo "Acquire::ftp::proxy \"http://x.x.x.x:3128\";" >> /etc/apt/apt.conf
echo "Acquire::https::proxy \"http://x.x.x.x:3128\";" >> /etc/apt/apt.conf
hn=`hostname`
echo 127.0.0.1 $hn >> /etc/hosts
echo "nameserver x.x.x.x" >> /etc/resolvconf/resolv.conf.d/head
resolvconf -u

#git
git config --global https.proxy http://x.x.x.x:3128
git config --global http.proxy http://x.x.x.x:3128
git config --global http.sslVerify false

#docker
#1604
mkdir -p /etc/systemd/system/docker.service.d
echo "[Service]" > /etc/systemd/system/docker.service.d/http-proxy.conf
echo "Environment=\"HTTP_PROXY=http://x.x.x.x:3128/\" \"HTTPS_PROXY=http://x.x.x.x:3128/\"" >> /etc/systemd/system/docker.service.d/http-proxy.conf
systemctl daemon-reload
systemctl restart docker
#or
service docker restart

#1404
vi /etc/default/docker
service docker restart
```

