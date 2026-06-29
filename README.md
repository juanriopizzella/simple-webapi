# Simple Web API – Learning Project

This project is a hands-on exploration of:

- ASP.NET Core Minimal APIs
- OpenAPI
- HTTP endpoints in C#

## Environment Setup

Project location:

`%USERPROFILE%\projects\simple-webapi`

## .NET SDK

This project uses a fixed .NET SDK version for consistency.

Command used:

`dotnet new globaljson --sdk-version 10.0.202`

This creates `global.json`, which tells .NET to use SDK version `10.0.202`.

## Create Web API

Command used:

`dotnet new webapi`

What it created:

- A minimal ASP.NET Core Web API project
- A `Program.cs` file that configures and starts the app
- Configuration files like `appsettings.json` and `launchSettings.json`
- A sample `/weatherforecast` endpoint
- An `.http` file for testing requests from the editor

## Restore Packages

Command:

`dotnet restore`

Purpose:

- Downloads the NuGet packages required by the project

## Run the API

Command:

`dotnet run`

What happens:

- The project is built
- A local web server starts listening on the configured URL

This project’s launch settings define these local URLs:

- `http://localhost:5046`
- `https://localhost:7031`

## Test the API

Example endpoint:

`/weatherforecast`

Examples:

- `http://localhost:5046/weatherforecast`
- `https://localhost:7031/weatherforecast`

You can also use the `simple-webapi.http` file in the editor to send a test request.

## OpenAPI

In Development, the app exposes an OpenAPI document because `AddOpenApi()` and `MapOpenApi()` are enabled in `Program.cs`.

Note:
This project does not currently include Swagger UI.