# Publish message to RabbitMQ

This action publishes a string message to an [RabbitMq](https://www.rabbitmq.com/) message broker. Messages can be published to **Queues** or **Topics** dependent on how you configure name of the Exchange and/or routing key(s).

<br/>

![Rabbitmq Publish Message](../../../../images/flow/rabbitmq-publish-message.png)

**Example** ![Example](../../../../images/strz.jpg)  

This flow runs daily using a [schedule](../../triggers/schedule-trigger.md), [reads rows](../sql-server/for-each-row-from-query.md) of overdue customer payments from a table, sends each as a message to an RabbitMQ queue.

<br/>

## Properties

| Name           | Required | Description                                      |
|----------------|----------|--------------------------------------------------|
| Title          | No | A descriptive label for the action.			   |
| Connection     | Yes | Select the [RabbitMQ connection](connection.md). |
| Exchange       | No | Name of the [Exchange](https://www.rabbitmq.com/docs/exchanges) to publish to. An exchange can e.g. be Direct, Fanout, Topic, or Headers. |
| Routing Key    | No | Select or enter the routing key. This is a message attribute used by an exchange to determine how to route a message to queues (e.g. the queue name). |
| Result variable name | No | The name of the variable in which the result will be stored. |
| Description    | No | Additional notes or comments about the action or configuration. |


## Returns

Either **true** or **false**, depending on whether the message was sent successfully.

