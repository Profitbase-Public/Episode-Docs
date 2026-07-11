# What is a Flow Package ?
A Flow Package serves as a container for a collection of Flows, allowing users to bundle Flows with their dependent resources ([Workspace Objects](../workspaces/workspace-objects.md) and [Workspace Variables](../workspaces/workspace-variables.md)). The package can be exported to a file and imported into another Workspace. 

![Packages Overview](../../../images/packages-overview.png)

<br/>

## Create 
To create a Flow Package press the Create button under the Package tab in a Workspace and fill out the properties in the appearing dialog. 

![Packages Create](../../../images/packages-create.png)

<br/>

### Properties

| Name                     | Required | Description                 |
| ------------------------ | -------- | --------------------------- |
| Name                     | Required | The name of the package. This will be displayed in the lists where the package is referenced. When an package export is made, this name together with the version will be used to give the export file its name. |
| Version                  | Required | The version of the package, which must follow the x.y.z format, where x, y, and z are digits between 0 and 999. The z (patch version) is optional.|
| Author                   | Required | The name of the creator of the package.           |
| Documentation(URL)       | Optional | A URL to the package documentation. |
| Package Icon             | Optional | A custom package icon. |
| Preferred Environment    | Required | The Preferred Environment is the environment from which a Flow is selected during the package export process. If the Flow is not available in that environment, the export will default to the Development environment.|
| Description              | Optional | Additional notes or comments about the Package.|

<br/>

## Edit 

To modify the properties of an existing Package, use the Edit button for the selected Package. To update the collection of Flows within the package, use the Edit button in the 'Flows in Package' section. Remember to update the version number if you change the content of the package before exporting it.

![Packages Edit](../../../images/packages-edit.png)

<br/>

## Install 
Installing a Package is done under the "Installed Packages" tab in a Workspace. 
Packages can be installed from two sources:
- The Hypergene Store; Official and certified Packages from Hypergene, typically ERP system integrations.
- Files; Custom or partner authored packages.

A package cannot be installed in the same Workspace where it was created.

![Packages Install](/images/flow/packages-install.png)

If the selected Package was created from an older version of Flow, it will automatically be upgraded to the current Flow version when installed.

<br/>

## Upgrade 
- To upgrade a Package that was installed from file, press `Ugrade from file` in the `Installed Packages` tab.
- To upgrade a Package that was installed from the Hypergene Store, in the `Installed Packages` tab, select / hover the package, open the `...` menu on the right and press `Upgrade from Store`.

<br/>

## Delete 
To delete a Package, to to the `Installed Packages` tab, select / hover the package, open the `...` menu on the right and press `Delete`.