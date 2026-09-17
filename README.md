# Simple Web API – Learning Project

This project is a hands-on exploration of:

- ASP.NET Core Minimal APIs
- OpenAPI
- HTTP endpoints in C#

## Environment Setup

Project location:

`%USERPROFILE%\projects\simple-webapi`

## .NET SDK

This project pins its .NET SDK version in `global.json`, which currently requires **10.0.401**.

Pinning matters because, without it, `dotnet` uses the newest SDK installed on the machine. The
build can then change silently the day someone installs a new one, and behave differently on two
machines with no visible cause.

The pin was originally created with:

`dotnet new globaljson --sdk-version 10.0.202`

In September 2026 a machine update removed 10.0.202, and the build failed rather than quietly
using a different SDK — which is the pin doing its job. It was re-pinned by editing the version
in `global.json` by hand.

To change the pinned version, edit `global.json` directly, or re-run the command above with the
version you want.

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

## Where configuration comes from

| # | Layer (lowest → highest priority) | What writes values into it | Active in this project? |
|---|---|---|---|
| 1 | `appsettings.json` | You: a committed file in the project, edited by hand | Yes — sets log levels and `AllowedHosts` |
| 2 | `appsettings.{Environment}.json` | You: one committed file per environment. Which file loads is decided by the environment name, read from `ASPNETCORE_ENVIRONMENT` | Yes locally: `appsettings.Development.json`, because `launchSettings.json` sets `Development`. In production the name defaults to `Production`, and there is no `appsettings.Production.json` |
| 3 | User secrets | The `dotnet user-secrets set` command, writing a `secrets.json` stored outside the project folder (under `%APPDATA%\Microsoft\UserSecrets\`). Loaded only in Development | No — the `.csproj` has no `UserSecretsId`; `dotnet user-secrets init` would add one |
| 4 | Environment variables | `launchSettings.json`, via `dotnet run` (dev only: `applicationUrl` becomes `ASPNETCORE_URLS`, plus its `environmentVariables` block) · your own shell before running, e.g. `$env:Logging__LogLevel__Default = "Debug"` · the hosting platform or deploy scripts in production. Nested keys use `__` in place of `:` | Yes — `launchSettings.json` sets `ASPNETCORE_ENVIRONMENT` and `ASPNETCORE_URLS` |
| 5 | Command-line arguments | Whoever launches the process: anything typed after `--` in `dotnet run -- --Logging:LogLevel:Default=Debug`, or after the `.dll` in production. It reaches configuration only because `Program.cs` passes `args` into `WebApplication.CreateBuilder(args)` | Wired up, but empty unless something is passed |

Layers combine key by key: a higher layer overrides only the keys it sets, and every other key keeps its value from the layers below.

## The `learning/desugar` branch

Branch with the same semantics as main, but a more explicit implementation to reveal hidden ASP.NET Core mechanisms.

Never merge this branch, as it is meant for learning by comparison.

### Same semantics verification

#### Option 1: Manual pipeline

Use two PowerShell terminals: **A** runs the server, **B** makes the requests. `dotnet run` never returns, so the requests can't be typed in the terminal running it.

The `http` profile matters: under `https`, requests to port 5046 get a redirect instead of the document.

**A** — start the original version:

```powershell
git switch main
dotnet run --launch-profile http
```

**B** — capture the baseline:

```powershell
curl.exe http://localhost:5046/openapi/v1.json -o $env:TEMP\openapi-baseline.json
```

**A** — stop it, switch, start the explicit version:

```powershell
# press Ctrl+C to stop the app, then:
git switch learning/desugar
dotnet run --launch-profile http
```

**B** — capture again:

```powershell
curl.exe http://localhost:5046/openapi/v1.json -o $env:TEMP\openapi-after.json
```

**A** — press Ctrl+C to stop the app.

**B** — compare:

```powershell
Compare-Object (Get-Content $env:TEMP\openapi-baseline.json) (Get-Content $env:TEMP\openapi-after.json)
```

No output means the two OpenAPI documents are identical: same semantics.

#### Option 2: GitHub Actions

Planned: run Option 1 automatically on every push.

## Branch to-do

Placeholder for to-do list

- [ ] ...
