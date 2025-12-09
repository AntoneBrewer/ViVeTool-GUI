# How to Compile ViVeTool GUI

This document explains how to compile the ViVeTool-GUI executable (.exe) files.

## Prerequisites

- Windows 10 Version 2004 / Windows 11
- Visual Studio 2022 (or newer)
- .NET Framework 4.8 Developer Pack

## Option 1: Using GitHub Actions (Automated)

The easiest way to compile the .exe is to use the automated GitHub Actions workflow:

1. Navigate to the "Actions" tab in the GitHub repository
2. Click on the "Build ViVeTool-GUI" workflow
3. Click "Run workflow" button (you may need repository permissions)
4. If prompted, approve the workflow run (required for forks)
5. Wait for the workflow to complete (usually 2-5 minutes)
6. Download the compiled executables from the "Artifacts" section at the bottom of the workflow run

The workflow will produce:
- `ViVeTool-GUI-Release-x64` - Main ViVeTool GUI executable
- `ViVeTool-GUI-FeatureScanner-Release-x64` - Feature Scanner executable
- `ViVeTool-GUI-Complete-Build` - All build artifacts including DLLs

**Note:** The workflow uses a Windows runner and automatically handles all dependencies. If you're working on a fork, you may need to enable GitHub Actions in your repository settings and approve the workflow run.

## Option 2: Manual Compilation with Visual Studio

### Step 1: Install Dependencies

1. Open Visual Studio 2022
2. Open `ViVeTool_GUI.sln`
3. You'll encounter reference issues on first open

### Step 2: Fix Reference Issues

**For Albacore.ViVe:**
- The DLL is already included in the `lib` folder
- Or build it yourself from [ViVe repository](https://github.com/thebookisclosed/ViVe/tree/master/ViVe)

**For NuGet packages (AutoUpdater.NET, CrashReporter.NET, Newtonsoft.Json, WebView2):**
1. Open the Package Manager Console in Visual Studio
2. Click "Restore" when prompted
3. Or run: `nuget restore ViVeTool_GUI.sln`

**For Telerik libraries:**
- The required DLLs are included in the `lib\RCWF\2021.3.1109.40` folder
- Or install the [Telerik UI for WinForms Suite](https://www.telerik.com/login/ui-for-winforms) (login required)

**Note:** You can compile without Telerik UI for WinForms installed, but you won't be able to access the designer.

### Step 3: Build the Solution

1. Select "Release" configuration
2. Select "x64" platform
3. Build -> Build Solution (or press Ctrl+Shift+B)

### Step 4: Locate the Executables

After successful compilation, the executables will be located at:
- **Main Application:** `vivetool-gui/bin/x64/Release/ViVeTool_GUI.exe`
- **Feature Scanner:** `ViVeTool-GUI.FeatureScanner/bin/x64/Release/ViVeTool_GUI.FeatureScanner.exe`

## Option 3: Command Line Build

If you prefer to build from the command line:

```cmd
# Navigate to the repository folder
cd path\to\ViVeTool-GUI

# Restore NuGet packages
nuget restore ViVeTool_GUI.sln

# Build using MSBuild (adjust path to your MSBuild installation)
msbuild ViVeTool_GUI.sln /p:Configuration=Release /p:Platform=x64
```

The MSBuild executable is typically located at:
- `C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\msbuild.exe`

## Build Output Structure

```
vivetool-gui/bin/x64/Release/
├── ViVeTool_GUI.exe          # Main executable
├── *.dll                     # Required dependencies
├── Images/                   # Application images
└── ...

ViVeTool-GUI.FeatureScanner/bin/x64/Release/
├── ViVeTool_GUI.FeatureScanner.exe  # Feature scanner executable
└── *.dll                            # Required dependencies
```

## Troubleshooting

### Missing .NET Framework 4.8
**Error:** `The reference assemblies for .NETFramework,Version=v4.8 were not found`

**Solution:** Install the [.NET Framework 4.8 Developer Pack](https://dotnet.microsoft.com/download/dotnet-framework/net48)

### Cannot Find Telerik References
**Error:** `Could not resolve reference to Telerik.*`

**Solution:** The required Telerik DLLs are included in the repository at `lib\RCWF\2021.3.1109.40`. Make sure this folder exists and contains the DLL files.

### NuGet Package Restore Failed
**Error:** `Package restore failed`

**Solution:** 
1. Open Visual Studio
2. Tools -> NuGet Package Manager -> Package Manager Console
3. Run: `Update-Package -reinstall`

### Build Fails on Linux/macOS
**Issue:** .NET Framework 4.8 is Windows-only

**Solution:** This project cannot be built natively on Linux or macOS. Use:
- A Windows machine or VM
- The GitHub Actions workflow (which uses Windows runners)
- Wine with Mono (not recommended, limited support)

## Additional Notes

- **CrashReporter.vb:** It is highly recommended to change the crash reporter settings. See [C_Please_Change_CrashReporter.vb.md](vivetool-gui/C_Please_Change_CrashReporter.vb.md) for details.
- **Platform Target:** The solution is configured for x64 only. Both Debug and Release configurations are available.
- **Dependencies:** All required dependencies (except Telerik UI for design-time) are included in the repository.

## System Requirements for Compiled Application

- Windows 10 Build 18963 (Version 2004) or newer
- .NET Framework 4.8 Runtime

## Related Documentation

- [building.md](building.md) - Detailed building instructions and reference issues
- [README.md](README.md) - General project information
