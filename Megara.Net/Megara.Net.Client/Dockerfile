FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Megara.Net/Megara.Net.Client/Megara.Net.Client.csproj", "Megara.Net/Megara.Net.Client/"]
RUN dotnet restore "Megara.Net/Megara.Net.Client/Megara.Net.Client.csproj"
COPY . .
WORKDIR "/src/Megara.Net/Megara.Net.Client"
RUN dotnet build "Megara.Net.Client.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Megara.Net.Client.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Megara.Net.Client.dll"]
