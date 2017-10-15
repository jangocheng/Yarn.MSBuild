Yarn.MSBuild
============

[![Travis][travis-badge]](https://travis-ci.org/natemcmaster/Yarn.MSBuild)
[![AppVeyor][appveyor-badge]](https://ci.appveyor.com/project/natemcmaster/yarn-msbuild)
[![NuGet][nuget-badge]](https://nuget.org/packages/Yarn.MSBuild)
[![MyGet][myget-badge]](https://www.myget.org/feed/natemcmaster/package/nuget/Yarn.MSBuild)

[travis-badge]: https://img.shields.io/travis/natemcmaster/Yarn.MSBuild.svg?style=flat-square&label=travis
[appveyor-badge]: https://img.shields.io/appveyor/ci/natemcmaster/yarn-msbuild.svg?style=flat-square&label=appveyor
[nuget-badge]: https://img.shields.io/nuget/v/Yarn.MSBuild.svg?style=flat-square&label=nuget
[myget-badge]: https://img.shields.io/www.myget/natemcmaster/vpre/Yarn.MSBuild.svg?style=flat-square&label=myget

An MSBuild task for running the Yarn package manager.

See [Yarn's Official Website](https://yarnpkg.com/en/) for more information about using Yarn.

# Installation

**Package Manager Console in Visual Studio**
```
PM> Install-Package Yarn.MSBuild
```

**.NET Core Command Line**
```
dotnet add package Yarn.MSBuild
```

**In csproj**
```xml
<ItemGroup>
  <PackageReference Include="Yarn.MSBuild" Version="1.1.0" />
</ItemGroup>
```

# Usage

This package installs yarn so you can use it from MSBuild without needing to install yarn globally.

### YarnBuildCommand

If you set the `YarnBuildCommand` property, the command will run automatically when you compile the application.

Example:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
    <YarnBuildCommand>run webpack</YarnBuildCommand>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
    <PackageReference Include="Yarn.MSBuild" Version="1.1.0" />
  </ItemGroup>
</Project>
```
```json
{
  "version": "1.0.0",
  "name": "myproject",
  "private": true,
  "scripts": {
    "webpack": "./node_modules/.bin/webpack"
  },
  "dependencies": {
    "react": "^16.0.0"
  },
  "devDependencies": {
    "awesome-typescript-loader": "^3.2.3",
    "typescript": "^2.5.3",
    "webpack": "^3.6.0"
  }
}
```

### Running yarn from a target

This package makes the `Yarn` task available for execution from your targets.

```xml
<Project>
  <Target Name="RunMyYarnCommands">
    <!-- defaults to "install" in the current directory using the bundled version of yarn. -->
    <Yarn />

    <!-- Specify the command -->
    <Yarn Command="run myscript" />

    <!-- Allow failures -->
    <Yarn Command="upgrade" IgnoreExitCode="true" />

    <!-- Change the directory where yarn is executed -->
    <Yarn Command="run test" WorkingDirectory="wwwroot/" />

    <!-- Set where NodeJS is installed -->
    <Yarn Command="run cmd" NodeJSExecutablePath="/opt/node8/bin/nodejs" />
  </Target>
</Project>
```

### Additional options

Yarn inherits all properties available on [ToolTask](https://docs.microsoft.com/en-us/dotnet/api/microsoft.build.utilities.tooltask)
which allows further fine-tuning, such as controlling the logging-level of stderr and stdout and the
process environment.

The `Yarn` task supports the following parameters

```
[Optional]
string Command                The arguments to pass to yarn.

[Optional]
string ExecutablePath         Where to find yarn (*nix) or yarn.cmd (Windows)

[Optional]
string NodeJsExecutablePath   Where to find node(js) (*nix) or node.cmd (Windows).
                              If not provided, node is expected to be in the PATH environment variable.

[Optional]
string WorkingDirectory       The directory in which to execute the yarn command

[Optional]
bool IgnoreExitCode           Don't create and error if the exit code is non-zero
```

Task outputs:
```
[Output]
int ExitCode                  Returns the exit code of the yarn process
```

# About

This is not an official Yarn project. See [LICENSE.txt](LICENSE.txt) and the [Third Party Notice](src/Yarn.MSBuild/third_party_notice.txt) for more details.
