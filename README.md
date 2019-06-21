# lnmp-cluster-with-docker
docker一键部署php/mysql/nginx高可用集群


1.修改docker-compose.yml里的XTRABACKUP_PASSWORD 和 MYSQL_ROOT_PASSWORD 为你的数据库密码

2.docker-compose.yml文件的ip地址可根据自己宿主机的docker环境进行设置和更改，本说明中以docker-compose.yml源文件里的ip地址举例

3.宿主机执行
      mkdir -p /www/wwwroot/web/www 
     
  /www/wwwroot/web/www为项目代码目录，也可以通过更改docker-compose.yml里的volumes进行自定义

4.执行docker-compose up

5.docker容器全部启动后进入随便一个db容器，创建name为haproxy的user，举例说明
     docker exec -it db1 bash
     mysql -uroot -p -h127.0.0.1
     
     CREATE USER 'haproxy'@'%'
     flush privileges;

6.因为mysql采用pxc集群，用haproxy做负载均衡，同时双节点的haproxy实现双机热备，这里我们需要手动启动keepalived
     docker exec -it haproxy1 bash
     service keepalived start
     
     docker exec -it haproxy2 bash
     service keepalived start
7.nginx通过keepalived同样实现双机热备，也需要手动启动keepalived
     docker exec -it balance1 bash
     service keepalived start
     
     docker exec -it balance2 bash
     service keepalived start
     
8.宿主机/www/wwwroot/web/www目录创建info.php

9.curl http://172.18.0.189:6102/info.php 就可以看到了
