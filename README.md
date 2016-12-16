# SamzaLight
Building a new lightweigth Apache Samza distributed messaging framework.

Secondary keys and caching. What the have in commmon

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

Samza is made up of three layers:

A streaming layer.
An execution layer.
A processing layer.

Samza provides out of the box support for all three layers.

Streaming: Kafka
Execution: YARN
Processing: Samza API

Decoupling database layer with Samza.

What is Kappa Architecture?

Kappa Architecture is a software architecture pattern. Rather than using a relational DB like SQL or a key-value store like Cassandra, the canonical data store in a Kappa Architecture system is an append-only immutable log. From the log, data is streamed through a computational system and fed into auxiliary stores for serving.

Samza Architecture

The most basic element of Samza is a stream. The stream definition for Samza is much more rigid and heavyweight than you would expect from other stream processing systems. Other processing systems, such as Storm, tend to have very lightweight stream definitions to reduce latency, everything from, say, UDP to a straight-up TCP connection.

Samza goes the other direction. It wants its streams to be, for starters, partitions. It wants them to be ordered. If you read Message 3 and then Message 4, you are never going to get those inverted within a single partition. It also wants them to replayable, which means you should be able to go back to reread a message at a later date. It wants them to be fault-tolerant. If a host from Partition 1 disappears, it should still be readable on some other hosts. Also, the streams are usually infinite. Once you get to the end – say, Message 6 of Partition 0 – you would just try to reread the next message when it's available. It's not the case that you're finished.

This definition maps very well to Kafka, which LinkedIn uses as the streaming infrastructure for Samza.

There are many concepts to understand within Samza. In a gist, they are –

Streams – Samza processes streams. A stream is composed of immutable messages of a similar type or category. The actual implementation can be provided via a messaging system such as Kafka (where each topic becomes a Samza Stream) or a database (table) or even Hadoop (a directory of files in HDFS)
Things like message ordering, batching are handled via streams.

Jobs – a Samza job is code that performs logical transformation on a set of input streams to append messages to a set of output streams
Partitions – For scalability, each stream is broken into one or more partitions. Each partition is a totally ordered sequence of messages.

Tasks – again for scalability, a job is distributed by breaking it into multiple tasks. The task consumes data from one partition for each of the job’s input streams.

Containers – whereas partitions and tasks are logical units of parallelism, containers are unit physical parallelism. Each container is a unix process (or linux cgroup) and runs one or more tasks.
TaskRunner – Taskrunner is Samza’s stream processing container. It manages startup, execution and shutdown of one or more StreamTask instances.

Checkpointing – Checkpointing is generally done to enable failure recovery. If a taskrunner goes down for some reason (hardware failure, for e.g.), when it comes back up, it should start consuming messages where it left off last – this is achieved via Checkpointing.
State management – Data that needs to be passed between processing of different messages can be called state – this can be something as simple as a keeping count or something a lot more complex. Samza allows tasks to maintain persistent, mutable, queryable state that is physically co-located with each task. The state is highly available: in the event of a task failure it will be restored when the task fails over to another machine.

This datastore is pluggable, but Samza comes with a key-value store out-of-the-box.

YARN (Yet Another Resource Manager) is Hadoop v2’s biggest improvement over v1 – it separates the Map-Reduce Job tracker from the resource management and enables Map-reduce alternatives to use the same resource manager. Samza utilizes YARN to do cluster management, tracking failures, etc.

Samza provides a YARN ApplicationMaster and a YARN job runner out of the box.




