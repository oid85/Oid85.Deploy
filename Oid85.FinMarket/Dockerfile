FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 1001

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
ENV ASPNETCORE_ENVIRONMENT=Development
WORKDIR /src
COPY ["Oid85.FinMarket.WebHost/Oid85.FinMarket.WebHost.csproj", "Oid85.FinMarket.WebHost/"]
COPY ["Oid85.FinMarket.Application/Oid85.FinMarket.Application.csproj", "Oid85.FinMarket.Application/"]
COPY ["Oid85.FinMarket.Common/Oid85.FinMarket.Common.csproj", "Oid85.FinMarket.Common/"]
COPY ["Oid85.FinMarket.Domain/Oid85.FinMarket.Domain.csproj", "Oid85.FinMarket.Domain/"]
COPY ["Oid85.FinMarket.External/Oid85.FinMarket.External.csproj", "Oid85.FinMarket.External/"]
COPY ["Oid85.FinMarket.DataAccess/Oid85.FinMarket.DataAccess.csproj", "Oid85.FinMarket.DataAccess/"]
RUN dotnet restore "./Oid85.FinMarket.WebHost/Oid85.FinMarket.WebHost.csproj"
COPY . .
WORKDIR "/src/Oid85.FinMarket.WebHost"
RUN dotnet build "./Oid85.FinMarket.WebHost.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
ENV ASPNETCORE_ENVIRONMENT=Development
RUN dotnet publish "./Oid85.FinMarket.WebHost.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Oid85.FinMarket.WebHost.dll"]