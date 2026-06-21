# Function

Write C# code for custom business logic.


![Function Doc](../../../../images/flow/functionDoc.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow retrieves a [sitemap](../../actions/http/get-sitemap.md) from a specified URL, [validates](if.md) its structure and content, and [throws an exception](throw-exception.md) if the validation fails. You will find a detailed description of this flow [here.](flow-getSitemap.md)

<br/>

## Properties

| Name               | Required | Description                                                   |
|--------------------|-----------|---------------------------------------------------------------|
| Title              | No | The title or name of the action.                              |
| Function Name      | Yes | The name of the function being defined.                       |
| Function Definition | Yes | The logic or implementation of the function.                 |
| Description        | No | Additional notes or comments about the action or configuration. |

<br/>

## Returns

The action returns the result of executing your custom C# function, which can then be used further in the Flow (for example, stored in a variable, evaluated in a condition, or passed to subsequent actions).



