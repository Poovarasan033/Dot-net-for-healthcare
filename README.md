# Dot-net-for-healthcare
Dot net for healthcare for specified look
<PropertyGroup>
    <RepositoryName>wpf</RepositoryName>
    <WindowsDesktopARM64Support>true</WindowsDesktopARM64Support>
    <TargetFramework>net7.0</TargetFramework>
    <TargetFrameworkVersion>7.0</TargetFrameworkVersion>
    <TargetFramework>net8.0</TargetFramework>
    <TargetFrameworkVersion>8.0</TargetFrameworkVersion>
  </PropertyGroup>
  
  **Normalize $(TestWpfArcadeSdkPath) by appending a '\' to it if one is missing**
  <PropertyGroup Condition="'$(TestWpfArcadeSdkPath)'!=''">
    <WpfArcadeSdkPath>$(TestWpfArcadeSdkPath)</WpfArcadeSdkPath>
    <WpfArcadeSdkPath Condition="!$(WpfArcadeSdkPath.EndsWith('\'))">$(TestWpfArcadeSdkPath)\</WpfArcadeSdkPath>
  </PropertyGroup>
  <PropertyGroup Condition="'$(TestWpfArcadeSdkPath)'=='' And Exists('$(MSBuildThisFileDirectory)eng\WpfArcadeSdk\')">
    <WpfArcadeSdkPath>$(MSBuildThisFileDirectory)eng\WpfArcadeSdk\</WpfArcadeSdkPath>
  </PropertyGroup>
  
**Select Sdk.props from test location or eng\WpfArcadeSdk\. If neither exists, then fall back to the use of one 
      obtained using MSBuild's Sdk resolver**
  <PropertyGroup Condition="Exists('$(WpfArcadeSdkPath)')">
    <WpfArcadeSdkProps>$(WpfArcadeSdkPath)Sdk\Sdk.props</WpfArcadeSdkProps>
    <WpfArcadeSdkTargets>$(WpfArcadeSdkPath)Sdk\Sdk.targets</WpfArcadeSdkTargets>
  </PropertyGroup>
  <Import Project="$(WpfArcadeSdkProps)"
          Condition="Exists('$(WpfArcadeSdkProps)') And Exists('$(WpfArcadeSdkTargets)')"/>
  <Import Sdk="Microsoft.DotNet.Arcade.Wpf.Sdk"
          Project="Sdk.props"
  6 changes: 3 additions & 3 deletions6  
eng/WpfArcadeSdk/tools/SdkReferences.targets
@@ -202,10 +202,10 @@
                 Condition="Exists('$(PkgMicrosoft_Private_Winforms)\ref\$(TargetFramework)\%(MicrosoftPrivateWinFormsReference.Identity).dll')">
        <NuGetPackageId>Microsoft.Private.Winforms</NuGetPackageId>
      </Reference>
      <Reference Include="$(PkgMicrosoft_Private_Winforms)\ref\net6.0\%(MicrosoftPrivateWinFormsReference.Identity).dll"
      <Reference Include="$(PkgMicrosoft_Private_Winforms)\ref\net7.0\%(MicrosoftPrivateWinFormsReference.Identity).dll"
                 Condition="!Exists('$(PkgMicrosoft_Private_Winforms)\ref\$(TargetFramework)\%(MicrosoftPrivateWinFormsReference.Identity).dll')
                 And $(TargetFramework) == 'net7.0'
                 And Exists('$(PkgMicrosoft_Private_Winforms)\ref\net6.0\%(MicrosoftPrivateWinFormsReference.Identity).dll')">
                 And $(TargetFramework) == 'net8.0'
                 And Exists('$(PkgMicrosoft_Private_Winforms)\ref\net7.0\%(MicrosoftPrivateWinFormsReference.Identity).dll')">
        <NuGetPackageId>Microsoft.Private.Winforms</NuGetPackageId>
      </Reference>
    </ItemGroup>
  4 changes: 2 additions & 2 deletions4  
global.json
@@ -1,6 +1,6 @@
{
  "tools": {
    "dotnet": "8.0.100-alpha.1.22423.9",
    "dotnet": "8.0.100-alpha.1.22512.5",
    "runtimes": {
      "dotnet": [
        "2.1.7",
@@ -16,7 +16,7 @@
    "Microsoft.DotNet.Helix.Sdk": "8.0.0-beta.22520.1"
  },
  "sdk": {
    "version": "8.0.100-alpha.1.22423.9"
    "version": "8.0.100-alpha.1.22512.5"
  },
  "native-tools": {
    "strawberry-perl": "5.28.1.1-1",
  4 changes: 2 additions & 2 deletions4  
src/Microsoft.DotNet.Wpf/src/PresentationBuildTasks/PresentationBuildTasks.csproj
@@ -1,6 +1,6 @@
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFrameworks>net472;net7.0</TargetFrameworks>
    <TargetFrameworks>net472;net8.0</TargetFrameworks>
    <EnableDefaultItems>false</EnableDefaultItems>
    <EnablePInvokeAnalyzer>true</EnablePInvokeAnalyzer>

@@ -60,7 +60,7 @@
    <PackagingAssemblyContent Include="System.Reflection.MetadataLoadContext.dll" />
    <PackagingAssemblyContent Include="System.Runtime.CompilerServices.Unsafe.dll" />
  </ItemGroup>
  <ItemGroup Condition="'$(TargetFramework)'=='net7.0'">
  <ItemGroup Condition="'$(TargetFramework)'=='net8.0'">
    <PackagingAssemblyContent Include="PresentationBuildTasks.dll" />
    <PackagingAssemblyContent Include="System.Reflection.MetadataLoadContext.dll" />
  </ItemGroup>
