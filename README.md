# SamzaLight
Building a new lightweigth Apache Samza distributed messaging framework.

Samza is a framework that helps application developers write code to consume streams, process messages,
and produce derived output streams. In essence, a Samza job consists of a Kafka consumer, an event loop
that calls application code to process incoming messages, and a Kafka producer that sends output messages
back to Kafka. In addition, the framework provides packaging, cluster deployment (using Hadoop YARN),
automatically restarting failed processes, state management, metrics and monitoring.




