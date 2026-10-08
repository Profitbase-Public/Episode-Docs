# Hypergene DevOps overview

Flow includes built-in support for managing Hypergene InVision deployments through the [AllSpark](https://allspark.profitbase.biz) API. Use these actions to automate provisioning workflows — for example, cloning a reference instance for a new customer, or cleaning up test instances when they are no longer needed.

To use any Hypergene DevOps action, you first need an [AllSpark connection](./connection.md) configured with API credentials.

<br/>

## Actions

[Get deployments](./get-invision-deployments.md) returns the Hypergene InVision deployments available through the connection, as a `List<Deployment>` with each deployment's ID, name and URI.

[Clone deployment](./clone-invision-deployment.md) creates a clone of an existing Hypergene InVision instance based on a provided `deploymentId`, returning a `CloneDeploymentResult` with the new deployment's ID.

[Delete deployment](./delete-invision-deployment.md) deletes an existing Hypergene InVision instance based on a provided `deploymentId`, returning a `DeleteDeploymentResult` indicating whether the operation succeeded.

<br/>

> [!IMPORTANT]
> The Clone and Delete actions cascade to connected Flow deployments. If the Hypergene InVision deployment has a Flow deployment connected to it, that Flow deployment is cloned alongside the InVision instance, or deleted together with it. The cloned Flow deployment is automatically connected to the cloned InVision instance.

<br/>

> [!NOTE]
> The Clone and Delete actions are long-running. The Flow containing them must be configured to run in long-running mode.