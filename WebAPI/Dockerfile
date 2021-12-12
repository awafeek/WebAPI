#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["WebAPI.csproj", "WebAPI/"]
ARG PAT
RUN dotnet nuget add source "http://get-nuget:5000/nuget" --name "get-nuget" 
RUN dotnet nuget add source "https://pkgs.dev.azure.com/GET-UAE/7070bf3d-24a5-454e-b0ce-0ed395c38096/_packaging/smart-patrol/nuget/v3/index.json" --name "smart-patrol" --username "awafeek@getgroup.com" --password "$PAT" --store-password-in-clear-text
RUN dotnet restore "WebAPI/WebAPI.csproj"
COPY . WebAPI/
WORKDIR "/src/WebAPI"
RUN dotnet build "WebAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebAPI.dll"]