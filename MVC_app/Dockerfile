# See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

# This stage is used when running from VS in fast mode (Default for Debug configuration)
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
USER $APP_UID
WORKDIR /app
EXPOSE 8080




# Use the official .NET SDK image

# Set the working directory in the container
#WORKDIR /app

# Copy the project file(s) into the container
COPY MVC_app/MVC_app/MVC_app.csproj ./ 

# Restore dependencies (this step is optional but recommended to cache the restore layer)
RUN dotnet restore "MVC_app.csproj"

# Copy the entire source code into the container
COPY . ./
# Clean and rebuild again
RUN dotnet clean 

# Build the application
RUN dotnet build "MVC_app.csproj" -c Release -o /app/build

# Optionally, publish the application (if you plan to publish)
RUN dotnet publish "MVC_app.csproj" -c Release -o /app/publish

# Set the entry point for the container (adjust this based on your needs)
ENTRYPOINT ["dotnet", "/app/publish/MVC_app.dll"]
