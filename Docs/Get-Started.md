# Development Get Started

The **Development Get Started** document to ensure all development team members—including **internal**, **partner**, **customer in-house resources**, and **other vendor delivery teams** —are properly onboarded with the required access, tools, standards, and responsibilities to perform compliant, efficient, and upgrade-safe customizations within the Dynamics 365 Finance & Operations (D365FO) environment, following DAXONET governance policies.

[[_TOC_]]


## Model
| Name | Description | Url |
| ---- | ----------- | --- |
| DEVCommon | Helper objects | https://github.com/TrudAX/XppTools |
| DEVTools | It contains user X++ tools, with the additional application functionality | https://github.com/TrudAX/XppTools |
| DAXCustomFeatureManagement | Manage custom logic enable or disable | https://dev.azure.com/daxonet-dynamics/ |
| DAXIntegrationFramework | Electronic Data Interchange Framework | https://dev.azure.com/daxonet-dynamics/ |
| DAXFormLink | Manage form link for user reference. | https://dev.azure.com/daxonet-dynamics/ |
| DAXMYINVOICE | Malaysia E-invoice Solution | https://dev.azure.com/daxonet-dynamics/ |
| DAXSolutionXXX | DAXONET Solution for Customer; XXX = Customer Prefix |

## Development Guidelines

### Prefix
| Name |  Team |
| ---- |  ---- |
| DAX  | DAXONET |

### Custom module
| Name |  Team | Description |
| ---- |  ---- | ----------- |
| WMS| DAXONET |

### Before Start Development Change Request/Fixing

- [ ] Connect to correct DevOps and Branch.

- [ ] Get the latest version.

- [ ] No pending items. (shelve for other pending items not related for current change)

- [ ] Create project to correct model, Project naming : [Prefix]\_[Number]\_[Description]

## Naming Conversion
  
New objects name format for  [Package][Module][Name]

- Standard model
   - Ledger/Cust/Vend
   - Sales/Purch
   - Invent
   - Prod/BOM/Route/Req
   - Proj
   - etc.
- Custom model
   - WMS
   - MES
   - etc.
- Country *Suffix with ISO code*
   - MY
   - SG
   - TH

**Sample:**
| **Type**                          | **Format**                                                                  | **Sample**                                                  |
|----------------------------------|------------------------------------------------------------------------------|-------------------------------------------------------------|
| Extended Data Types              | [Prefix][Module][Name]                                                      | DAX_WMSOrderId                                              |
| Extended Data Type Extensions    | [Standard Name].[Model name]                                                | CustName.DAXSolutionXXX                                      |
| Table                            | [Prefix][Module][Name]                                                      | DAX_WMSOrderTable                                           |
| Table Extension                  | [Standard Name].[Model name]                                                | CustTable.DAXSolutionXXX                                     |
| Table Extension new field        | [Prefix][Name]                                                              | DAX_Name                                                    |
| Table Code Extension             | [Standard Name]_[Model name]_Extension                                      | CustTable_DAXSolutionXXX_Extension                           |
| Classes                          | [Prefix][Module][Name]                                                      | DAX_WMSOrderTableType                                       |
| Event Handle Class               | [Standard Name][Type]_[Model name]_EventHandler                             | CustTableForm_DAXSolutionXXX_EventHandler                    |
| Class Extensions                 | [Standard Name]_[Model name]_Extension                                      | SalesFormLetter_DAXSolutionXXX_Extension                     |
| Class Extensions new method      | [Prefix][Name]                                                              | DAX_Name                                                    |
| Form Code Extensions             | [Standard Name]Form_[Model name]_Extension                                  | CustTableForm_DAXSolutionXXX_Extension                       |
| Form Data Source Extensions      | [Standard Name]Form_[Data Source Name]_[Model name]_Extension               | CustTableForm_CustTable_DAXSolutionXXX_Extension             |

###  Feature Management

Create class implement *DAXCustomIFeature* for global or *DAXCustomIFeatureCompany* for by company

