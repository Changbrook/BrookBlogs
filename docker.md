docker私库

2018年5月17日

15:13

docker run -d -p 5000:5000 -v \`pwd\`/data:/var/lib/registry --restart=always --name registry registry:2

来自 &lt;[https://tonybai.com/2016/02/26/deploy-a-private-docker-registry/&gt](https://tonybai.com/2016/02/26/deploy-a-private-docker-registry/&gt);





docker操作



2018年5月30日

9:42



robot检查命令：

docker cp /root/config/integration\_robot\_properties.py openecompete\_container:/share/config/integration\_robot\_properties.py



docker exec -it openecompete\_container /var/opt/OpenECOMP\_ETE/runTags.sh -i health h -d ./html -V /share/config/integration\_robot\_properties.py -V /share/config/integration\_preload\_parameters.py -V /share/config/vm\_properties.py













portal遭遇意外重启后，需要先删除/opt/config/boot.txt，初始化才能成功。

