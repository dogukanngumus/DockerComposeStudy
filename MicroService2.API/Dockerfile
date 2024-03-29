# Runtime'da çalışacak image ile başlayarak yeni bir dizin oluşturduk ve 80 portunu expose ettik
FROM mcr.microsoft.com/dotnet/aspnet:8.0 as base
WORKDIR /app
EXPOSE 80

# Sdk image'i base alarak restore ve publish işlemlerini başlat
FROM mcr.microsoft.com/dotnet/sdk:8.0 as build
WORKDIR /src
COPY ["MicroService2.API/MicroService2.API.csproj", "MicroService2.API/"]
RUN dotnet restore "MicroService2.API/MicroService2.API.csproj"
COPY . .
RUN dotnet build "MicroService2.API/MicroService2.API.csproj" -c Release -o /app/build

FROM build as publish
RUN dotnet publish "MicroService2.API/MicroService2.API.csproj" -c Release -o /app/publish

FROM base as final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT [ "dotnet", "MicroService2.API.dll" ]