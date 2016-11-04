# SamzaLight
Building a new lightweigth Apache Samza distributed messaging framework.

Samza is a framework that helps application developers write code to consume streams, process messages,
and produce derived output streams. In essence, a Samza job consists of a Kafka consumer, an event loop
that calls application code to process incoming messages, and a Kafka producer that sends output messages
back to Kafka. In addition, the framework provides packaging, cluster deployment (using Hadoop YARN),
automatically restarting failed processes, state management, metrics and monitoring.

It's a distributed stream processing framework that uses Kafka for messaging and state storage. Unlike many other stream processing frameworks, which involve jobs or tasks talking to each other, Samza instances only communicate with each other by reading and writing Kafka topics.

If you’ve ever written a map/reduce job, Samza’s programming model should be really familiar. It’s like a streaming version of map/reduce. Samza jobs consume messages from partitioned input streams and produce output messages to partitioned output streams. They can shuffle messages into different partitions while doing that, which gives you everything you need to do map/reduce.

The nice thing about this API is that it’s composable. You can have a single job, or you can have a series of related jobs where outputs from one job are used as inputs for the next. You can even have many jobs use the output from a single job.

Samza parallelism is partition-oriented. Kafka topics have a certain, user-configurable number of partitions each. You can tell Samza that you want a certain number of containers started up, and Samza will split up the Kafka partitions evenly across the containers you asked for by assigning one “task” per partition. The tasks in one container all execute interleaved on the same thread.

Samza and Spring Boot - a first approach

Building the samza package.

Databases are global, shared, mutable state. That’s the way it has been since the 1960s, and no amount of NoSQL has changed that. However, most self-respecting developers have got rid of mutable global variables in their code long ago. So why do we tolerate databases as they are?

A more promising model, used in some systems, is to think of a database as an always-growing collection of immutable facts. You can query it at some point in time — but that’s still old, imperative style thinking. A more fruitful approach is to take the streams of facts as they come in, and functionally process them in real-time.

At its core is a distributed, durable commit log, implemented by Apache Kafka. Layered on top are simple but powerful tools for joining streams and managing large amounts of data reliably.




