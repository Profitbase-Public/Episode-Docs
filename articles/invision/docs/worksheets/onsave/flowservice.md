# Flow service

Use the FlowService to run a Flow when a Worksheet or Table is saved.

<br/>

```csharp
// Example: Execute a Flow by name in any of the On* methods of the OnSave handler 
protected override void OnEndSave(CudDataPackage data)
{   
    base.OnAfterSave(rows, operationType);

    // Execute by name with no arguments
    this.FlowService.Execute("Flow name");

   // Execute a flow by its name and pass in a custom object as parameter (parameter must match the expected format in the called Flow)
    data.Params.TryGetValue("@CustomerId", out var customerId);
    this.FlowService.Execute("Flow name", new () { Data = JsonSerialize(new { CustomerId = customerId })});
}
```