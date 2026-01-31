---
layout: default
title: Best practices for .csproj files
parent: .NET GitHub actions
grand_parent: CI & automation
---

# Managing .csproj files

## Version management

### DOs:
- Use exclusively the `.csproj`'s `Version` attribute to manage package, assembly, or any other versions.
- Use `<GenerateAssemblyInfo>true</GenerateAssemblyInfo>`

### DON'Ts:
- Don't use `PackageVersion`, `AssemblyVersion`, `AssemblyFileVersion`,`AssemblyInformationalVersion`, `VersionSuffix`, `VersionPrefix` (neither in the `.csproj` nor in the `AssemblyInfo.cs`)
- Don't use `GenerateAssemblyXYZ` attributes in `.csproj` (e.g. `GenerateAssemblyTitleAttribute`)
- Don't put any version or package information into `AssemblyInfo.cs`

To learn more about the purpose of the various attributes, read:
- https://andrewlock.net/version-vs-versionsuffix-vs-packageversion-what-do-they-all-mean/#fileversion

The `Version` attribute gets propagated to all other version numbers (unless overridden). In case you think you need more granular settings for your project, always consult with your team.

### `Version`
For .NET projects we recommend **not storing a version number anywhere in the source files**.

The version of a release is determined by a [tag version](https://help.github.com/en/articles/creating-releases) and promoted to the NuGet package through the combination of [`dotnet build -p:Version=1.2.3`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-build) and [`dotnet pack -p:PackageVersion=1.2.3`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-pack).

### Directory.build.props
This file lets you set shared properties for multiple `.csproj` files. It's automatically being picked up by MSBuild during the build. It's a good idea to use this file for attributes such as `<RepositoryUrl>`, `<IncludeSymbols>`, `<SymbolPackageFormat>`, or `<GenerateDocumentationFile>` that you typically want to manage in one place for the whole repository.

### AssemblyInfo.cs

In case you want to get rid of the `AssemblyInfo.cs` completely and the only thing stopping you are the `InternalsVisibleToAttribute`s for tests, then you can [move them to `.csproj` or `.props` file](https://stackoverflow.com/a/49978185/1332034):

```xml
<ItemGroup>
    <AssemblyAttribute Include="System.Runtime.CompilerServices.InternalsVisibleTo">
      <_Parameter1>$(MSBuildProjectName).Test</_Parameter1>
    </AssemblyAttribute>
</ItemGroup>
```

## SourceLink & Symbol publishing
We recommend enabling [SourceLink](https://github.com/dotnet/sourcelink/) for .NET projects. This is the recommended configuration:

```xml
<Project Sdk="Microsoft.NET.Sdk">
 <PropertyGroup>
    <PublishRepositoryUrl>true</PublishRepositoryUrl>
    <EmbedUntrackedSources>true</EmbedUntrackedSources>
    <IncludeSymbols>true</IncludeSymbols>
    <SymbolPackageFormat>snupkg</SymbolPackageFormat>
  </PropertyGroup>
  <ItemGroup>
      <PackageReference Include="Microsoft.SourceLink.GitHub" Version="1.0.0-*" PrivateAssets="All"/>
  </ItemGroup>
</Project>
```

## Documentation generation
Always use `<GenerateDocumentationFile>true</GenerateDocumentationFile>`. Don't use the `<DocumentationFile>` attribute if not necessary.

## Repository & package metadata
The following attributes are recommended:
```xml
<PropertyGroup>
  <Authors>{{ site.organization.name }}</Authors>
  <Product>{{ site.organization.name }}</Product>
  <Copyright>© 2024 {{ site.organization.name }}. All rights reserved.</Copyright>
  <Description>Your package description</Description>
  <PackageLicenseExpression>MIT</PackageLicenseExpression>
  <PackageProjectUrl>https://github.com/yourorg/your-project</PackageProjectUrl>
  <PackageIconUrl>https://github.com/yourorg/.github/blob/master/images/logo_nuget.png?raw=true</PackageIconUrl>
  <RepositoryUrl>https://github.com/yourorg/your-project.git</RepositoryUrl>
</PropertyGroup>
```

## Target framework

For all [class libraries](https://docs.microsoft.com/en-us/dotnet/standard/class-libraries) target NET Standard 2.0 and .NET 6.

```xml
<TargetFrameworks>netstandard2.0;net6.0</TargetFrameworks>
```

For other types of projects (i.e. test projects, console applications) target .NET 5 and .NET 6.

```xml
<TargetFrameworks>net5.0;net6.0</TargetFrameworks>
```
