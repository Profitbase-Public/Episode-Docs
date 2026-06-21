# While (loop)

Runs a loop while its condition evaluates to `true`.

When the condition no longer evaluates to `true`, execution continues from the `Continue` port.

![image](../../../../images/flow/while.png)


**Example** ![Example](../../../../images/strz.jpg)  
This flow uses the While action to repeatedly execute a set of actions as long as a specified condition evaluates to true. The condition is evaluated before each iteration, and the loop stops automatically once the condition is no longer met. This is typically used for polling, retry logic, or iterative processing where the number of repetitions is not known in advance.

| Name                            | Required   | Description                                           |
|---------------------------------|-------------|-------------------------------------------------------|
| Title                 | No | Name of the loop.                       |
| Condition                  | Yes | Specifies the condition that must be satisfied in order to keep the loop running. The condition must evaluate to either a boolean `true` or `false`. When the condition no longer evaluates to true, the loop terminates.     |
| Maximum number of iterations                  | No | Sets a limit on the number of iterations the loop can perform before termination. The default value is 10 000. If set to 0 or blank, the loop will have no hard limit and runs until the `Condition` evaluates to `false`. You can use this property as a safeguard to prevent the loop from running forever. |
| Description              | No | Additional notes or comments about the action or configuration.    |

#### Maximum number of iterations

This property prevents the loop from running indefinitely.  Setting a reasonable maximum is recommended to avoid potential infinite loops.

![Max While Iteration](../../../../images/flow/max-While-Iteration.png)