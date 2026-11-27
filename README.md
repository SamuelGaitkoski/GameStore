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
