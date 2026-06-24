# Simple Web API – Learning Project

This project is a hands-on exploration of:

- ASP.NET Core Web API
- Microsoft Entra ID
- OAuth 2.0 Client Credentials Flow

## Environment Setup

Project location:
```
C:\Users\%USERPROFILE%\projects\simple-webapi
```

## .NET SDK

This project uses a fixed .NET SDK version to ensure consistency across environments.

### Locking SDK version

Command used:

dotnet new globaljson --sdk-version 10.0.202

Purpose:
- Ensures the project always uses .NET 10.0.202
- Prevents issues caused by different SDK versions

## Create Web API

Command used:

`dotnet new webapi`

What it does:
- Creates a new ASP.NET Core Web API project
- Generates default structure (controllers, configuration, etc.)
- Includes a sample endpoint (WeatherForecast)
- Enables Swagger for testing APIs in the browser

---

## Package Restore

Command:

`dotnet restore`

Purpose:
- Downloads all dependencies required by the project
- Ensures the project can build and run correctly

Note:
- Restore may fail if package sources are misconfigured or inaccessible

---

## Configure Package Sources (if needed)

If you encounter errors during restore:

- Ensure only valid and accessible package sources are enabled
- Disable any sources that require authentication or are not needed

Check sources:

`dotnet nuget list source`

---

## Run the API

Command:

`dotnet run`

What happens:
- Builds the project
- Starts a local web server

Important:
- The application does NOT open a browser automatically
- You must check the terminal output for the listening address

Example output:
```
Now listening on: http://localhost:5046
```

---

## Access the API

After running the application:

- Open your browser
- Navigate to the URL shown in the terminal output

Example:
```
http://localhost:5046/weatherforecast
```
Or use Swagger UI:
```
http://localhost:5046/swagger
```
Purpose:
- Confirms the API is running correctly
- Allows testing endpoints
