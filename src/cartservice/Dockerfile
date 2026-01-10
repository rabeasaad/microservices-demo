# Build Stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

COPY *.sln .
COPY src ./src
COPY tests ./tests

RUN dotnet restore
RUN dotnet publish src/cartservice -c Release -o /app/publish

# Runtime Stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .

EXPOSE 7070
ENTRYPOINT ["dotnet", "cartservice.dll"]
