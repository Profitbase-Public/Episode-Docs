# AllSpark connection

To use any of the [Hypergene DevOps actions](./overview.md), you need to either select an existing **AllSpark connection** or create a new one. The connection holds the AllSpark environment selection and the credentials used to authenticate, and is reused across actions in this category.

## Properties

| Name          | Required | Description |
|---------------|----------|-------------|
| Name          | No  | A custom name for the connection. This will be shown when selecting it in Flow actions. |
| Api           | Yes | The AllSpark environment to connect to. Select either **AllSpark Development** or **AllSpark Production**. |
| Tenant Id     | Yes | The tenant identifier used to authenticate against AllSpark. Use `Profitbase AS`. |
| Client Id     | Yes | The client identifier used to authenticate against AllSpark. Use `allspark-api`. |
| Client secret | Yes | The client secret used to authenticate against AllSpark. |

<br/>

## Configuration steps

1. Provide a **Name** for the connection.
2. Select the **Api** — either **AllSpark Development** or **AllSpark Production** — depending on the AllSpark environment you want to connect to.
3. Enter the **Tenant Id** (`Profitbase AS`), **Client Id** (`allspark-api`), and the **Client secret** issued for your AllSpark integration.
4. Click **Test connection** to verify the setup.
5. If the test is successful, click **Ok** to save the connection.

<!--![AllSpark Connection](../../../../images/flow/allspark-connection.png)-->