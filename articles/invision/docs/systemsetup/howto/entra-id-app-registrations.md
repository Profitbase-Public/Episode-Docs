# Create Entra ID App Registrations

## What Profitbase needs back

In order to complete the setup on our side we need the following sent back to us

- Tenant ID
- InVision Client App ID
- InVision Client Secret (make sure it's the Secret value and not the ID, it should not be a GUID)
- Expiry Date for the Client Secret
- InVision Designer App ID
- API Scope from the Expose API Page. Need the full url including api:// and the /name at the end. 

## Hypergene InVision Client App Registration

- Login to the Azure Portal and Create a new App Registration
![Create-App-Registration](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/EntraID-Create-AppRegistration.png)
- Give the App Registration a name, we suggest Hypergene InVision Client.
  - Make sure you change the Redirect URI to Web and supply the redirect URI that Profitbase provided.
![Profitbase-InVision-Client](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionClient.png)
- Save the App ID
- Click Add a certificate or secret
![Profitbase-InVision-Client-Secret](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionClientSecret.png)
- Give the secret a name and set the expiry lenght
![Profitbase-InVision-Client-Secret-Details](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionCLientSecretDetails.png)
- Save the Secret Value
![Profitbase-InVision-Client-Secret-Copy](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionClientSecretCopy.png)
- Click Add Client Permission
![Profitbase-InVision-Client-Add-Permissions](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionClientAddPermission.png)
- Select Application permissions and Add the following
  - Directory.Read.All
  - Group.Read.All
  - User.Read.All
![Profitbase-InVision-Client-Add-Permissions-App](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionClientAddPermissions.png)
- When finished it should look like this, click Grant Admin Consent
![Profitbase-InVision-Client-Grant-Consent](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionClientGrantConsent.png)
- Go to Expose API and create an API record for InVision
![Profitbase-InVision-Client-Expose-API](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionClientExposeAPI.png)
- Go to Authentication make sure the two checkboxes are enabled
  - Access tokens
  - ID tokens
![Profitbase-InVision-Client-Authentication](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseClientAuthentication.png)
- Go to Manifest and change the following two propeties
  - groupMembershipClaims set to "None"
  - isFallbackPublicClient set to false
  - Under api set requestedAccessTokenVersion to 2


## Hypergene InVision Designer App Registration

- Create a new App Registration
  - Name: InVision Designer (This can be set to anything)
  - Make sure you change the type to public client and set the reply URL to exactly: https://invisiondesigner
![Profitbase-InVision-Designer](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionDesigner.png)
- Go to API Permissions, select APIs that my Organization uses and find Hypergene InVision Client 
![Profitbase-InVision-Designer-API](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseInVisionDesignerAPIPermissions.png)
- Click grant consent
![Profitbase-InVision-Designer-Consent](https://profitbasedocs.blob.core.windows.net/images/invision/docs/systemsetup/howto/ProfitbaseDesignerConsent.png)
