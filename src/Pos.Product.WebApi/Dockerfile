FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY ["Pos.Product.WebApi/Pos.Product.WebApi.csproj", "Pos.Product.WebApi/"]
COPY ["Pos.Product.Domain/Pos.Product.Domain.csproj", "Pos.Product.Domain/"]
COPY ["Pos.Event.Contracts/Pos.Event.Contracts.csproj", "Pos.Event.Contracts/"]
COPY ["Pos.Product.Infrastructure/Pos.Product.Infrastructure.csproj", "Pos.Product.Infrastructure/"]
RUN dotnet restore "Pos.Product.WebApi/Pos.Product.WebApi.csproj"
COPY . .
WORKDIR "/src/Pos.Product.WebApi"
RUN dotnet build "Pos.Product.WebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Pos.Product.WebApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Pos.Product.WebApi.dll"]