# When a new email arrives

Runs the Flow when a new email arrives in a Microsoft 365 mailbox.

Use this action to run a Flow when a new mail arrives in a Microsoft 365 Outlook mailbox. If you don't already have one, you need to first create a [Microsoft 365 Outlook connection](../../actions/microsoft-365-outlook/outlook-connection.md) by logging on with an account having access to the mailbox.

<br/>

![Microsoft 365 Outlook When Email Arrives](/images/flow/microsoft-365-outlook-when-email-arrives.png)


**Example**![Example](/images/strz.jpg)  
This flow is triggered whenever a new email arrives in the connected Outlook mailbox. For each email attachment, it checks the file type and processes it accordingly:

- Excel attachments are opened and their contents saved into a database.

- Parquet attachments are opened and imported into a database.

- CSV attachments are opened and stored in a database.

- If the attachment type is unsupported, the flow continues without processing it.

<br/>

## Properties

<!--prettier-ignore-->
| Property            | Required | Description                                               |
|---------------------|-----------------------------------------------------------------------|
| Connection          | Yes       | Specifies the [Microsoft 365 Outlook connection](../../actions/microsoft-365-outlook/outlook-connection.md) that the trigger should use to check for new emails. |
| Polling interval    | Yes       | Specifies how often to check for new emails.              |
| Disabled            | No        | Specifies if the trigger should be disabled. You may want to disable the trigger in the Development and Test environment once you have published to Production. |
| Filter              | No        | Specifies optional filters to restrict which emails to receive. See detailed description below. | 
| Folder              | No        | Specifies the folder to monitor for new emails. If not specified, all folders are monitored. |
| Include attachments | No        | Specifies whether or not to fetch the contents of all attachments when the trigger runs. [Read more about this option below](#attachments). |
| Output name         | Yes       | The name of the output variable that represents the received mail message. You can change it from the default `mailMessage` to something that more clearly describes the received mail, for example `customerFeedbackMessage`. |

#### Filter

[!INCLUDE [filters](../../actions/microsoft-365-outlook/mail-filters-paragraph.md)]

## Attachments

If `Include attachments` is set to false (unchecked), only metadata about any attachments in the message is included when the trigger runs.
If this is the case, you need to use an additional action such as [Get attachment](../../actions/microsoft-365-outlook/get-attachment.md) or [For each attachment](../../actions/microsoft-365-outlook/foreach-attachment.md), to get the actual file contents of the attachments.  
If emails contains large attachments, and you don't need all of them when processing the messages, it is recommended to set the `Include attachments` to false, and instead selectively get attachments you need using the [Get attachment](../../actions/microsoft-365-outlook/get-attachment.md) action.
