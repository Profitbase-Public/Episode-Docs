# RabbitMQ overview

Flow includes built-in support for publishing messages to a [RabbitMQ](https://www.rabbitmq.com/) message broker through the [Publish Message](./publish-message.md) action. Use it to integrate Flow with systems that consume messages from RabbitMQ queues or topics — for example, a scheduled flow reading overdue customer payments from a database and publishing each one as a message for downstream processing.

The action accepts a RabbitMQ connection (configured when the action is added to a flow), the name of an [Exchange](https://www.rabbitmq.com/docs/exchanges) (Direct, Fanout, Topic, or Headers) and a Routing Key — together these determine how RabbitMQ routes the message to one or more queues. The action returns `true` or `false` depending on whether the message was published successfully.

The category currently covers publishing only — Flow does not consume messages from RabbitMQ in this category.