```csharp

internal final class {CustomFeature} implements DAXCustomIFeature 
{ 
    private static {CustomFeature}instance; 
    private void new() 
    { 
    } 

    private static void TypeNew() 
    { 
        instance = new {CustomFeature} (); 
    } 

    public static {CustomFeature}instance() 
    { 
        return {CustomFeature}::instance; 
    } 

    [Hookable(false)] 
    public str featureNumber() 
    { 
        return 'CR123'; 
    } 

    [Hookable(false)] 
    public str displayName() 
    { 
        return 'CR Subject'; 
    } 

    [Hookable(false)] 
    public int module() 
    { 
        return FeatureModuleV0::InventoryAndWarehouseManagement; 
    } 

    [Hookable(false)] 
    public boolean isEnabled() 
    { 
        return DAXCustomFeatureStateProvider::isFeatureEnabled({CustomFeature}::instance()); 
    } 
}

```

Sample Handler

```csharp
internal final class ({CustomFeature}Handler 
{ 
    [FormEventHandler(formStr(InventLocation), FormEventType::Initialized)] 
    public static void InventLocation_OnInitialized(xFormRun sender, FormEventArgs e) 
    { 
        boolean isEnable = ({CustomFeature}::instance().isEnabled();        

        sender.dataSource('InventLocation').object(fieldNum(InventLocation,DAX_InterCompanyCompanyId)).visible(isEnable);         
        sender.dataSource('InventLocation').object(fieldNum(InventLocation,DAX_InterCompanyInventLocationId)).visible(isEnable);         
        sender.dataSource('InventLocation').object(fieldNum(InventLocation,DAX_CustAccount)).visible(isEnable);         
        sender.dataSource('InventLocation').object(fieldNum(InventLocation,DAX_VendAccount)).visible(isEnable); 

        sender.design(0).controlName('DAX_Intercompany').visible(isEnable); 
    } 
}
```

Sample extension
```csharp
[ExtensionOf(classStr(PurchPackingSlipJournalPost))] 
final class DAX_PurchPackingSlipJournalPost_Extension
{ 
    protected void interCompanyPost() 
    { 
        if(({CustomFeature}::instance().isEnabled()) 
        { 
            DAX_InventTransferOrderPurchPost::post(this,vendPackingSlipJour); 
        } 
        next interCompanyPost(); 
    } 
}
```

Sample menu item show/hide
```csharp
[SubscribesTo(classstr(SysMenuNavigationObjectFactory), staticdelegatestr(SysMenuNavigationObjectFactory, checkAddSubMenuDelegate))] 
    public static void menuItemVisibilityHandler(SysDictMenu _rootMenu, SysDictMenu _subMenu, SysBoxedBoolean _subMenuVisibility) 
    { 
        if (_subMenu.isMenuItem()) 
        { 
            var metaElement = _subMenu.GetMenuItemMetaElement(); 
            if (metaElement != null) 
            { 
                if (metaElement.Name == menuItemDisplayStr(ExpenseVisibilitySetup)) 
                { 
                    _subMenuVisibility.value = ExpenseFeatureManagementShowHideMenuItems::isEnabled(); 
                } 
            } 
        } 
        else if (_subMenu.isTileReference()) 
        { 
            var metaElement = _subMenu.getTileMetaElement(); 
            if (metaElement != null) 
            { 
                if (metaElement.Name == tileStr(ExpenseWorkspace)) 
                { 
                    _subMenuVisibility.value = ExpenseFeatureManagementShowHideMenuItems::isEnabled(); 
                } 
            } 
        } 
    }
```


### Before Check-in
- [ ] Build model

- [ ] Error Free

- [ ] Review pending list

       Exclude objects that don't belong current change.
       Include current project and avoid missing objects.

- [ ] Check-in Comment: [Prefix]\_[Developer]\_[Number]_[Description]

## Source Control & Branching
Single Branch Strategy
- [ ] Implement feature management for all change request.
- [ ] Ensure all new logic control by feature management.
- [ ] Ensure test coverage for both enable/disable feature.
