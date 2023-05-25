# Pull the base image for build stage
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

# Set working directory within the image
# and copy the project file to it, run restore on the project
WORKDIR /src
COPY backend.csproj .
RUN dotnet restore

# Copy everything else (mostly, source code files) to the image
# Build and publish the app to /app directory
COPY . .
RUN dotnet publish -c release -o /app

# Pull the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Copy the app from /app in the build image to the /app directory of this image
# Set the entrypoint of this image to launch/start the backend service
COPY --from=build /app .
ENTRYPOINT ["dotnet", "backend.dll"]

# CD to the directory containing this dockerfile
# Run the command to build the image:
#   docker build -t pizzabackend:v0.1-dev .
