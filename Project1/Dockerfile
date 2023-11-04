FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Project1/Project1.csproj", "Project1/"]
RUN dotnet restore "Project1/Project1.csproj"
COPY . .
WORKDIR "/src/Project1"
RUN dotnet build "Project1.csproj" -c Release -o /app/build

FROM node:latest AS node_base
WORKDIR /src/ClientApp
RUN echo "NODE Version:" && node --version
RUN echo "NPM Version:" && npm --version
RUN npm install

WORKDIR "/src/Project1"

FROM build AS publish
RUN dotnet publish "Project1.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Project1.dll"]