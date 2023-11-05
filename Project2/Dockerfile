FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base_1
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build_1
WORKDIR /src
COPY ["Project2.csproj", "Project2/"]
RUN dotnet restore "Project2/Project2.csproj"
COPY . /src/Project2/
WORKDIR "/src/Project2"
RUN dotnet build "Project2.csproj" -c Release -o /app/build

FROM build_1 AS publish_1
RUN dotnet publish "Project2.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM node:latest AS node-builder
WORKDIR /node
COPY ./ClientApp /node
RUN npm install
RUN npm run build

FROM base_1 AS final_1
WORKDIR /app
COPY --from=publish_1 /app/publish .
COPY --from=node-builder /node /app/ClientApp
ENTRYPOINT ["dotnet", "Project2.dll"]
