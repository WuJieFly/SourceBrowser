#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/SourceIndexServer/SourceIndexServer.csproj", "src/SourceIndexServer/"]
COPY ["src/Common/Common.csproj", "src/Common/"]
RUN dotnet restore "src/SourceIndexServer/SourceIndexServer.csproj"
COPY . .
WORKDIR "/src/src/SourceIndexServer"
RUN dotnet build "SourceIndexServer.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SourceIndexServer.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Microsoft.SourceBrowser.SourceIndexServer.dll"]