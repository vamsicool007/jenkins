#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["s3_task/s3_task.csproj", "s3_task/"]
RUN dotnet restore "s3_task/s3_task.csproj"
COPY . .
WORKDIR "/src/s3_task"
RUN dotnet build "s3_task.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "s3_task.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "s3_task.dll"]