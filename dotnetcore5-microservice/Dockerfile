#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:5.0-buster AS build
WORKDIR /src
COPY ["dotnetcore5-microservice/dotnetcore5-microservice.csproj", "dotnetcore5-microservice/"]
RUN dotnet restore "dotnetcore5-microservice/dotnetcore5-microservice.csproj"
COPY . .
WORKDIR "/src/dotnetcore5-microservice"
RUN dotnet build "dotnetcore5-microservice.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "dotnetcore5-microservice.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dotnetcore5-microservice.dll"]
