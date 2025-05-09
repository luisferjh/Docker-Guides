### CREATE A WEBAPI CONTAINER WITH DEPENDENCIES

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY *.sln, .
COPY ["./OnlineStoreApp/OnlineStoreApp.csproj", "OnlineStoreApp/"]
COPY ["./OnlineStoreApp.Repository.EFCore/OnlineStoreApp.Repository.EFCore.csproj", "OnlineStoreApp/"]
RUN dotnet restore "./OnlineStoreApp/OnlineStoreApp.csproj"
COPY . .
WORKDIR /src/OnlineStoreApp
RUN dotnet build "./OnlineStoreApp.csproj" -c $BUILD_CONFIGURATION -o /app/build


FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./OnlineStoreApp.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OnlineStoreApp.dll"]
```