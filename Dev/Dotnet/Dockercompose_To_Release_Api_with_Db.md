### CREATE A SQL CONTAINER AND A WEBAPI CONTAINER IN THE SAME NETWORK

```yml
services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: sqlserver
    environment:
      SA_PASSWORD: "Your_password123!"
      ACCEPT_EULA: "Y"
    ports:
      - "1433:1433"
    volumes:
      - ~/sqlserver_data:/var/opt/mssql
    networks:
      - mynetwork

  webapi:
    build:
      context: .
    container_name: webapi
    ports:
      - "7124:8080"
    depends_on:
      - sqlserver
    networks:
      - mynetwork

networks:
  mynetwork:
```