    # Stage 1: Build the application...
    # Uses the .NET SDK image to compile and publish the application.
    FROM mcr.microsoft.com/dotnet/sdk:9.0 AS base
    WORKDIR /app
    EXPOSE 8080
    EXPOSE 8081

    FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
    WORKDIR /src
    # Copies the solution file (.sln) and then the project file (.csproj)
    # This is common in VS-generated Dockerfiles to handle multi-project solutions.
    COPY ["WebApiDocker/WebApiDocker.csproj", "WebApiDocker/"]
    RUN dotnet restore "WebApiDocker/WebApiDocker.csproj"
    COPY . .
    WORKDIR "/src/WebApiDocker"
    RUN dotnet build "WebApiDocker.csproj" -c Release -o /app/build

    # Stage 2: Publish the application
    # Creates the deployable output.
    FROM build AS publish
    RUN dotnet publish "WebApiDocker.csproj" -c Release -o /app/publish /p:UseAppHost=false

    # Stage 3: Create the final runtime image
    # Uses the smaller ASP.NET runtime image to run the published application.
    FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
    WORKDIR /app
    COPY --from=publish /app/publish .7
    ENTRYPOINT ["dotnet", "WebApiDocker.dll"]

    

    