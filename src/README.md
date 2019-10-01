知识点
- .env文件会默认作为docker-compose整个文件的变量来源
- env-file指定的文件会填充到environment



#使用dockerhub镜像启动,连接外部数据库
sudo docker run --name gopub -e MYSQL_HOST=10.168.1.216
-e MYSQL_PORT=3306  \
-e MYSQL_USER=root \
-e MYSQL_PASS=123456 \
-e MYSQL_DB=walle \
-p 8192:8192  \
--restart always \
 -d   lc13579443/gopub:latest

#使用dockerhub镜像启动,连接Docker数据库
sudo docker run --name gopub-mysql -h gopub-mysql \
 -p 3306:3306 \
  -v /data/gopub-mysql:/var/lib/mysql \
  -v /etc/localtime:/etc/localtime \
  -e MYSQL_ROOT_PASSWORD=123456 \
   --restart always \
   -d mysql:5.7.24 \
   --character-set-server=utf8mb4 \
   --collation-server=utf8mb4_unicode_ci

sudo docker run --name gopub --link gopub-mysql:gopub-mysql \
-e MYSQL_HOST=gopub-mysql -e MYSQL_PORT=3306  \
-e MYSQL_USER=root -e MYSQL_PASS=123456 \
-e MYSQL_DB=walle -p 8192:8192  --restart always  -d \
lc13579443/gopub:latest