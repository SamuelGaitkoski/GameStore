# ASP.NET Core with .NET 8

A learning project built while following **Julio Casal**'s ASP.NET Core course on YouTube.

📺 Course playlist: [ASP.NET Core with .NET 8 — Julio Casal](https://www.youtube.com/watch?v=AhAxLiGC7Pc&list=PLeD0-5Hw0ZJ8U7NmCqObexO-mnuts2vi1)

## About

This repository contains the code written step by step alongside the videos, covering how to build a web API with ASP.NET Core and .NET 8.

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
- [IntelliCode for C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.vscodeintellicode-csharp) (Microsoft)
- [.NET Install Tool](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.vscode-dotnet-runtime) (Microsoft)
- [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) (Huachao Mao)
- [SQLite](https://marketplace.visualstudio.com/items?itemName=alexcvzz.vscode-sqlite) (alexcvzz)

Installing C# Dev Kit also installs its dependencies, the [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) extension and the .NET Install Tool.

## .NET CLI commands

| Command | What it does |
| --- | --- |
| `dotnet --version` | Shows the .NET SDK version in use |
| `dotnet --info` | Shows details on installed SDKs, runtimes, and environment |
| `dotnet new <template>` | Creates a new project from a template |
| `dotnet new list` | Lists the project templates available in the SDK |

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
