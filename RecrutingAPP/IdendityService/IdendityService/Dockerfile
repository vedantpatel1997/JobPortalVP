FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["RecrutingAPP/Shared.Contracts/Shared.Contracts.csproj", "Shared.Contracts/"]
COPY ["RecrutingAPP/IdendityService/IdendityService/IdendityService.csproj", "IdendityService/"]
RUN dotnet restore "IdendityService/IdendityService.csproj"
COPY RecrutingAPP/Shared.Contracts/ ./Shared.Contracts/
COPY RecrutingAPP/IdendityService/IdendityService/ ./IdendityService/
WORKDIR "/src/IdendityService"
RUN dotnet build "IdendityService.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "IdendityService.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "IdendityService.dll"]
