
# InVision 4.1 change log

<br/>
## New features

#### User management and Permissions Workbook components

This feature enables creating Workbooks for administrators and power users to manage users, user groups and permissions to resources (data access and Workbooks) in the Solution. This means that administrators and power users no longer need to install or use the Designer (developer tool) to manage access to Workbooks and data.

![User management and Permissions](https://cutt.ly/mdMoTai)
<br/>
<br/>

#### Integrated PowerShell support

Built-in [PowerShell support](../docs/powershell.md) lets you create, manage and execute PowerShell scripts directly from InVision without having to deploy and execute them from disk on the server.

We have built-in support for PowerShell Gallery and NuGet, and if you need to use modules from other sources, you can easily do that as well.

Because the PowerShell scripts are executed by InVision and not an external process, you get access to InVision APIs and resources such as Directives and SQL Execution Services that makes it easy to use PowerShell as a tool to define custom business logic and automate processes in InVision.

![PowerShell](https://cutt.ly/jdMoRat)
<br/>
<br/>

#### New Homepage design with Grouping support

We have changed the design of the Homepage and added support for grouping of Workbooks so that Workbooks that belong to the same business processes can be grouped together. The Homepage now works better when there are many Workbooks, and it scales better to different screen sizes and form factors.

![Homepage](https://cutt.ly/idMoW1V)
<br/>
<br/>

#### Workbook Quick Navigation Menu

We added a (sidebar) navigation menu to Workbooks so that you can quickly navigate between Workbooks without having to go to the Homepage. The grouping, captions and icons in the menu comes from the Homepage configuration.  
InVision 5 will have support for defining Workbook Menus that has their own configurations instead of relying on the Homepage configuration.

![Workbook menu](https://cutt.ly/BdMoQpy)
<br/>
<br/>

#### Homepage configuration included in Package

The Homepage configuration is now included as part of Packages, so that when business modules are deployed, the original Homepage configuration from the export is restored at the deployment site.
<br/>
<br/>

#### Custom images are included in Package Deployment

We now include custom images in Packages so that business modules can be restored without having to manually import custom images afterwards.
<br/>
<br/>

#### Form Schema – support for DataChangeHandler to listen for changes to objects in arrays (SetModels)

Previously, if you wanted to listen for changes to specific properties in array items, for example array items in a custom Model or items in a in a SetModel, there was no easy way to do this. You could set up a listener binding {Binding Path:Model.\*}, but you could not distinguish between an array item and a property directly on the model so you did not know what object the change happened to – an array item or the Model itself.  
<br/>

**Example**  
The following example will listen for changes to all properties of all items the OrderLines array.

```xml
<DataChangeHandler Value="{Binding Path:Order.OrderLines.*.*}"> do something </DataChangeEventHandler>
```

<br/>
<br/>

#### New Directives

The following new user info directives has been added, and can be used wherever @CurrentUserName currently can be used (SQL Scripts, custom queries, SQL reports, etc)

- **@CurrentUserUID** returns the ID or the user account from the identity provider (Windows or Microsoft Entra ID) that is currently logged in. If the system is set up to use Windows AD, the SID will be returned. If the system is set up to use Microsoft Entra ID, the OID will be returned.
- **@CurrentUserFullName** returns the full name (first and last name) of the logged in user.
- **@CurrentUserEMail** returns the email of the currently logged in user.

**Example**

```sql
SELECT '@CurrentUserUID' AS UserUID, '@CurrentUserFullName' AS UserFullName, '@CurrentUserEMail' AS Email
```

<br/>
<br/>

## Misc changes

**Changed font**  
We have changed the font from Segoe UI to Roboto. This change should not cause any problems, but it’s always a good idea to check your custom screens to ensure they still look good.
<br/>

**New look for Popups**  
We have increased the height of the header bar and font size and added rounded corners to make the Popup look better.

![Popup style](https://cutt.ly/hdMoUHh)
<br/>
<br/>

## Enhancements

#### Spreadsheet (Worksheet, SQL Report and Table) - Raise events from custom cell renderer.

This feature makes it possible to raise events from custom [cell renderers](../docs/worksheets/columnproperties/cellrenderer.md) so that actions performed by users (for example clicking a button or link) can be acted upon in the Workbook.  
<br/>

#### Checkout Objects in the Designer

If you have a Solution Object open for edit when performing a checkout, we now automatically reload the configuration to ensure that you get the latest server version. 

> [!NOTE]
>  Any changes you have made before checkout is discarded.  
<br/>

#### Simplify Microsoft Entra ID redirect URI handling

[#183](https://cutt.ly/NdMocPj)
This enhancement makes it easier to set up the correct redirect URI in the Azure App Registration because the application will always use a lowercase redirect URI, not the casing entered by the user.
<br/>

#### Add collapse() API to Form Schema controls

[#137](https://cutt.ly/HdMov0y)
The [collapse](../docs/forms/formschemas/apis/controlproxyAPI.md) API makes it possible to hide a Form Schema control and **NOT** have it take up any space in the DOM. The current hide() API hides the control, but does not collapse the space occupied by the control.

<br/>

#### Db object references in Data Flow Activity arguments will be set during Package Deployment

[#123](https://cutt.ly/tdMob0V)
This feature enables Solution Developers to use Table parameters instead of Value parameters to pass database object names as parameters to Data Flow Items. By specifying a parameter as a Table parameter, Solution Developers can use the Table Picker instead of the @Object[…].DbObjectName directive to specify the database object name to pass as an argument.  
<br/>

#### Form Schema – added raiseCustomEvent() API

This API replaces the current raiseWorkbookComponentEvent(…) API which raises the Custom event on the Form Schema component. raiseWorkbookComponentEvent(…) is still supported, but should be considered obsolete. The reason for this change is not a functional one, but rather for naming consistency between the APIs in the Workbook – it makes sense that the [raiseCustomEvent(…)](../docs/forms/formschemas/apis/formrunAPI.md) raises the Custom event.  
<br/>

#### Form Schema – make it easier to style Label using the global styling system

The Form Schema Label had a default system style applied that was hard to override from a Stylesheet style because of a high CSS specificity. We changed the default CSS rule to make it easy to apply a custom CSS style from a StyleSheet instead of having to use the !important property or other “hacks” to get the CSS style to apply if it was defined outside the Form Schema CSS style.
<br/>
<br/>

## Bug fixes

[#193](https://cutt.ly/EdMeG3k) @Language directive is not evaluated in SQL Script

[#180](https://cutt.ly/RdMru2y) Form schema CSS classes registered multiple times

[#165](https://cutt.ly/edMroKs) Handle format strings with 0 decimals correctly

[#163](https://cutt.ly/2dMrp40) Linked Dimensions cannot be applied

[#162](https://cutt.ly/pdMraVR) Hover effect for Filter items

[#160](https://cutt.ly/AdMrfJt) Export spreadsheet with undefined name to Excel

[#159](https://cutt.ly/qdMiDxd) Spreadsheet filter did not display when hosted in popup

[#157](https://cutt.ly/vdMowj7) Form Schema Init Event runs multiple times

[#156](https://cutt.ly/RdMoeo2) Designer Microsoft Entra ID Login cache pr user

[#149](https://cutt.ly/UdMorgC) Dropdown text cropped

[#145](https://cutt.ly/gdMotse) Enable data binding for Form Schema Input Disabled property

[#144](https://cutt.ly/7dMoyMy) Form Schema Input control did allow entering 0 or negative values

[#141](https://cutt.ly/ldMouGB) Designer crash when trying to use a view that has not been materialized as a Filter source

[#132](https://cutt.ly/sdMoiAW) Duplicate summary rows in spreadsheets

[#131](https://cutt.ly/NdMopwD) Workbook Page Navigated To event is not raised if page is already open

[#127](https://cutt.ly/adMop4H) Designer crashes when Data Store with ghost references are opened

[#126](https://cutt.ly/YdMoaAd) Data Store reload fails when comments are enabled on columns

[#94](https://cutt.ly/5dMos3g) Spreadsheet validation warning is treated as validation error

[#29](https://cutt.ly/kdModH6) Typing letters into numeric spreadsheet cells causes freeze

[#17](https://cutt.ly/idMofux) Add column name validation to spreadsheet dropdown configuration

[#28](https://cutt.ly/adMof9s) Do not try to save Form Schema Models when no source or SaveQuery is defined

[#50](https://cutt.ly/sdMolli) Table List Data Modified event is not raised

[#51](https://cutt.ly/udMozLE) Issue with compiling ranked input tables when columns have been removed from referenced dimensions

**Internal:** Options dialog can be opened multiple times. This bug let the user open multiple instances of the Workbook Options dialog.

**Internal:** Listbox and Checkbox Select All and Clear buttons should affect the items visible in the list (from search criteria), not all items in the filter.

<br/>

### See Also

- [Change Log 2025.4](changelog25_4.md)
- [Change Log 2025.3](changelog25_3.md)
- [Change Log 2025.2](changelog25_2.md)
- [Change Log 2025.1](changelog25_1.md)
- [Change Log 2024.4](changelog24_4.md)
- [Change Log 2024.3](changelog24_3.md)
- [Change Log 2024.2](changelog24_2.md)
- [Change Log 2024.1](changelog24_1.md)
- [Change Log 2023.7](changelog23_7.md)
- [Change Log 2023.6](changelog23_6.md)
- [Change Log 2023.5](changelog23_5.md)
- [Change Log 2023.4](changelog23_4.md)
- [Change Log 2023.3](changelog23_3.md)
- [Change Log 2023.2](changelog23_2.md)
- [Change Log 2023.1](changelog23_1.md)
- [Change Log 2022.5](changelog22_5.md)
- [Change Log 2022.4](changelog22_4.md)
- [Change Log 2022.3 pt 2](changelog22_3_2.md)
- [Change Log 2022.3 pt 1](changelog22_3_1.md)
- [Change Log 2022.2](changelog22_2.md)
- [Change Log 2022.1](changelog22_1.md)
- [Change Log 5.2](changelog52.md)
- [Change Log 5.1](changelog51.md)
- [Change Log 5.0](changelog5.md)
- [Change Log 4.0](changelog40.md)
