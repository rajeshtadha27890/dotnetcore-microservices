#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["dotnet-microservices/dotnet-microservices.csproj", "dotnet-microservices/"]
RUN dotnet restore "dotnet-microservices/dotnet-microservices.csproj"
COPY . .
WORKDIR "/src/dotnet-microservices"
RUN dotnet build "dotnet-microservices.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "dotnet-microservices.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dotnet-microservices.dll"]