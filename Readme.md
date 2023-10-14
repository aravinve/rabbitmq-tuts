# RabbitMQ Tuts

RabbitMQ is a message broker: it accepts and forwards messages. 

You can think about it as a post office: when you put the mail that you want posting in a post box, you can be sure that the letter carrier will eventually deliver the mail to your recipient.

A program that sends messages is a producer.

A queue is the name for the post box in RabbitMQ.

A consumer is a program that mostly waits to receive messages

Declaring a queue is idempotent - it will only be created if it doesn't exist already.