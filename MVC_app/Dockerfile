

# See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

# This stage is used when running from VS in fast mode (Default for Debug configuration)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081


# Use an official .NET SDK image to build the app
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Set the working directory to /src (change based on your repo structure)
WORKDIR /MVC_app

# Copy the csproj file and restore dependencies (this is essential for Docker to find the project file)
COPY ["MVC_app/MVC_app/MVC_app.csproj", "MVC_app/MVC_app/"]
RUN dotnet restore "MVC_app/MVC_app/MVC_app.csproj"

# Now copy the entire project and build
COPY . .
WORKDIR /MVC_app/MVC_app
RUN dotnet build "MVC_app.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Optionally, publish the app (for production)
RUN dotnet publish "MVC_app.csproj" -c $BUILD_CONFIGURATION -o /app/publish

# Use a smaller runtime image to run the app
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .

# Set entry point
ENTRYPOINT ["dotnet", "MVC_app.dll"]


