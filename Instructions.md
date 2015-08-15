On the host machine: 

#Create a diretory to store server data    
##  for example: /home/server-data/mysqldata  

mkdir /home/server-data  
mkdir /home/server-data/mysqldata  

#Add the config file my.cnf (in the root of this repository) to another directory on the host, for example:  

/home/server-cfg/my.cnf  

vim /home/server-cfg/my.cnf 
(paste in the contents, adjust as you see fit)  

#Create a data container  (this will map the directories & files from the host to a docker container) 
docker create -v /home/server-data/mysqldata:/var/lib/mysql -v /home/server-cfg/my.cnf:/etc/mysql/my.cnf --name mysqldata busybox  

#create the mysql container  
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=secretpassword -e MYSQL_DATABASE=mydatabase -e MYSQL_USER=myuser -e MYSQL_PASSWORD=mypassword -p 3306:3306 --volumes-from mysqldata elevate/mysql     

#connect to the server  
docker run -it --rm --link mysql:mysql elevate/mysql bash -c 'mysql -h $MYSQL_PORT_3306_TCP_ADDR -p'  
