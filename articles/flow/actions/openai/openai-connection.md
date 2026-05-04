# OpenAI Connection

The connection is part of the configuration of [OpenAI](https://platform.openai.com/docs) actions and it is required in most cases.

<br/>

## Creating a New Connection

When adding an OpenAI action (such as Agent Model or Chat Completion), you can select an **existing OpenAI connection** or **create a new one**:

1. Click on an **OpenAI** action in order to open the Properties panel.
2. Click on the Connection field in order to select or create a connection. In the popup that shows up click on the **New connection** button.
3. Fill out the required connection fields.

<br/>

## Properties

| Name       | Description |
|-------------|-------------|
| Name    | A custom label for the connection. This name will be shown in dropdowns when selecting the connection in actions. |
| API Key | Your personal or organizational [OpenAI API key](https://platform.openai.com/account/api-keys). This key authorizes requests from Flow to OpenAI's endpoints. |

**Important:**  
- API keys can be created and managed in the [OpenAI dashboard](https://platform.openai.com/account/api-keys).  
- Keep your key private – it grants access to your OpenAI usage quota and billing.

<br/>

## Related documentation

- [OpenAI API Reference](https://platform.openai.com/docs/api-reference/introduction)
- [Using OpenAI in Flow](https://docs.profitbase.com/articles/flow/actions/openai/text-completion.html)