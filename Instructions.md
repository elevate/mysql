#Create a data container  
docker create -v /home/server-data/mysqldata:/var/lib/mysql -v /home/server-cfg/my.cnf:/etc/mysql/my.cnf --name mysqldata busybox  

#create the mysql container  
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=secretpassword -e MYSQL_DATABASE=mydatabase -e MYSQL_USER=myuser -e MYSQL_PASSWORD=mypassword -p 3306:3306 --volumes-from mysqldata elevate/mysql     

#connect to the server  
docker run -it --rm --link mysql:mysql elevate/mysql bash -c 'mysql -h $MYSQL_PORT_3306_TCP_ADDR -p'  
