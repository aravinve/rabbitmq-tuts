# RabbitMQ Tuts

RabbitMQ is a message broker: it accepts and forwards messages. 

You can think about it as a post office: when you put the mail that you want posting in a post box, you can be sure that the letter carrier will eventually deliver the mail to your recipient.

- A program that sends messages is a producer.

- A queue is the name for the post box in RabbitMQ.

- A consumer is a program that mostly waits to receive messages

Declaring a queue is idempotent - it will only be created if it doesn't exist already.

### Setting up Work Queues

In this example main idea is: Deliver the message to exactly one worker.

The main idea behind Work Queues (aka: Task Queues) is to avoid doing a resource-intensive task immediately and having to wait for it to complete.

Instead we schedule the task to be done later. 

1. We encapsulate a task as a message and send it to a queue. 

2. A worker process running in the background will pop the tasks and eventually execute the job. 

3. When you run many workers the tasks will be shared between them.


By default, RabbitMQ will send each message to the next consumer, in sequence. - RoundRobin

An ack(nowledgement) is sent back by the consumer to tell RabbitMQ that a particular message has been received, processed and that RabbitMQ is free to delete it.

- If a consumer dies (its channel is closed, connection is closed, or TCP connection is lost) without sending an ack, RabbitMQ will understand that a message wasn't processed fully and will re-queue it.

- If there are other consumers online at the same time, it will then quickly redeliver it to another consumer.


NOTE:
When RabbitMQ quits or crashes it will forget the queues and messages unless you tell it not to.
- Two things are required to make sure that messages aren't lost: we need to mark both the queue and messages as durable.
- RabbitMQ doesn't allow you to redefine an existing queue with different parameters and will return an error to any program that tries to do that

### Setting up Publish/Subscribe

In this example main idea is: Deliver the message to multiple consumers.

- A producer is a user application that sends messages.
- A queue is a buffer that stores messages.
- A consumer is a user application that receives messages.

The core idea in the messaging model in RabbitMQ is that the producer never sends any messages directly to a queue. --> Instead, the producer can only send messages to an exchange. 

An exchange is a very simple thing. On one side it receives messages from producers and the other side it pushes them to queues. 
> The exchange must know exactly what to do with a message it receives.

NOTE: There will be always one default exchange in RMQ - string.Empty ("")

Exchange Types: direct, topic, headers, fanout

fanout - Broadcasts all the messages it receives to all the queues it knows about.

We need to tell the exchange to send messages to our queue. That relationship between exchange and a queue is called a binding. `QueueBind` - the queue is interested in messages from this exchange

### Routing

Bindings can take an extra routingKey parameter.

In Publisher Side -> Call it as binding key
In Consumer Side -> Call it as routing key
But both are same

Binding key depends on the exchange type!!

NOTE: Fanout exchanges ignore the routingKey parameter.

`direct` exchange = a message goes to the queues whose binding key exactly matches the routing key of the message.