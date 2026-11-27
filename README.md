# ASP.NET Core with .NET 8

A learning project built while following **Julio Casal**'s ASP.NET Core course on YouTube.

📺 Course playlist: [ASP.NET Core with .NET 8 — Julio Casal](https://www.youtube.com/watch?v=AhAxLiGC7Pc&list=PLeD0-5Hw0ZJ8U7NmCqObexO-mnuts2vi1)

## About

This repository contains the code written step by step alongside the videos, covering how to build a web API with ASP.NET Core and .NET 8.

The project is **GameStore.Api**, a REST API for a game store, built on the **ASP.NET Core Empty** template and grown step by step through the course.

## Solution

The repo uses a **solution** (`GameStore.sln`) to group the different projects that together build the application. Right now it holds just `GameStore.Api`, and more projects can be added to it as the app grows. Opening the solution in VS Code (C# Dev Kit) or Visual Studio loads all its projects at once, and building the solution builds every project in it.

### Solution Explorer

C# Dev Kit adds a **Solution Explorer** view to the VS Code Explorer sidebar. It shows the solution, its projects, and a **Dependencies** node under each project. If it's hidden, right-click the `Explorer` header (or click `···`) and check **Solution Explorer**, or run `.NET: Open Solution` from the Command Palette.

In the course, Julio Casal uses Solution Explorer to show how it helps manage dependencies across the different projects. He's showing the **projects**, because that's where dependencies live, not in the solution:

- **The `.sln`** only lists which projects belong together, so you can build and open them as one unit.
- **Each `.csproj`** stores that project's own dependencies:
  - `<ProjectReference>` for other projects in the solution
  - `<PackageReference>` for NuGet packages
- **Solution Explorer** gives you a visual way to edit those `.csproj` entries. Right-click a project and use **Add Project Reference** or **Add NuGet Package**, then see the result under the project's **Dependencies** node.

## The project file (.csproj)

`GameStore.Api.csproj` is the **project file**. It tells the .NET SDK what kind of project this is and how to build it.

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

</Project>
```

| Part | What it means |
| --- | --- |
| `Sdk="Microsoft.NET.Sdk.Web"` | The project type. The **Web SDK** makes this an ASP.NET Core web project, so it references the ASP.NET Core framework and supports web features like `launchSettings.json` and `appsettings.json`. A plain console app or class library uses `Microsoft.NET.Sdk` instead. |
| `<TargetFramework>net8.0</TargetFramework>` | The **target framework**: the .NET version the project builds for and runs on (.NET 8). |
| `<Nullable>enable</Nullable>` | Turns on **nullable reference types**, so the compiler warns about possible `null` values. That's why the editor shows types like `WebApplication?`. |
| `<ImplicitUsings>enable</ImplicitUsings>` | Adds common `using` directives automatically (like `System` and `Microsoft.AspNetCore.Builder`), so `Program.cs` needs no `using` lines at the top. |

As the project grows, the `.csproj` also lists:

- **NuGet packages** (imported libraries), with `<PackageReference Include="..." Version="..." />`, for example Entity Framework Core later in the course
- **Other projects** it depends on, with `<ProjectReference Include="..." />`
- Other **build settings**, like the output type, assembly name or root namespace

You can add packages from the CLI with `dotnet add package <name>`, or from Solution Explorer.

## Building the project

The **build process** turns the code into binaries: it compiles the C# source files into an assembly (`GameStore.Api.dll`) that .NET can run. There are a few ways to start a build:

- **Solution Explorer:** right-click the `GameStore.Api` project and choose **Build** to build just that project.
- **Command Palette:** run `.NET: Build`.
- **CLI:** run `dotnet build` in the project folder.

You can also build **all the projects at once through the solution**:

- **Solution Explorer:** right-click the solution (`GameStore`) and choose **Build**.
- **CLI:** run `dotnet build` in the repo root, where `GameStore.sln` is, or `dotnet build GameStore.sln`.
- **VS Code build task:** press `Cmd+Shift+B` (`Ctrl+Shift+B` on Windows/Linux), or run **Tasks: Run Build Task** from the Command Palette. C# Dev Kit provides `dotnet: build` tasks, so you can pick the solution or a single project from the list without writing a `tasks.json`.

Building the solution builds every project it lists, in the right order based on their project references.

The build output appears in the terminal, ending with the path of the built DLL. `dotnet run` builds the project first too, so you don't need a separate build before running.

## Build output (bin and obj)

Building the project creates two folders inside `GameStore.Api`:

| Folder | What it holds |
| --- | --- |
| `obj/` | **Intermediate/temporary files** used during the build: NuGet restore data (`project.assets.json`), generated source files and partly compiled output. The build uses them to put the final result together and to skip work that hasn't changed. |
| `bin/` | The **final build output**: the compiled **assembly** `GameStore.Api.dll` (your app's code as a DLL), plus the files needed to run it, like copied `appsettings` files and the DLLs of referenced packages. For a Debug build it's in `bin/Debug/net8.0/`. |

Both folders are generated, so they're in `.gitignore` and never committed. It's safe to delete them (or run `dotnet clean`), and the next build creates them again.

## Running and debugging in VS Code

Press **`F5`** (or **Run > Start Debugging**) to run the application. This starts a **debug session**: VS Code builds the project, runs it and attaches the debugger to it.

The first time, VS Code asks you to **choose a debugger**. Pick **C#**, then choose the project and launch profile to run (for example `GameStore.Api [http]`, from `launchSettings.json`).

During a debug session you can:

- Set **breakpoints** by clicking left of a line number, so the app pauses there
- **Step** through the code and inspect variables while paused
- Use the debug toolbar to continue, restart or stop

| Shortcut | What it does |
| --- | --- |
| `F5` | Start debugging (or continue when paused) |
| `Ctrl+F5` | Run without debugging |
| `Shift+F5` | Stop the debug session |

Because the profile has `launchBrowser` set to `true`, a browser opens on the app's URL when it starts.

## Configuration (appsettings.json)

The `appsettings` files hold the app's **configuration**: settings that can change from one environment to another (development, staging, production) without changing code. Examples are log levels, allowed hosts and, later in the course, the database connection string.

| File | When it's used |
| --- | --- |
| `appsettings.json` | Base settings, loaded in **every** environment |
| `appsettings.Development.json` | Loaded only in the **Development** environment, on top of `appsettings.json` |

- **Which environment:** set by the `ASPNETCORE_ENVIRONMENT` environment variable. `Properties/launchSettings.json` sets it to `Development` when you run the app locally with `dotnet run`.
- **Overrides:** an environment file only needs the settings that differ. Any key it repeats replaces the value from `appsettings.json`. Other environments work the same way, like `appsettings.Production.json`.
- **Loading:** `WebApplication.CreateBuilder(args)` loads these files automatically, and the values are available through `builder.Configuration`.

Configuration can also come from other sources, which override the JSON files in this order (later wins): `appsettings.json` → `appsettings.{Environment}.json` → user secrets (Development only) → environment variables → command-line arguments.

## Launch profiles (launchSettings.json)

`Properties/launchSettings.json` defines **launch profiles**. Each profile is a set of settings for running the app **locally**: which URLs it listens on, which environment variables it gets, and whether a browser opens. It's only used on your machine, not when the app is deployed.

| Profile | URL(s) | What it does |
| --- | --- | --- |
| `http` | `http://localhost:5277` | Runs the app with the Kestrel web server over plain HTTP |
| `https` | `https://localhost:7220` and `http://localhost:5277` | Runs the app with Kestrel over HTTPS, and HTTP too |
| `IIS Express` | `http://localhost:51787` (SSL port `44312`) | Runs the app behind IIS Express, which is Windows and Visual Studio only. The `iisSettings` section configures it. |

The settings inside each profile:

- **`commandName`:** how the app is started. `Project` runs it directly with Kestrel (like `dotnet run`), and `IISExpress` runs it behind IIS Express.
- **`applicationUrl`:** the URL(s) the app listens on.
- **`environmentVariables`:** variables set for the run. All three profiles set `ASPNETCORE_ENVIRONMENT` to `Development`, which is why `appsettings.Development.json` is loaded.
- **`launchBrowser`:** opens a browser when the app starts from an IDE debugger. `dotnet run` doesn't open one.
- **`dotnetRunMessages`:** shows extra startup messages in the terminal when using `dotnet run`.

`dotnet run` uses the first profile, `http`. To pick another, run `dotnet run --launch-profile https`.

### How `ASPNETCORE_ENVIRONMENT` reaches the app

The app itself never reads `launchSettings.json`. It happens in two steps:

1. **The tool that starts the app** (`dotnet run`, VS Code or Visual Studio) reads the chosen profile and sets its `environmentVariables`, like `ASPNETCORE_ENVIRONMENT=Development`, on the app's process.
2. **ASP.NET Core** reads `ASPNETCORE_ENVIRONMENT` from the process's environment variables when `WebApplication.CreateBuilder(args)` runs. It uses the value to pick the environment, for example to load `appsettings.Development.json`. The current environment is available as `app.Environment`.

If `ASPNETCORE_ENVIRONMENT` isn't set, the environment defaults to **Production**. That's what happens when the app is deployed, since `launchSettings.json` isn't used there.

### HTTP vs HTTPS

- **HTTP** sends requests and responses as plain text, so anyone on the network could read or change them.
- **HTTPS** is HTTP encrypted with **TLS**, which keeps traffic private and proves the server is who it claims to be. It's what real apps use in production.

Locally, HTTPS needs the .NET **development certificate**. Run `dotnet dev-certs https --trust` once so your browser trusts it.

## Program.cs

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
```

- `WebApplication.CreateBuilder(args)` creates a builder that sets up configuration, logging and services.
- `builder.Build()` creates the `WebApplication`, stored in `app`.
- `app.MapGet(...)` maps an endpoint: a `GET` request to `/` returns `"Hello World!"`.
- `app.Run()` starts the app and listens for HTTP requests.

### `app` is the host

`app` is our **host**. It's a `WebApplication`, which does three jobs at once:

- **Host** (`IHost`): runs the app, starts the web server (Kestrel) and manages the app's lifetime.
- **Request pipeline** (`IApplicationBuilder`): where you add middleware.
- **Endpoint routing** (`IEndpointRouteBuilder`): where you map endpoints like `MapGet`.

> Hovering `app` in VS Code shows `WebApplication? app`. The `?` appears because `var` is always inferred as nullable for reference types. The hover also says `'app' is not null here`, so there's nothing to fix.

## What this course covers

- Creating ASP.NET Core apps
- Understanding REST APIs
- Implementing CRUD endpoints
- Data Transfer Objects (DTOs)
- Extension methods
- Route groups
- Handling invalid inputs
- Entity Framework Core (ORM)
- The configuration system
- Dependency injection
- Service lifetimes
- Mapping entities to DTOs
- Asynchronous programming
- Frontend integration

## Tech stack

- .NET 8 SDK
- ASP.NET Core

## Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Visual Studio Code](https://code.visualstudio.com/)

## VS Code extensions

- [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) (Microsoft)
- [.NET Install Tool](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.vscode-dotnet-runtime) (Microsoft)
- [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) (Huachao Mao)
- [SQLite](https://marketplace.visualstudio.com/items?itemName=alexcvzz.vscode-sqlite) (alexcvzz)

Installing C# Dev Kit also installs its dependencies, the [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) extension and the .NET Install Tool.

> **IntelliCode for C# Dev Kit is deprecated.** Microsoft now recommends GitHub Copilot instead, so it's not used here. You don't lose normal C# IntelliSense without it. Completions, go-to-definition and error checking come from the C# extension that C# Dev Kit installs. IntelliCode only added AI ranking and whole-line suggestions on top, and Copilot does that job now.

## .NET CLI commands

| Command | What it does |
| --- | --- |
| `dotnet --version` | Shows the .NET SDK version in use |
| `dotnet --info` | Shows details on installed SDKs, runtimes, and environment |
| `dotnet new <template>` | Creates a new project from a template |
| `dotnet new list` | Lists the project templates available in the SDK |
| `dotnet new web -n GameStore.Api` | Creates the `GameStore.Api` project from the **ASP.NET Core Empty** template (`web`); `-n` sets the project name |

> `web` is the **ASP.NET Core Empty** template, a minimal app with just `Program.cs`, and it's the one this course uses. Don't confuse it with `webapi` (**ASP.NET Core Web API**), which adds Swagger and a sample weather endpoint.

## C# Dev Kit commands

Because C# Dev Kit is installed, many tasks can be done from the VS Code Command Palette (`Cmd+Shift+P` on macOS, `Ctrl+Shift+P` on Windows/Linux). Type `.NET:` to see all options.

> The `>` prefix is what tells VS Code to search commands. `Cmd+Shift+P` adds it automatically. If you open Quick Open with `Cmd+P` (`Ctrl+P`) instead, which searches files, type `>` first to switch to commands.

Some common commands:

| Command | What it does |
| --- | --- |
| `.NET: New Project...` | Creates a new project from a template (like `dotnet new`) |
| `.NET: Open Solution` | Opens an existing solution |
| `.NET: Build` | Builds the solution |
| `.NET: Clean` | Removes build outputs |

## Getting started

```bash
# Check the SDK
dotnet --version   # should be 8.x

# Restore and run (once the project is created)
dotnet restore
dotnet run
```

## Credits

All course content and teaching by [Julio Casal](https://www.youtube.com/@juliocasal). This repo is my personal follow-along implementation.
