---
title: "KCS - VS Code on Ubuntu 24.04: Opening PartsUnlimited solution requires Mono msbuild and NuGet"
---

# KCS - VS Code on Ubuntu 24.04: Opening PartsUnlimited solution requires Mono msbuild and NuGet

## Environment
- OS: Ubuntu 24.04
- IDE: VS Code
- Project: PartsUnlimited (generated via AzDevOpsDemoGenerator)
- Command used: `dotnet run --project src/ADOGenerator/ADOGenerator.csproj`

## Issue
When opening the PartsUnlimited `.sln` file in VS Code, the editor prompts for missing dependencies: `mono` (msbuild) and `nuget`. The solution fails to load properly without these tools.

## Cause
PartsUnlimited targets the classic .NET Framework (ASP.NET 4.5). On Linux, MSBuild and NuGet tooling for .NET Framework projects are provided via Mono and the NuGet client, not the modern `dotnet` SDK alone. The modern .NET SDK does not include support for classic .NET Framework projects on Linux.

## Resolution

1. **Install Mono (includes msbuild)**

   Follow the [Mono project instructions](https://www.mono-project.com/download/stable/#download-lin-fedora) for Ubuntu.  
   Verify installation:  
   ```bash
   msbuild -version
   ```

2. **Install NuGet client**

    Follow the [Install NuGet client tools](https://learn.microsoft.com/en-us/nuget/install-nuget-client-tools?tabs=macos) for linux.
    Verify installation:
    ```bash
    nuget help
    ```

3. **Restore NuGet packages**

    From the projects root directory, run:
    ```bash
    nuget restore
    ```

4. ***Reload the solution in VS Code***

    Close and reopen VS Code, or restart the C# extension. OmniSharp should detect msbuild and load the solution.

## Verification
- The solution loads in VS Code without dependency prompts.
- IntelliSense and code navigation work for C# files.
- `Build → Run Build Task` or `msbuild` commands complete without errors.
- NuGet packages are restored without warnings.

## References
- [Configure and run the AzDevOps Demo Generator](https://learn.microsoft.com/en-us/azure/devops/demo-gen/configure?toc=%2Fazure%2Fdevops%2Fdemo-gen%2Ftoc.json&view=azure-devops)
- [VS Code C# and Mono Issue](https://github.com/dotnet/vscode-csharp/issues/5476)
- [NuGet package restore troubleshooting](https://learn.microsoft.com/en-us/nuget/Consume-Packages/Package-restore-troubleshooting#this-project-references-nuget-packages-that-are-missing-on-this-computer)
