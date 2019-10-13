# Get Base Image (Full .NET Core SDK) & Creatspecify a dedicated working directory where our app will resides
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env
WORKDIR /app

# Copy csproj file from our PC to the Working Container directory(/app) and restore
COPY *.csproj ./
RUN dotnet restore
# Resolve any project dependencies(done using .csproj file and retreiving any additional via Nuget)
# Copy the rest of files into our workind DIR and build
COPY . ./
RUN dotnet publish -c Release -o out
# Above we specify that it is an RELEASE build and specifying an folder(OUT), 
# to contain the app build dll and sny support files & libraries
# Generate runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2
WORKDIR /app
EXPOSE 80
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "WebApiDocker.dll"]


##Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat
#
#	FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-nanoserver-1803 AS base
#	WORKDIR /app
#	EXPOSE 80
#	EXPOSE 443
#
#	FROM mcr.microsoft.com/dotnet/core/sdk:2.1-nanoserver-1803 AS build
#	WORKDIR /src
#	COPY ["WebApiDocker/WebApiDocker.csproj", "WebApiDocker/"]
#	RUN dotnet restore "WebApiDocker/WebApiDocker.csproj"
#	COPY . .
#	WORKDIR "/src/WebApiDocker"
#	RUN dotnet build "WebApiDocker.csproj" -c Release -o /app
#
#	FROM build AS publish
#	RUN dotnet publish "WebApiDocker.csproj" -c Release -o /app
#
#	FROM base AS final
#	WORKDIR /app
#	COPY --from=publish /app .
#	ENTRYPOINT ["dotnet", "WebApiDocker.dll"]