#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#FROM mcr.microsoft.com/azure-functions/dotnet:3.0 AS base
#WORKDIR /app
#EXPOSE 80
#
#FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
#WORKDIR /src
#COPY ["ServerlessFunctions/ServerlessFunctions.csproj", "ServerlessFunctions/"]
#RUN dotnet restore "ServerlessFunctions/ServerlessFunctions.csproj"
#COPY . .
#WORKDIR "/src/ServerlessFunctions"
#RUN dotnet build "ServerlessFunctions.csproj" -c Release -o /app/build
#
#FROM build AS publish
#RUN dotnet publish "ServerlessFunctions.csproj" -c Release -o /app/publish
#
#FROM base AS final
#WORKDIR /app
#COPY --from=publish /app/publish .
#ENV AzureWebJobsScriptRoot=/app

#---------------------------------------COMMANDS---------------------------------------------
# >func init --docker-only
# >docker build -t ServerlessFunctions:v1 .
# >docker images
# >docker run ServerlessFunctions:v1

# >docker ps                     -> to view which docker containers are currently running
# >docker stop CONTAINERID       ->to stop
# >docker rm CONTAINERID         ->to remove instance created when we ran docker run

# >$connstr = az storage account show-connection-string -g serverless-functions-docker -n serverlessfunctionsdocker -0 tsv             -->to get connection string 
# >docker run -e AzureWebJobsStorage=$connstr -p 8080:80 ServerlessFunctions:v1                                                        -->now it will run without connection string errors

FROM microsoft/dotnet:2.2-sdk AS installer-env

COPY . /src/dotnet-function-app
RUN cd /src/dotnet-function-app && \
    mkdir -p /home/site/wwwroot && \
    dotnet publish *.csproj --output /home/site/wwwroot

# To enable ssh & remote debugging on app service change the base image to the one below
# FROM mcr.microsoft.com/azure-functions/dotnet:2.0-appservice 
FROM mcr.microsoft.com/azure-functions/dotnet:2.0
ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
    AzureFunctionsJobHost__Logging__Console__IsEnabled=true

COPY --from=installer-env ["/home/site/wwwroot", "/home/site/wwwroot"]