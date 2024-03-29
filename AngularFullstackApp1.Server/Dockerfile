#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["angularfullstackapp1.client/nuget.config", "angularfullstackapp1.client/"]
COPY ["AngularFullstackApp1.Server/AngularFullstackApp1.Server.csproj", "AngularFullstackApp1.Server/"]
COPY ["angularfullstackapp1.client/angularfullstackapp1.client.esproj", "angularfullstackapp1.client/"]
RUN dotnet restore "./AngularFullstackApp1.Server/./AngularFullstackApp1.Server.csproj"
COPY . .
WORKDIR "/src/AngularFullstackApp1.Server"
RUN dotnet build "./AngularFullstackApp1.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./AngularFullstackApp1.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AngularFullstackApp1.Server.dll"]