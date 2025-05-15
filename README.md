## a. How much data will your publisher program send to the message broker in one run?

In the `main()` function, the publisher sends **five messages**, each being a `UserCreatedEventMessage` struct, containing two `String` fields: `user_id` and `user_name`.

Each message will be serialized using Borsh (a binary serialization format). The size of each serialized message will be approximately:

- 4 bytes for the length of `user_id` + length of `user_id` string (in bytes)
- 4 bytes for the length of `user_name` + length of `user_name` string (in bytes)

For the example data, the calculation for each message:
- `user_id` like `"1"` to `"5"` is 1 character = 1 byte (UTF-8)
- `user_name` like `"129500004y-Amir"` is 16–18 characters = 16–18 bytes (UTF-8), varies by name

So, for one message (taking the longest `user_name`, e.g. `"129500004y-Emir"` = 16 bytes):

- `user_id` = 4 (length) + 1 = 5 bytes
- `user_name` = 4 (length) + 16 = 20 bytes
- **Total per message ≈ 25 bytes**

For 5 messages:  
**Total ≈ 5 × 25 bytes = 125 bytes**

*Note: Actual size may vary slightly due to serialization overhead, but it will be close to this value.*

---

## b. The URL `amqp://guest:guest@localhost:5672` is the same as in the subscriber program. What does it mean?


**This means both the publisher and subscriber are connecting to the same local RabbitMQ (or other AMQP broker) instance, using the same credentials and port, so they are communicating over the same message queue.**

## Screenshot of Running RabbitMQ
![Running RabbitMQ](running_rabbitmq.png)

## Screenshot of sending and processing event
The left side is subscriber console, the right side is publisher console. When I run publisher, it will send 5 events to the message broker as shown in the subscriber console that processed and captured it
![Send Process](send_process.png)

## Monitoring chart based on publisher.
The spikes capturing the message rates of the event sent by the publisher. There are 2 spikes, meaning in the last minute, there are two separate range of time in which the publisher sent events.
![Monitor Chart](monitor_chart.png)

