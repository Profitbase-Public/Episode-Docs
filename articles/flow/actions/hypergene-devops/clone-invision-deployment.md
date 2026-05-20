# Clone deployment

Clones an existing Hypergene InVision deployment using the [AllSpark](https://allspark.profitbase.biz) API. The action takes the `deploymentId` of the source instance and returns a result containing the `deploymentId` of the newly created clone.

![Clone Deployment](../../../../images/flow/hypergene-devops-clone-deployment.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow lets DevOps validate a package upgrade on a clone of a customer solution before touching production. It **clones** the customer's Hypergene InVision deployment from production, runs the [Upgrade package](../profitbase-invision/upgrade-package.md) action against the clone (which may take hours), and passes the result to a custom [Function](../built-in/function.md) action ("Validate upgrade") that checks the upgrade against business rules. The [If](../built-in/if.md) action then branches on the validation result — on success, the clone is [deleted](delete-invision-deployment.md) since it has served its purpose; on failure, an email is [sent](../microsoft-365-outlook/send-email.md) to the admin so they can investigate while the clone is preserved for inspection.

<br/>

## Properties

| Name           | Required | Description |
|----------------|----------|-------------|
| Title          | No  | A descriptive label for the action. |
| Connection     | Yes | The [AllSpark connection](./connection.md) used to authenticate against the AllSpark API. |
| Deployment Id  | Yes | The identifier of the Hypergene InVision deployment to clone. |
| Result variable name | No | The name of the variable in which the result will be stored. Defaults to `clone`. |
| Disabled       | No  | Indicates whether the action is disabled (true/false). |
| Description    | No  | Additional notes or comments about the action or configuration. |

<br/>

## Returns

A `CloneDeploymentResult` object with the following properties:

| Name | Type | Description |
|------|------|-------------|
| Success | bool | Indicates whether the clone operation succeeded. |
| DeploymentId | string | The identifier of the newly cloned Hypergene InVision deployment. |

<br/>

> [!NOTE]
> Cloning a deployment is a long-running operation. The Flow containing this action must be configured to run in long-running mode.

<br/>

> [!NOTE]
> If the source Hypergene InVision deployment has a connected Flow deployment, that Flow deployment is cloned as well and automatically connected to the new InVision clone.