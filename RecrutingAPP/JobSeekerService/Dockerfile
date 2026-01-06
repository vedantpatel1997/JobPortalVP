FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["RecrutingAPP/Shared.Contracts/Shared.Contracts.csproj", "Shared.Contracts/"]
COPY ["RecrutingAPP/JobSeekerService/JobSeekerService.csproj", "JobSeekerService/"]
RUN dotnet restore "JobSeekerService/JobSeekerService.csproj"
COPY RecrutingAPP/Shared.Contracts/ ./Shared.Contracts/
COPY RecrutingAPP/JobSeekerService/ ./JobSeekerService/
WORKDIR "/src/JobSeekerService"
RUN dotnet build "JobSeekerService.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "JobSeekerService.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "JobSeekerService.dll"]
