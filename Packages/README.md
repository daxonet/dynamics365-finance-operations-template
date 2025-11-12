# Packages

This folder stores Microsoft-provided packages.

---

## 1. Download `nuget.exe`

Download the latest version of [nuget.exe](https://www.nuget.org/downloads) and save it to the `Packages` folder.

You may rename it to `nuget.exe` if the downloaded file has a version suffix (e.g., `nuget-6.9.1.exe`).

---

## 2. Install Azure Artifacts Credential Provider

To support interactive login for Azure DevOps NuGet feeds, run the following PowerShell script:

```powershell
iex "& { $(irm https://aka.ms/install-artifacts-credprovider.ps1) } -AddNetfx"
```

> 🔗 Official Reference: [Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider#download-and-install)

---

## 3. Download NuGet Packages from LCS Shared Asset Library

Retrieve required NuGet packages from the **Shared Asset Library** in [Microsoft Dynamics 365 Lifecycle Services (LCS)](https://learn.microsoft.com/en-us/dynamics365/fin-ops-core/dev-itpro/dev-tools/hosted-build-automation).

- Microsoft.Dynamics.AX.ApplicationSuite.DevALM.BuildXpp.nupkg
- Microsoft.Dynamics.AX.Application1.DevALM.BuildXpp.nupkg
- Microsoft.Dynamics.AX.Application2.DevALM.BuildXpp.nupkg
- Microsoft.Dynamics.AX.Platform.CompilerPackage.nupkg
- Microsoft.Dynamics.AX.Platform.DevALM.BuildXpp.nupkg

Ensure the packages are placed into the `Packages` folder.

---

## 4. Push package to DevOps

nuget push "*.nupkg" -ConfigFile "..\Metadata\nuget.config" -Source "Dynamics365"  -ApiKey az

---

## 5. Update `packages.config`

Edit the `packages.config` file to include required NuGet package references. Example entry:

```xml
<package id="Microsoft.Dynamics.AX.Application.DevALM.BuildXpp" version="10.0.X.0" targetFramework="net40" />
```

Make sure the package names and versions match those downloaded.

---

## 6. Clean Up the `Packages` Folder

Delete all `.nupkg` files in Packages folder

---
