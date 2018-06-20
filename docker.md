docker私库



2018年5月17日

15:13



docker run -d -p 5000:5000 -v \`pwd\`/data:/var/lib/registry --restart=always --name registry registry:2



来自 &lt;https://tonybai.com/2016/02/26/deploy-a-private-docker-registry/&gt; 



