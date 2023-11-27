FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

COPY ["SearchService/SearchService.csproj", "SearchService/"]
COPY ["Contracts/Contracts.csproj", "Contracts/"]
COPY ["AuctionService/AuctionService.csproj", "AuctionService/"]
COPY ["src/Services/GatewayService/GatewayService.csproj", "src/Services/GatewayService/"]
COPY ["src/Services/IdentityService/IdentityService.csproj", "src/Services/IdentityService/"]

RUN dotnet restore "SearchService/SearchService.csproj"
COPY . .
WORKDIR "/src/SearchService"
RUN dotnet build "SearchService.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SearchService.csproj" -c Release -o /app/publish 

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SearchService.dll"]
