FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

COPY ["src/Services/IdentityService/IdentityService.csproj", "src/Services/IdentityService/"]
COPY ["SearchService/SearchService.csproj", "SearchService/"]
COPY ["AuctionService/AuctionService.csproj", "AuctionService/"]
COPY ["src/Services/GatewayService/GatewayService.csproj", "src/Services/GatewayService/"]

RUN dotnet restore "src/Services/IdentityService/IdentityService.csproj"
COPY . .
WORKDIR "/src/src/Services/IdentityService"
RUN dotnet build "IdentityService.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "IdentityService.csproj" -c Release -o /app/publish 

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "IdentityService.dll"]
