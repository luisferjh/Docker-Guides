#### Open SqlServer engine on a docker a docker container
```
docker exec -it sqlserver /opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P Your_password123! -C
opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P "TuPasswordFuerte123!" -C
```

#### how to show containers running
```
docker ps
docker ps -a
```

#### How to run commands into a container
```
docker exec
docker exec 2cd891005afc cat /etc/debian_version
docker exec -it 2cd891005afc bash
docker run -it -d --restart always --name debian_test
docker exec ubuntu_with_figlet_2 cat tmp/hello.txt  

```

#### how to create a container
```
docker run -it -d --restart always --name debian_with_figlet debian-figlet:1.0 bash 
docker run -it -d --restart always --name nginx-server nginx_test:1.0 bash
docker run -p 80:80 -d --restart always --name nginx-server nginx_test:1.0
docker run -d --restart always --name nginx-server nginx_test:1.0
docker run -it -p 5277:8080 --name WebAppDocker webappdocker
```

#### how to create an image
```
docker image tag 1ae50d61886f debian-figlet:1.0
docker images
ubuntu_with_figlet ubuntu-figlet:1.0 bash
```

#### how to delete a container
```
docker rm stoic_germain
```

#### how to start a container
```
docker start beautiful_goldstine
```

#### how to stop a container
```
docker stop stoic_germain
```

#### another util commands
```
docker build -y ubuntu-figlet:1.0 .
docker build -t ubuntu-figlet:1.0 .
docker build -t ubuntu-figlet:1.1 .
docker build --no-cache -t testimage .
```