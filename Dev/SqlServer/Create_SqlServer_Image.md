### DOWNLOAD THE OFFICIAL SQL SERVER IMAGEN
```cmd
> docker pull mcr.microsoft.com/mssql/server:2022-latest
```

### CREATION OF CONTAINER WITH VOLUME
```cmd
 > docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=TuPasswordFuerte123!" -p 1433:1433 --name sqlserverWithVolume -v ~/sqlserver_data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2022-latest
```

```cmd
 > docker run -it -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=TuPasswordFuerte123!" -p 1433:1433 --name sqlserverWithVolume -v ~/sqlserver_data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2022-latest
```

* The ~ symbol specifies the home directory of the host; for Windows, it would be. 
C:\Users\juan\sqlserver_data
* Permissions must be granted to mssql so that it can access this folder

### STORAGE OPTION CREATING VOLUME
```cmd
> docker volume create sqlserver_volume
```

```cmd
> docker run -it -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=TuPasswordFuerte123!" -p 1433:1433 --name sqlserverWithVolume -v sqlserver_volume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2022-latest
```

### ENTER THE CONTAINER AND CREATE THE FOLDER WHERE WE ARE GOING TO STORE THE BACKUP. / 

> docker exec -it sqlserverWithVolume bash
```cmd
>$ var/opt/mssql && mkdir backup
>$ exit
```

### RESTORE THE DATABASE 

Place on the route where we have the backup

```cmd
> docker cp _MyDatabase.bak_ sqlserverWithVolume:/var/opt/mssql/backup/MyDatabase.bak_ 
```

```cmd
> docker exec -it sqlserverWithVolume /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P YourPasswordFuerte123! -C
```

```SQL
1. RESTORE DATABASE _MyDatabase_
2. FROM DISK = '/var/opt/mssql/backup/_MyDatabase.bak_'
3. WITH MOVE '_MyDatabase_' TO '/var/opt/mssql/data/_MyDatabase.mdf_',
4. MOVE '_MyDatabase_log_' TO '/var/opt/mssql/data/_MyDatabase_log.ldf_';
5. GO
```