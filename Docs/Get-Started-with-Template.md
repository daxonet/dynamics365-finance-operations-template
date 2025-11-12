# Get Started with Template

[[_TOC_]]

## Create new DevOps Organization

1. Sign in to [Azure DevOps].

1. Select **New organization**.

1. Enter the name for customer organization, select its hosting geography, and then select Continue.

1. Sign in to new organization (https://dev.azure.com/{Customer_Organization}).

1. Select gear icon **Organization settings**.

1. On **Policies** > **User policies** > **External guest access** toggle **on**.

1. On **Pipelines** > **Settings** > **Disable creation of classic release pipelines** toggle **off**.

> For more information refer [Create new DevOps organization](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization).

## Azure DevOps Install Extensions

1. Sign in to customer organization (https://dev.azure.com/{Customer_Organization}).

1. Select the shopping bag icon, and then select **Browse Marketplace**.

1. Find below extension and select **Get it free**

    | Name                                     | Description
    | ---------------------------------------- | -----------
    | Code Search                              | Code Search provides fast, flexible and accurate search across all your code.
    | Dynamics 365 Finance and Operation Tools | Azure DevOps pipeline tools for Dynamics 365 Finance and Operations.
    | Power Platform Build Tools               | Automate common build and deployment tasks related to Power Platform.

1. Select your organization from the dropdown menu, and then select **Install** to install the extension.

> For more information refer  [Install Extensions](https://docs.microsoft.com/en-us/azure/devops/marketplace/install-extension?view=azure-devops&tabs=browser) 

## Create new DevOps Project

1. Sign in to your organization (https://dev.azure.com/{Your_Organization}).

1. Select New project.

1. On the **New project** screen expand **Advanced** and choose the following values:

    - Name: *[ProjectName]-D365FO*
    - Visibility: *Private*
    - Work item process: *Basic*
    - Version control: *Gif* 

## Azure DevOps Parallelism Request

1. Open [Request of free build](https://aka.ms/azpipelines-parallelism-request)

1. On **Azure DevOps Parallelism Request** choose the following values:

    - What is your name? : *your name*
    - What is your email address? *your working email*
    - What is the name of your Azure DevOps Organization? *Url created previously*
    - Are you requesting a parallelism increase for Public or Private projects? *Private*
	
>  It can take several business days to process your free tier requests. During certain time periods, processing times might be longer.

## Clone Project Template

1. On **Repos** > **Files** > **Import a repository** select **Import**

1. On **Import a Git repository** creen choose the following values:


	- Clone URL: https://github.com/daxonet/dynamics365-finance-operations-template.git
	
1. Select **Import**


## Azure DevOps Create Artifacts feed

1. Sign in to your project (https://dev.azure.com/{Customer_Organization}/[ProjectName]-D365FO).

1. Select **Artifacts**, and then select **Create Feed**.

1. On the **Create new feed** screen choose the following values:

    - Name: *D365FO*
    - Visibility: *Member of your Azure Active Directory*
    - Upstream sources: *Yes*
    - Scope: *Project*

1. Select **Create**.

1. Select **Connect to Feed** > **NuGet.exe**

1. Copy the selected text and paste it into the Metadata\nuget.config file.

> For more information refer  [Create Artifacts feed](https://docs.microsoft.com/en-us/azure/devops/artifacts/concepts/feeds)

1. Folow instructions in [Pacakges folder](/packages/README.md) to upload pacakges from LCS to Artifacts.

## Adding submodelds


Below models incldued in Template:

| Url                                                   | Model                      | Description
| ----------------------------------------------------- | -------------------------- | --------------
| https://github.com/TrudAX/XppTools 			        | DEVCommon, DEVTools        | Helper objects and user X++ tools, with the additional application functionality
| https://github.com/daxonet/DAXCustomFeatureManagement | DAXCustomFeatureManagement | Manage custom logic enable or disable

1. Run below script to update submodelds

```
git submodule update --init --recursive
```

1. Copy below folder to /PackageLocalDirectory/

	- /Vendors/XppTools/DEVCommon
	- /Vendors/XppTools/DEVTools
	- /Vendors/DAXCustomFeatureManagement/Metadata/DAXCustomFeatureManagement


> Refer [Vendors folder](/Vendors/README.md)

## Rename model

Rename below to include customer suffix:

- PackagesLocalDirectory\DAXSolution
- PackagesLocalDirectory\DAXSolution\Descriptor\DAXSolution.xml

```
<?xml version="1.0" encoding="utf-8"?>
<AxModelInfo xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	<DisplayName>DAXONET Solution</DisplayName>
	<ModelModule>DAXSolution</ModelModule>
	<Name>DAXSolution</Name>
	<Publisher>DAXONET</Publisher>
</AxModelInfo>
```

## Update pacakges version

Update Metadata\packages.config to latest version.

## Update build-xpp

1. Copy **/build/build-xpp-sample.yml** to **/build/build-xpp.yml**

1. Edit **build-xpp.yml**

## Update template file

Update README.md

## Commit and push

```
git commit -m "Initialize from template"
git push origin master
```

## Schedule pipelines and release

1. Sign in to your project (https://dev.azure.com/{Customer_Organization}/[ProjectName]-D365FO).

1. Select **Pipelines**, and then select **Create Pipeline**.

1. Selectt **Azure Repos Git**.

1. Select repository.

1. Select **Existing Azure Pipelines YAML file**.

1. Select `/build/build-xpp.yml`.

1. Select **Save**.

## Azure DevOps Assign team access to Project

> For more information refer  [Add users team project](https://docs.microsoft.com/en-us/azure/devops/organizations/security/add-users-team-project)