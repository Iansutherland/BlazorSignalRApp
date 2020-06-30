#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["BlazorSignalRApp/Server/BlazorSignalRApp.Server.csproj", "BlazorSignalRApp/Server/"]
COPY ["BlazorSignalRApp/Client/BlazorSignalRApp.Client.csproj", "BlazorSignalRApp/Client/"]
COPY ["BlazorSignalRApp/Shared/BlazorSignalRApp.Shared.csproj", "BlazorSignalRApp/Shared/"]
RUN dotnet restore "BlazorSignalRApp/Server/BlazorSignalRApp.Server.csproj"
COPY . .
WORKDIR "/src/BlazorSignalRApp/Server"
RUN dotnet build "BlazorSignalRApp.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorSignalRApp.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorSignalRApp.Server.dll"]