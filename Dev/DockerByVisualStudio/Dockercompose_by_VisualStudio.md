
#### Dockerfile

```dockerfile

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["OnlineStoreApp/OnlineStoreApp.csproj", "OnlineStoreApp/"]
COPY ["OnlineStoreApp.Repository.EFCore/OnlineStoreApp.Repository.EFCore.csproj", "OnlineStoreApp.Repository.EFCore/"]
COPY ["OnlineStoreApp.Entities/OnlineStoreApp.Entities.csproj", "OnlineStoreApp.Entities/"]
COPY ["OnlineStoreApp.UseCases/OnlineStoreApp.UseCases.csproj", "OnlineStoreApp.UseCases/"]
COPY ["OnlineStore.DTOs/OnlineStore.DTOs.csproj", "OnlineStore.DTOs/"]
RUN dotnet restore "./OnlineStoreApp/OnlineStoreApp.csproj"
COPY . .
WORKDIR "/src/OnlineStoreApp"
RUN dotnet build "./OnlineStoreApp.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./OnlineStoreApp.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OnlineStoreApp.dll"]
```

#### Docker Compose 

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
      - sqlvolume:/var/opt/mssql
    networks:
      - mynetwork

  onlinestoreapp:
    image: ${DOCKER_REGISTRY-}onlinestoreapp
    build:
      context: .
      dockerfile: OnlineStoreApp/Dockerfile
    container_name: onlinestoreapp
    ports:
      - "7124:8080"
    depends_on:
      - sqlserver
    networks:
      - mynetwork

networks:
  mynetwork:
  

volumes:
  sqlvolume:
```