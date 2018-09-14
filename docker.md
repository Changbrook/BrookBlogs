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

### dockerfile
RUN、CMD 和 ENTRYPOINT 这三个 Dockerfile 指令看上去很类似，很容易混淆。本节将通过实践详细讨论它们的区别。

简单的说：

RUN 执行命令并创建新的镜像层，RUN 经常用于安装软件包。

CMD 设置容器启动后默认执行的命令及其参数，但 CMD 能够被 docker run 后面跟的命令行参数替换。

ENTRYPOINT 配置容器启动时运行的命令。

