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

# Switch to the ClientApp directory within your project
WORKDIR "/src/Project1/ClientApp"

# Install Node.js and npm within the ClientApp directory
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get install -y nodejs
RUN npm install

# Move back to the main project directory before publishing
WORKDIR "/src/Project1"

FROM build AS publish
RUN dotnet publish "Project1.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Project1.dll"]
