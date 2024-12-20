# Use the official .NET SDK image as the build environment
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /source

# Copy csproj and restore as distinct layers
COPY *.csproj .
RUN dotnet restore

# Copy everything else and build
COPY . .
RUN dotnet publish -c release -o /app --no-restore

# Use a non-root user to avoid permission issues
RUN adduser --disabled-password appuser
RUN mkdir -p /app && chown -R appuser:appuser /app
USER appuser

# Final stage: runtime image
FROM mcr.microsoft.com/dotnet/runtime:7.0
WORKDIR /app
COPY --from=build /app .
USER appuser

ENTRYPOINT ["dotnet", "Worker.dll"]
